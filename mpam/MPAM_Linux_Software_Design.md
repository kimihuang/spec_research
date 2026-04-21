# MPAM Linux 软件方案设计文档

> 场景：嵌入式/边缘计算，实时与非实时任务共存
> 需求：缓存分区、带宽分区、实时监控、动态策略、KVM 虚拟化支持
> 约束：基于 Arm IHI 0099B.a 规范，所有硬件交互必须通过规范定义的 MMIO/系统寄存器

---

## 目录

1. [整体架构](#1-整体架构)
2. [内核驱动层 (mpam.ko)](#2-内核驱动层)
3. [守护进程层 (mpamd)](#3-守护进程层)
4. [动态库层 (libmpam.so)](#4-动态库层)
5. [用户态应用](#5-用户态应用)
6. [虚拟化支持](#6-虚拟化支持)
7. [模块交互与关键数据流](#7-模块交互与关键数据流)
8. [可扩展性与解耦设计](#8-可扩展性与解耦设计)

---

## 1. 整体架构

### 1.1 四层架构

```mermaid
graph TB
    subgraph "用户态应用层"
        APP1["mpamctl<br/>管理工具"]
        APP2["mpam-top<br/>监控面板"]
        APP3["业务应用<br/>libmpam 客户端"]
    end

    subgraph "动态库层 (libmpam.so)"
        API["MPAM 抽象 API"]
        TAG["线程标记接口<br/>prctl/clone 扩展"]
        DBUS_C["D-Bus 客户端"]
    end

    subgraph "守护进程层 (mpamd)"
        CONF["配置管理器<br/>YAML 解析"]
        MGR["资源分区管理器"]
        MON["监控采集器"]
        POL["策略引擎<br/>插件化"]
        DBUS_S["D-Bus 服务端"]
        NOTIFY["Netlink 事件通知"]
    end

    subgraph "内核驱动层 (mpam.ko, 基于 mpam_devices.c)"
        ACPI["ACPI MPAM 解析<br/>MSC 发现 (platform_driver)"]
        OBJ["5层对象模型<br/>Class→Comp→vMSC→RIS→MSC"]
        MMIO["MMIO 寄存器操作<br/>smp_call_on_cpu + readl/writel"]
        CDEV["字符设备 + ioctl<br/>(扩展建议)"]
        SYSFS["sysfs 属性节点<br/>(扩展建议)"]
        IRQ["错误中断 (PPI/SPI)<br/>mpam_ppi/spi_handler"]
        REG["mpam_register_requestor()<br/>唯一 EXPORT_SYMBOL"]
        KVM_E["KVM ioctl 扩展<br/>(扩展建议)"]
    end

    subgraph "硬件"
        MSC_HW["MPAM MSC<br/>共享缓存/内存控制器"]
    end

    APP1 --> API
    APP2 --> API
    APP3 --> API
    API --> DBUS_C
    TAG --> SYREG
    DBUS_C <--> DBUS_S
    MGR --> CDEV
    MGR --> SYSFS
    MON --> SYSFS
    MON --> PERF
    POL --> MGR
    CONF --> MGR
    NOTIFY --> APP3
    CDEV --> OBJ
    SYSFS --> OBJ
    OBJ --> MMIO
    ACPI --> OBJ
    IRQ --> MMIO
    REG --> OBJ
    KVM_E --> OBJ
```

### 1.2 设计原则

| 原则 | 实现方式 |
|------|---------|
| **解耦** | 各层通过明确定义的接口通信，不直接访问其他层内部数据 |
| **可扩展** | 策略引擎插件化，资源类型抽象为统一接口 |
| **安全** | 驱动层做权限校验，PARTID 分配需通过守护进程 |
| **兼容** | sysfs/ioctl/D-Bus 多通道，支持不同集成场景 |
| **实时友好** | 驱动路径零拷贝，关键操作不走守护进程 |

---

## 2. 内核驱动层

> 本节基于 linux_mpam_drivers/mpam_devices.c (Arm Ltd., 2025, GPL-2.0) 源码分析，描述真实的上游 Linux MPAM 驱动架构。

### 2.1 五层对象模型

驱动的核心设计是 **5 层对象层级**，将物理硬件映射为可编程的逻辑抽象：

```mermaid
graph TB
    subgraph "Class (资源类) -- 全局唯一类型 + 层级"
        C1["Class: L3 Cache<br/>type=MPAM_CLASS_CACHE, level=3"]
        C2["Class: Memory<br/>type=MPAM_CLASS_MEMORY"]
    end

    subgraph "Component (组件) -- 同类资源的不同实例"
        COMP1["Component: L3 Slice 0<br/>comp_id=cache_id_0"]
        COMP2["Component: L3 Slice 1<br/>comp_id=cache_id_1"]
        COMP3["Component: MC 0<br/>comp_id=memory_node_0"]
    end

    subgraph "vMSC (虚拟 MSC) -- 同一 MSC 中控制相同资源的 RIS 组"
        V1["vMSC: MSC0+RIS0"]
        V2["vMSC: MSC1+RIS0"]
        V3["vMSC: MSC0+RIS0"]
    end

    subgraph "RIS (资源实例) -- MSC 内的物理寄存器集合"
        R1["RIS0 in MSC0"]
        R2["RIS0 in MSC1"]
    end

    subgraph "MSC (MPAM 系统组件) -- 物理设备, 共享 MMIO 基地址"
        MSC0["MSC #0<br/>pdev, mmio_base"]
        MSC1["MSC #1<br/>pdev, mmio_base"]
    end

    C1 --> COMP1
    C1 --> COMP2
    C2 --> COMP3
    COMP1 --> V1
    COMP2 --> V2
    COMP3 --> V3
    V1 --> R1
    V2 --> R2
    R1 --> MSC0
    R2 --> MSC1
```

**层级关系说明 (源码注释 mpam_devices.c:79-111)：**

| 层级 | 结构体 | 含义 | 示例 |
|------|--------|------|------|
| **Class** | `struct mpam_class` | 全局同一类型+层级的资源集合 | 所有 L3 缓存 |
| **Component** | `struct mpam_component` | 同类资源的不同物理实例 | L3 Slice 0, L3 Slice 1 |
| **vMSC** | `struct mpam_vmsc` | 同一 MSC 内控制相同资源的 RIS 分组 | MSC0 中控制缓存的 RIS 集合 |
| **RIS** | `struct mpam_msc_ris` | MSC 内的一个资源实例 (寄存器集合) | MSC0 RIS0 |
| **MSC** | `struct mpam_msc` | 物理设备, 共享 MMIO 基地址和中断 | 一个 L3 MSC 芯片 |

**关键规则：** vMSC 的特性是其所包含 RIS 的**并集**；Class 和 Component 的特性是所包含 vMSC 的**交集**（不匹配的特性被清除）。

### 2.2 驱动模块结构

```mermaid
graph TB
    subgraph "mpam_devices.c 核心模块 (2726 行)"
        INIT["模块初始化<br/>mpam_msc_driver_init()<br/>subsys_initcall"]
        PROBE["Platform Driver<br/>mpam_msc_drv_probe()"]

        subgraph "MSC 发现与注册"
            ACPI_P["ACPI 解析<br/>acpi_mpam_parse_resources()"]
            CPUHP["CPU Hotplug 发现<br/>mpam_discovery_cpu_online()"]
            HW_PROBE["硬件探测<br/>mpam_msc_hw_probe()"]
            RIS_PROBE["RIS 特性探测<br/>mpam_ris_hw_probe()"]
        end

        subgraph "对象模型管理"
            CLASS_M["Class 管理<br/>mpam_class_alloc/find/destroy()"]
            COMP_M["Component 管理"]
            VMS_M["vMSC 管理"]
            RIS_M["RIS 管理"]
            MERGE["特性合并<br/>mpam_enable_merge_features()"]
        end

        subgraph "寄存器访问"
            MMIO_RW["MMIO 读写<br/>__mpam_read_reg()<br/>__mpam_write_reg()<br/>readl/writel_relaxed()"]
            PART_SEL["PARTID 选择<br/>__mpam_part_sel()"]
            MON_SEL["监控器选择<br/>mon_sel_lock"]
            IPI["IPI 调度<br/>smp_call_on_cpu()"]
        end

        subgraph "配置管理"
            CFG_ARRAY["配置数组<br/>comp->cfg[partid_max+1]"]
            PROG["重新编程<br/>mpam_reprogram_ris_partid()"]
            APPLY["应用配置<br/>mpam_apply_config()"]
            RESET["重置配置<br/>mpam_reset_component_cfg()"]
        end

        subgraph "监控器"
            CSU_R["CSU 监控读取<br/>__ris_msmon_read()"]
            MBWU_R["MBWU 监控读取<br/>含长计数器(44/63位)"]
            MBWU_OVF["溢出修正<br/>mbwu_state.correction"]
            NRDY["NRDY 等待<br/>nrdy_usec 超时"]
        end

        subgraph "中断处理"
            IRQ_REG["中断注册<br/>PPI/SPI 支持"]
            IRQ_H["错误中断处理<br/>__mpam_irq_handler()"]
            ESR_CLR["ESR 清除<br/>先清高位再清低位"]
            BROKEN["禁用驱动<br/>schedule_work(mpam_broken_work)"]
        end

        subgraph "生命周期"
            ENABLE["mpam_enable()<br/>static_branch_enable()"]
            DISABLE["mpam_disable()<br/>static_branch_disable()"]
            CPU_ON["mpam_cpu_online()<br/>重新编程 MSC"]
            CPU_OFF["mpam_cpu_offline()<br/>保存 MBWU 状态<br/>重置配置"]
            GARBAGE["延迟释放<br/>synchronize_srcu + llist"]
        end

        subgraph "外部接口"
            REG_REQ["mpam_register_requestor()<br/>唯一的 EXPORT_SYMBOL"]
        end
    end

    INIT --> ACPI_P
    ACPI_P --> PROBE
    PROBE --> RIS_PROBE
    PROBE --> CPUHP
    CPUHP --> HW_PROBE
    HW_PROBE --> RIS_PROBE
    RIS_PROBE --> CLASS_M
    CLASS_M --> COMP_M
    COMP_M --> VMS_M
    VMS_M --> RIS_M
    PROG --> PART_SEL
    PART_SEL --> MMIO_RW
    APPLY --> CFG_ARRAY
    APPLY --> PROG
    CSU_R --> MON_SEL
    MBWU_R --> MON_SEL
    CSU_R --> MMIO_RW
    MBWU_R --> MMIO_RW
    MBWU_R --> IPI
    PROG --> IPI
    IRQ_H --> ESR_CLR
    IRQ_H --> BROKEN
    BROKEN --> DISABLE
    CPU_ON --> ENABLE
    CPU_OFF --> RESET
    ENABLE --> MERGE
    ENABLE --> IRQ_REG
    DISABLE --> RESET
    REG_REQ -.->|"resctrl 调用"| ENABLE
```

### 2.3 核心数据结构

```c
/* MSC (物理设备) -- 一个 platform_device 对应一个 */
struct mpam_msc {
    uint32_t            id;
    struct platform_device *pdev;
    void __iomem        *mapped_hwpage;       /* MMIO 基地址 (ioremap) */
    size_t              mapped_hwpage_sz;
    enum mpam_iface     iface;               /* MMIO / PCC */

    uint16_t            ris_max;             /* 最大 RIS 索引 */
    uint16_t            partid_max;          /* 此 MSC 支持的 PARTID 最大值 */
    uint8_t             pmg_max;             /* 此 MSC 支持的 PMG 最大值 */
    bool                has_extd_esr;        /* 64 位扩展 ESR */
    bool                probed;              /* 硬件探测完成标志 */

    cpumask_t           accessibility;       /* 可访问此 MSC 的 CPU 掩码 */
    atomic_t            online_refs;         /* 在线 CPU 引用计数 */
    uint32_t            nrdy_usec;           /* 固件提供的 NRDY 超时 (us) */

    /* 锁: 严格顺序 cfg_lock > part_sel_lock > mon_sel_lock > error_irq_lock */
    struct mutex        probe_lock;
    struct mutex        part_sel_lock;       /* 保护 PARTID_SEL 寄存器 */
    struct mutex        cfg_lock;            /* 保护配置写入序列 */
    struct mutex        error_irq_lock;
    struct lockdep_map  mon_sel_lock_dep_map;
    bool                in_reset_state;

    struct list_head_rcu ris;                /* 此 MSC 的所有 RIS */
    struct list_head_rcu all_msc_list;       /* 全局 MSC 链表 */
    struct mpam_garbage  garbage;
};

/* RIS (资源实例) -- MSC 内的物理寄存器集合 */
struct mpam_msc_ris {
    uint8_t             ris_idx;
    uint64_t            idr;                 /* MPAMF_IDR 缓存值 */
    struct mpam_props   props;               /* 此 RIS 的特性标志 */
    struct msmon_mbwu_state *mbwu_state;     /* MBWU 监控器状态数组 */
    cpumask_t           affinity;            /* 关联的 CPU 集合 */
    struct mpam_vmsc    *vmsc;
    struct list_head_rcu msc_list;
    struct list_head_rcu vmsc_list;
    struct mpam_garbage  garbage;
};

/* Component (组件) -- 同类资源的不同物理实例 */
struct mpam_component {
    int                 comp_id;             /* 缓存 ID 或内存节点 ID */
    cpumask_t           affinity;
    struct mpam_config  *cfg;                /* cfg[partid_max+1] 配置数组 */
    struct mpam_class   *class;
    struct list_head_rcu vmsc;
    struct list_head_rcu class_list;
    struct mpam_garbage  garbage;
};

/* Class (资源类) -- 全局类型+层级 */
struct mpam_class {
    uint8_t             level;               /* 缓存层级 */
    enum mpam_class_types type;              /* CACHE / MEMORY / UNKNOWN */
    struct mpam_props   props;               /* 合并后的特性 (交集) */
    uint32_t            nrdy_usec;           /* 所有 vMSC 中最大值 */
    struct ida          ida_csu_mon;
    struct ida          ida_mbwu_mon;
    struct list_head_rcu components;
    struct list_head_rcu classes_list;
    struct mpam_garbage  garbage;
};

/* 每分区的配置 (shadow copy) */
struct mpam_config {
    DECLARE_BITMAP(features, MPAM_FEATURE_LAST);
    uint32_t            cpbm;                /* CPOR 位图 (最大 32 位) */
    uint32_t            mbw_pbm;             /* MBWPBM 位图 (最大 32 位) */
    uint32_t            mbw_max;             /* MBWMAX */
    struct mpam_garbage  garbage;
};

/* 特性标志 (bitmap 操作) */
struct mpam_props {
    DECLARE_BITMAP(features, MPAM_FEATURE_LAST);
    uint16_t            cpbm_wd;             /* CPOR 位图宽度 */
    uint16_t            mbw_pbm_bits;        /* MBWPBM 位图宽度 */
    uint8_t             cmax_wd;             /* CCAP CMAX 定点小数位数 */
    uint8_t             cassoc_wd;           /* CASSOC 定点小数位数 */
    uint8_t             bwa_wd;              /* MBW A 位宽 */
    uint8_t             intpri_wd, dspri_wd; /* 优先级位宽 */
    uint16_t            num_csu_mon;         /* CSU 监控器数量 */
    uint16_t            num_mbwu_mon;        /* MBWU 监控器数量 */
};
```

### 2.4 驱动生命周期

```mermaid
flowchart TD
    START["subsys_initcall<br/>mpam_msc_driver_init()"] --> CHECK{"system_supports_mpam()?"}
    CHECK -->|"否"| EXIT["返回 -EOPNOTSUPP"]
    CHECK -->|"是"| COUNT["acpi_mpam_count_msc()"]
    COUNT --> REG["platform_driver_register()"]

    REG --> FW_PROBE["固件枚举 MSC<br/>为每个 MSC 调用 probe"]
    FW_PROBE --> DRV_PROBE["mpam_msc_drv_probe(pdev)"]
    DRV_PROBE --> MMIO["ioremap MMIO 基地址"]
    MMIO --> PARSERES["acpi_mpam_parse_resources(msc)"]
    PARSERES --> RIS_CREATE["创建 RIS 条目<br/>mpam_ris_create()"]
    RIS_CREATE --> CHK_ALL{"所有 MSC<br/>已 probe?"}
    CHK_ALL -->|"否"| CPUHP_D["注册 discovery cpuhp<br/>mpam_discovery_cpu_online()"]
    CPUHP_D --> WAIT_CPU["等待 CPU 上线"]
    WAIT_CPU --> HW_P["mpam_msc_hw_probe()"]
    HW_P --> CHK_ALL

    CHK_ALL -->|"是"| SCHED_ENABLE["schedule_work<br/>mpam_enable_work"]
    SCHED_ENABLE --> MPAM_ENABLE["mpam_enable()"]
    MPAM_ENABLE --> PUB["partid_max_published=true"]
    PUB --> MERGE["mpam_enable_merge_features()<br/>合并 RIS/vMSC/Component/Class 特性"]
    MERGE --> IRQ_REG["mpam_register_irqs()<br/>注册错误中断 (PPI/SPI)"]
    IRQ_REG --> ALLOC_CFG["mpam_allocate_config()<br/>分配 comp->cfg[] 数组"]
    ALLOC_CFG --> BRANCH["static_branch_enable<br/>(mpam_enabled)"]
    BRANCH --> CPUHP["切换到 online/offline<br/>cpuhp 回调"]
    CPUHP --> READY["MPAM 已启用"]

    READY --> ERR_IRQ{"错误中断?"}
    ERR_IRQ -->|"是"| ESR["__mpam_irq_handler()<br/>读取 ESR, 清除, 日志"]
    ESR --> DISABLE["schedule_work<br/>mpam_broken_work"]
    DISABLE --> MPAM_DISABLE["mpam_disable()"]
    MPAM_DISABLE --> UNREG_IRQ["mpam_unregister_irqs()"]
    UNREG_IRQ --> RESET_ALL["重置所有 Component 配置"]
    RESET_ALL --> BRANCH_OFF["static_branch_disable"]
```

### 2.5 寄存器访问机制

驱动不直接使用 `ioremap` 后的裸指针，而是通过 **IPI 调度** 到可访问 MSC 的 CPU 上执行：

```mermaid
sequenceDiagram
    participant CALLER as 内核子系统 (resctrl)
    participant DRV as mpam_devices.c
    participant TARGET as 可访问 MSC 的 CPU
    participant HW as MPAM MSC 硬件

    CALLER->>DRV: mpam_apply_config(comp, partid, cfg)
    DRV->>DRV: 遍历 comp->vmsc->ris
    DRV->>TARGET: smp_call_on_cpu(preferred_cpu, __write_config, arg)
    Note over TARGET: 不可抢占, 但可迁移

    TARGET->>TARGET: mpam_reprogram_ris_partid(ris, partid, cfg)
    TARGET->>TARGET: mutex_lock(&msc->part_sel_lock)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_PART_SEL, partsel)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_CPBM, cfg->cpbm)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_MBW_PBM, cfg->mbw_pbm)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_MBW_MAX, cfg->mbw_max)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_CMAX, cmax)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_CMIN, 0)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_CASSOC, cassoc)
    TARGET->>HW: __mpam_write_reg(msc, MPAMCFG_PRI, pri_val)
    TARGET->>TARGET: mutex_unlock(&msc->part_sel_lock)

    TARGET-->>DRV: 完成
    DRV-->>CALLER: 返回
```

**设计原因 (源码注释 1766-1771)：** MSC 可能控制某组 CPU 的流量，但只能从更广泛的 CPU 集合访问。如果该组所有 CPU 都处于 PSCI:CPU_SUSPEND，对应缓存也可能被关闭。因此必须在可访问的 CPU 上执行 MMIO 操作。

### 2.6 锁策略

驱动使用严格的锁顺序和 SRCU 保护：

```
锁顺序 (必须按此顺序获取):
  cfg_lock > part_sel_lock > mon_sel_lock > error_irq_lock

全局锁:
  mpam_list_lock   -- 保护 SRCU 链表的写操作
  partid_max_lock  -- spinlock, 保护 mpam_partid_max/pmg_max

SRCU:
  mpam_srcu        -- 保护所有 RCU 链表的读操作

快速路径:
  static_branch    -- mpam_enabled, 零开销检查
```

### 2.7 监控器设计

驱动实现了完整的 CSU 和 MBWU 监控器支持：

```mermaid
flowchart TD
    subgraph "CSU 监控读取流程"
        CSU_CALL["mpam_msmon_read(comp, ctx, mpam_feat_msmon_csu, &val)"]
        CSU_CALL --> CSU_IPI["smp_call_function_any(&msc->accessibility)"]
        CSU_IPI --> CSU_SEL["写 MON_SEL = ris_idx | mon_idx"]
        CSU_SEL --> CSU_CMP{"配置匹配?"}
        CSU_CMP -->|"否"| CSU_W["重写 FLT/CTL, 清零计数器, 使能"]
        CSU_CMP -->|"是"| CSU_R["读 MSMON_CSU"]
        CSU_W --> CSU_R
        CSU_R --> CSU_NR{"NRDY?"}
        CSU_NR -->|"是"| CSU_WAIT["等待 nrdy_usec<br/>重试读取"]
        CSU_NR -->|"否"| CSU_RET["返回 VALUE"]
        CSU_WAIT --> CSU_R
    end

    subgraph "MBWU 长计数器读取 (44/63位)"
        MBWU_CALL["mpam_msmon_read(comp, ctx, mpam_feat_msmon_mbwu, &val)"]
        MBWU_CALL --> MBWU_IPI["smp_call_function_any()"]
        MBWU_IPI --> MBWU_SEL["写 MON_SEL"]
        MBWU_SEL --> MBWU_CMP{"配置匹配?"}
        MBWU_CMP -->|"否"| MBWU_W["重写 FLT/CTL, 使能"]
        MBWU_CMP -->|"是"| MBWU_R["读 MSMON_MBWU_L<br/>64位原子读取"]
        MBWU_W --> MBWU_R
        MBWU_R --> MBWU_OVF{"溢出?"}
        MBWU_OVF -->|"是"| MBWU_COR["correction += 溢出值<br/>硬件清零 OFLOW_STATUS"]
        MBWU_OVF -->|"否"| MBWU_ADD["val += correction"]
        MBWU_COR --> MBWU_ADD
        MBWU_ADD --> MBWU_RET["返回 val"]
    end
```

**关键设计点：**

- **配置缓存：** 驱动缓存 FLT/CTL 的当前值，仅在配置变化时重写，避免不必要的 NRDY 等待
- **溢出修正：** MBWU 31/44/63 位计数器溢出时，驱动累加 `correction` 值实现软件扩展
- **NRDY 处理：** CSU 计数器依赖固件提供 `arm,not-ready-us` 属性；MBWU 支持硬件管理 NRDY (通过 hw probe 检测)
- **PMG 过滤：** CSU 支持 XCL (Exclude Clean) 过滤脏行；MBWU 支持 RWBW (Read/Write Bandwidth) 过滤

### 2.8 中断处理

```mermaid
flowchart TD
    IRQ_IN["MPAM 错误中断<br/>电平触发"] --> HW{"PPI or SPI?"}
    HW -->|"PPI (per-CPU)"| PPI_H["mpam_ppi_handler()<br/>从 percpu dev_id 获取 msc"]
    HW -->|"SPI"| SPI_H["mpam_spi_handler()<br/>dev_id 直接是 msc"]

    PPI_H --> COMMON["__mpam_irq_handler()"]
    SPI_H --> COMMON

    COMMON --> READ_ESR["读 MPAMF_ESR"]
    READ_ESR --> CHK{"errcode != 0?"}
    CHK -->|"否"| NONE["返回 IRQ_NONE"]
    CHK -->|"是"| CLEAR["清除 ESR<br/>先清高位再清低位"]
    CLEAR --> LOG["pr_err_ratelimited<br/>打印 MSC/ERRCODE/PARTID/PMG/RIS"]
    LOG --> DIS_ECR["写 MPAMF_ECR=0<br/>禁用中断"]
    DIS_ECR --> CHK_EN{"mpam 已启用?"}
    CHK_EN -->|"否"| DONE["返回 IRQ_HANDLED"]
    CHK_EN -->|"是"| BROKEN["schedule_work(mpam_broken_work)"]
    BROKEN --> DISABLE["mpam_disable()"]
    DISABLE --> FULL_RESET["重置所有 Component<br/>static_branch_disable"]
```

**设计原则 (源码注释 70-76)：** 所有 MPAM 错误中断都表示软件 bug。收到中断后直接禁用驱动。

### 2.9 特性合并机制

当多个 RIS 组成 vMSC/Component/Class 时，驱动需要合并特性：

```mermaid
graph TB
    subgraph "vMSC 层 -- RIS 特性并集"
        V_MERGE["vmsc->props = OR(ris.props)"]
        V_MERGE -.->|"同资源别名"| V_ALIAS["参数取安全值<br/>max/min 视参数而定"]
    end

    subgraph "Class 层 -- vMSC 特性交集"
        C_INIT["class->props = 第一个 vmsc.props"]
        C_INIT --> C_MERGE["AND(class.props, vmsc.props)<br/>逐参数取安全值"]
    end

    subgraph "安全值选择规则"
        R1["位图宽度 (cpbm_wd):<br/>不匹配 → 清除特性"]
        R2["计数器数量 (num_csu_mon):<br/>取最小值"]
        R3["优先级位宽 (intpri_wd):<br/>取最小值"]
        R4["0-is-low 行为不一致:<br/>清除特性"]
    end

    V_ALIAS --> C_MERGE
    C_MERGE --> R1
    C_MERGE --> R2
    C_MERGE --> R3
    C_MERGE --> R4
```

### 2.10 外部接口

驱动目前仅导出一个符号：

```c
/* 外部子系统 (如 resctrl) 注册为 Requestor, 参与 PARTID 范围协商 */
int mpam_register_requestor(u16 partid_max, u8 pmg_max);
EXPORT_SYMBOL(mpam_register_requestor);
```

**PARTID 范围协商规则：**
- 第一个 requestor 设定 `mpam_partid_max` 和 `mpam_pmg_max`
- 后续 requestor 只能进一步**缩小**范围 (`min()`)
- `partid_max_published=true` 后 (MPAM 启用)，新 requestor 不能再缩小

### 2.11 与软件方案其他层的接口扩展建议

由于现有驱动是内部子系统，需要扩展接口以支持用户态守护进程和应用：

```mermaid
graph TB
    subgraph "现有驱动能力 (mpam_devices.c)"
        EXIST["mpam_apply_config()<br/>mpam_msmon_read()<br/>mpam_msmon_reset_mbwu()<br/>mpam_register_requestor()"]
    end

    subgraph "需要扩展的接口"
        SYSFS_E["sysfs 导出<br/>Class/Component 信息"]
        IOCTL_E["ioctl 字符设备<br/>分区分配/释放/配置"]
        MONFS["监控文件系统<br/>类 resctrl 的 mon_groups"]
        KVM_E["KVM 扩展<br/>VM PARTID 分配"]
    end

    subgraph "现有驱动作为基础"
        BASE["mpam_devices.c<br/>提供对象模型和硬件操作"]
    end

    SYSFS_E --> BASE
    IOCTL_E --> BASE
    MONFS --> BASE
    KVM_E --> BASE
    EXIST --> BASE
```

---

## 3. 守护进程层

### 3.1 守护进程模块结构

```mermaid
graph TB
    subgraph "mpamd 主循环"
        MAIN["main()"] --> INIT_D["初始化"]
        INIT_D --> LOAD["加载配置文件"]
        LOAD --> PARSE["解析静态配置"]
        PARSE --> APPLY["应用初始配额<br/>调用 ioctl"]
        APPLY --> LOOP["主事件循环"]
    end

    subgraph "D-Bus 服务 (com.arm.mpam)"
        DBUS_S["D-Bus 服务端<br/>GDBus"]
        DBUS_API["API 方法"]
        DBUS_SIG["信号推送"]
    end

    subgraph "资源分区管理器"
        ALLOC["分区分配器<br/>mpam_alloc_partition()"]
        FREE["分区释放<br/>mpam_release_partition()"]
        CFG["配额配置<br/>mpam_configure_quota()"]
        POOL["PARTID 池管理<br/>bitmap 分配"]
    end

    subgraph "监控采集器"
        POLL["sysfs 轮询线程<br/>poll_interval"]
        PERF_R["perf event 读取"]
        CSU_R["CSU 使用量采集"]
        MBWU_R["MBWU 使用量采集"]
        AGG["数据聚合<br/>per-PARTID 统计"]
    end

    subgraph "策略引擎 (插件化)"
        ENGINE["策略调度器"]
        P_STATIC["static 策略<br/>加载时固定"]
        P_FEEDBACK["feedback 策略<br/>基于监控反馈"]
        P_GUEST_AWARE["guest_aware 策略<br/>感知 VM 生命周期"]
        PLUG_IN[".so 插件接口"]
    end

    LOOP --> DBUS_S
    DBUS_S --> DBUS_API
    DBUS_S --> DBUS_SIG
    DBUS_API --> ALLOC
    DBUS_API --> FREE
    DBUS_API --> CFG
    ALLOC --> POOL
    POLL --> CSU_R
    POLL --> MBWU_R
    CSU_R --> AGG
    MBWU_R --> AGG
    AGG --> ENGINE
    ENGINE --> P_STATIC
    ENGINE --> P_FEEDBACK
    ENGINE --> P_GUEST_AWARE
    ENGINE --> PLUG_IN
    ENGINE --> CFG
```

### 3.2 配置文件格式 (YAML)

```yaml
# /etc/mpam/mpam.conf
global:
  default_policy: feedback
  poll_interval_ms: 100
  log_level: info

# 静态分区定义
partitions:
  - name: rt_critical
    partid: 1
    owner: pid:1000          # 或 cgroup:/rt_critical
    resources:
      cache_llc:
        ccap_cmax: 0xC000    # 75%
        ccap_cmin: 0x8000    # 50%
        cpor_cpbm: 0xC0      # 高6路
      memory:
        mbw_min: 0x4000      # 25% 保证
        mbw_max: 0xFFFF      # 无上限

  - name: be_normal
    partid: 2
    owner: cgroup:/be
    resources:
      cache_llc:
        ccap_cmax: 0x4000    # 25%
        ccap_cmin: 0x0000    # 无保证
        cpor_cpbm: 0x3C      # 低6路
      memory:
        mbw_min: 0x0000      # 无保证
        mbw_max: 0x8000      # 50% 上限

  - name: vm_default
    partid: 3
    owner: vm_uuid:*
    resources:
      cache_llc:
        ccap_cmax: 0x2000
        cpor_cpbm: 0x03
      memory:
        mbw_pbm: 0x02

# 策略插件
policies:
  feedback:
    module: libmpam_policy_feedback.so
    params:
      scale_up_threshold: 80
      scale_down_threshold: 20
      adjust_step: 0x0800
      min_quota: 0x1000
      max_quota: 0xC000

  guest_aware:
    module: libmpam_policy_guest.so
    params:
      vm_boot_boost: 0x4000
      vm_idle_shrink: 0x1000
```

### 3.3 策略引擎接口

```c
/* 策略插件接口 -- 所有策略 .so 必须实现 */
struct mpam_policy_ops {
    const char *name;

    /* 初始化 */
    int (*init)(const struct mpam_policy_params *params);

    /* 监控数据输入 */
    int (*feed_monitor_data)(uint16_t partid,
                              const struct mpam_usage_stats *stats);

    /* 策略决策 -- 返回需要调整的配额 */
    int (*decide)(uint16_t partid,
                  struct mpam_quota_adjustment *adjust);

    /* VM 生命周期事件 */
    int (*on_vm_start)(uint16_t vm_partid);
    int (*on_vm_stop)(uint16_t vm_partid);
    int (*on_vm_pause)(uint16_t vm_partid);

    /* 销毁 */
    void (*destroy)(void);
};

struct mpam_usage_stats {
    uint64_t cache_usage_bytes;   /* CSU 监控值 */
    uint64_t bw_usage_bytes;      /* MBWU 监控值 */
    uint64_t cache_hit_rate;      /* perf cache-misses 参考值 */
    uint64_t timestamp_ms;
};

struct mpam_quota_adjustment {
    uint32_t new_ccap_cmax;
    uint32_t new_mbw_max;
};
```

### 3.4 D-Bus 接口定义

```
服务名: com.arm.mpam
对象路径: /com/arm/mpam
接口: com.arm.mpam.Manager

方法:
  AllocatePartition(in s name, in a resources, out u partid)
  ReleasePartition(in u partid)
  ConfigureQuota(in u partid, in a resources)
  GetUsage(in u partid, out a stats)

信号:
  PartitionAllocated(u partid, s name)
  PartitionReleased(u partid)
  QuotaAdjusted(u partid, a new_quota)
  MonitorOverflow(u msc_index, u mon_index)
```

---

## 4. 动态库层

### 4.1 API 类图

```mermaid
classDiagram
    class MPAM_Manager {
        -mpam_ctx_t *ctx
        +MPAM_Manager(config_path: string)
        +~MPAM_Manager()
        +init() int
        +cleanup() int
    }

    class MPAM_Partition {
        -uint16_t partid
        -uint16_t pmg
        -mpam_quota_t quota
        +alloc(name: string, resources: ResourceSpec) int
        +release() int
        +configure(resources: ResourceSpec) int
        +enable() int
        +disable() int
        +get_quota() mpam_quota_t
    }

    class MPAM_Monitor {
        -uint32_t msc_idx
        -uint16_t mon_idx
        -mpam_mon_filter_t filter
        +create(msc: int, type: MonType) int
        +set_filter(partid: int, pmg: int) int
        +read() UsageStats
        +reset() int
        +enable() int
        +disable() int
    }

    class MPAM_ThreadTag {
        +set_tag(partid: int, pmg: int) int
        +get_tag() TagInfo
        +clear_tag() int
    }

    class MPAM_Callback {
        <<interface>>
        +on_quota_changed(partid: int, new_quota: mpam_quota_t)
        +on_overflow(msc: int, mon: int)
        +on_partition_released(partid: int)
    }

    class ResourceSpec {
        +cache_ccap_cmax: uint32_t
        +cache_ccap_cmin: uint32_t
        +cache_cpor_cpbm: bytes
        +mem_mbw_max: uint32_t
        +mem_mbw_min: uint32_t
        +mem_mbw_pbm: bytes
        +to_dict() dict
        +from_dict(d: dict) ResourceSpec
    }

    class UsageStats {
        +cache_usage_bytes: uint64_t
        +bw_usage_bytes: uint64_t
        +timestamp_ms: uint64_t
    }

    MPAM_Manager --> MPAM_Partition : manages
    MPAM_Manager --> MPAM_Monitor : manages
    MPAM_Manager --> MPAM_ThreadTag : provides
    MPAM_Manager ..|> MPAM_Callback : implements
    MPAM_Partition --> ResourceSpec : uses
    MPAM_Monitor ..|> UsageStats : returns
```

### 4.2 用户态 API (libmpam.h)

```c
#ifndef LIBMPAM_H
#define LIBMPAM_H

#include <stdint.h>
#include <stddef.h>

/* ========== 基础类型 ========== */

typedef struct {
    uint32_t ccap_cmax;       /* 缓存最大容量 (定点小数) */
    uint32_t ccap_cmin;       /* 缓存最小容量 (定点小数) */
    uint32_t cpor_cpbm_len;   /* CPOR 位图字节长度 */
    uint8_t  *cpor_cpbm;      /* CPOR 位图数据 */
    uint32_t mbw_max;         /* 带宽最大限制 (定点小数) */
    uint32_t mbw_min;         /* 带宽最小保证 (定点小数) */
    uint32_t mbw_pbm_len;     /* MBWPBM 位图字节长度 */
    uint8_t  *mbw_pbm;        /* MBWPBM 位图数据 */
} mpam_resource_spec_t;

typedef struct {
    uint64_t cache_usage_bytes;
    uint64_t bw_usage_bytes;
    uint64_t timestamp_ms;
} mpam_usage_stats_t;

typedef struct {
    uint16_t partid;
    uint16_t pmg;
} mpam_tag_t;

/* ========== 管理器 ========== */

typedef struct mpam_ctx mpam_ctx_t;

mpam_ctx_t *mpam_create(const char *config_path);
void         mpam_destroy(mpam_ctx_t *ctx);

/* ========== 分区管理 ========== */

int mpam_partition_alloc(mpam_ctx_t *ctx, const char *name,
                         const mpam_resource_spec_t *spec,
                         uint16_t *out_partid);

int mpam_partition_release(mpam_ctx_t *ctx, uint16_t partid);

int mpam_partition_configure(mpam_ctx_t *ctx, uint16_t partid,
                              const mpam_resource_spec_t *spec);

int mpam_partition_enable(mpam_ctx_t *ctx, uint16_t partid);
int mpam_partition_disable(mpam_ctx_t *ctx, uint16_t partid);

/* ========== 监控 ========== */

int mpam_monitor_create(mpam_ctx_t *ctx, uint32_t msc_index,
                        const char *type, uint16_t *out_mon_idx);

int mpam_monitor_set_filter(mpam_ctx_t *ctx, uint32_t msc_index,
                             uint16_t mon_idx, uint16_t partid, uint8_t pmg);

int mpam_monitor_read(mpam_ctx_t *ctx, uint32_t msc_index,
                      uint16_t mon_idx, mpam_usage_stats_t *stats);

int mpam_monitor_reset(mpam_ctx_t *ctx, uint32_t msc_index,
                       uint16_t mon_idx);

int mpam_monitor_enable(mpam_ctx_t *ctx, uint32_t msc_index,
                        uint16_t mon_idx);

int mpam_monitor_disable(mpam_ctx_t *ctx, uint32_t msc_index,
                         uint16_t mon_idx);

/* ========== 线程标记 ========== */

int mpam_thread_set_tag(uint16_t partid, uint16_t pmg);
int mpam_thread_get_tag(mpam_tag_t *tag);
int mpam_thread_clear_tag(void);

/* ========== 回调 ========== */

typedef void (*mpam_quota_changed_cb)(uint16_t partid,
                                        const mpam_resource_spec_t *new_spec);

typedef void (*mpam_overflow_cb)(uint32_t msc_index, uint16_t mon_index);

int mpam_register_callback(mpam_ctx_t *ctx, int event_mask,
                           void (*cb)(void *data), void *data);

#endif /* LIBMPAM_H */
```

### 4.3 传输层抽象

```mermaid
graph TB
    subgraph "libmpam 传输层抽象"
        TRANS["mpam_transport_t<br/>接口"]
    end

    subgraph "传输实现"
        IOCTL_T["ioctl 传输<br/>/dev/mpamctl<br/>低延迟, 无守护进程依赖"]
        DBUS_T["D-Bus 传输<br/>com.arm.mpam<br/>支持回调, 需要守护进程"]
        SYSFS_T["sysfs 传输<br/>/sys/devices/platform/mpam<br/>最简单, 无锁"]
    end

    subgraph "传输层选择逻辑"
        CHECK["mpam_create()"]
        CHECK -->|"默认"| IOCTL_T
        CHECK -->|"config指定mode=dbus"| DBUS_T
        CHECK -->|"config指定mode=sysfs"| SYSFS_T
    end

    TRANS --> IOCTL_T
    TRANS --> DBUS_T
    TRANS --> SYSFS_T
```

```c
typedef struct mpam_transport {
    const char *name;
    int (*open)(void);
    int (*close)(void);
    int (*alloc_partition)(const mpam_resource_spec_t *spec, uint16_t *partid);
    int (*release_partition)(uint16_t partid);
    int (*configure)(uint16_t partid, const mpam_resource_spec_t *spec);
    int (*monitor_read)(uint32_t msc, uint16_t mon, mpam_usage_stats_t *stats);
    int (*monitor_config)(uint32_t msc, uint16_t mon, uint16_t partid, uint8_t pmg);
} mpam_transport_t;

/* 传输层注册 -- 可通过环境变量覆盖 */
extern mpam_transport_t mpam_transport_ioctl;
extern mpam_transport_t mpam_transport_dbus;
extern mpam_transport_t mpam_transport_sysfs;
```

---

## 5. 用户态应用

### 5.1 mpamctl -- 命令行管理工具

```
用法: mpamctl [命令] [参数]

命令:
  info                          显示系统 MPAM 信息
  list                          列出所有分区
  alloc -n NAME [-c CCAP] [-w CPOR] [-b BW] [-m MBWMIN]
                                分配新分区
  release PARTID                释放分区
  config PARTID [-c CCAP] [-w CPOR] [-b BW]
                                修改分区配额
  mon read MSC MON               读取监控值
  mon reset MSC MON              重置监控器
  mon enable MSC MON             使能监控器
  vm assign -p VM_PID [-c CCAP]  为 VM 分配 PARTID
  vm release VM_PID              释放 VM 的 PARTID

示例:
  # 查看系统信息
  mpamctl info

  # 为实时任务分配 75% 缓存 + 25% 带宽保证
  mpamctl alloc -n rt_task -c 0xC000 -w 0xC0 -m 0x4000

  # 查看所有分区配额
  mpamctl list

  # 读取 MSC0 的 CSU 监控器 0
  mpamctl mon read 0 0
```

### 5.2 mpam-top -- 实时监控工具

类似 `htop` 的终端 UI，实时显示各 PARTID 的资源使用情况：

```
MPAM Monitor - Refresh: 100ms | MSCs: 2 | Partitions: 4

PARTID  NAME          Cache Usage    Cache %   BW Usage       BW %
    0    (default)     12.5 MB       9.8%     256 MB/s       10.2%
    1    rt_critical  75.0 MB      58.6%     512 MB/s       20.5%
    2    be_normal     25.0 MB      19.5%     102 MB/s        4.1%
    3    vm_default   15.0 MB      11.7%     128 MB/s        5.1%
    4    (free)         0.5 MB       0.4%       2 MB/s        0.1%
    ─────────────────────────────────────────────────────────────────
    Total              128.0 MB     100.0%    1000 MB/s       40.0%
```

### 5.3 应用集成示例

```c
#include "libmpam.h"

int main(int argc, char *argv[])
{
    mpam_ctx_t *ctx = mpam_create(NULL);  /* 使用默认配置 */

    /* 1. 分配分区 -- 75% 缓存 */
    uint16_t partid;
    mpam_resource_spec_t spec = {
        .ccap_cmax = 0xC000,     /* 75% 缓存最大容量 */
        .ccap_cmin = 0x8000,     /* 50% 缓存最小保证 */
        .mbw_min   = 0x4000,     /* 25% 带宽最小保证 */
    };
    mpam_partition_alloc(ctx, "my_rt_task", &spec, &partid);

    /* 2. 创建监控器 */
    uint16_t mon;
    mpam_monitor_create(ctx, 0, "csu", &mon);
    mpam_monitor_set_filter(ctx, 0, mon, partid, 0);

    /* 3. 设置工作线程的 PARTID 标记 */
    mpam_thread_set_tag(partid, 0);

    /* 4. 执行实际工作负载 */
    do_realtime_work();

    /* 5. 读取监控数据 */
    mpam_usage_stats_t stats;
    mpam_monitor_read(ctx, 0, mon, &stats);
    printf("Cache usage: %lu bytes\n", stats.cache_usage_bytes);

    /* 6. 清理 */
    mpam_thread_clear_tag();
    mpam_partition_release(ctx, partid);
    mpam_destroy(ctx);
    return 0;
}
```

---

## 6. 虚拟化支持

### 6.1 KVM/QEMU 集成架构

```mermaid
graph TB
    subgraph "Host 管理层"
        VMM["mpamd 守护进程"]
        KVM["KVM /dev/kvm<br/>KVM_MPAM_VM_ASSIGN ioctl"]
        QEMU["QEMU 进程<br/>-device mpam,assign=partid:N"]
    end

    subgraph "内核层"
        DRV["mpam.ko"]
        VFIO["vfio-mpam 设备"]
        VCPU["vCPU 调度"]
    end

    subgraph "Guest (VM)"
        VDRV["virtio-mpam 驱动"]
        VLIB["guest libmpam"]
        VAPP["Guest 应用"]
    end

    VMM -->|"VM 启动时<br/>分配 host_partid<br/>映射到 vm_partid"| KVM
    QEMU -->|"ioctl<br/>VM_ASSIGN"| KVM
    KVM --> DRV
    DRV --> VFIO
    VCPU -->|"VM 切换时<br/>写 MPAM2_EL2"| DRV
    VFIO -.->|"MMIO trap<br/>转发到 guest"| VDRV
    VAPP --> VLIB
    VLIB --> VDRV
```

### 6.2 VM 启动时 PARTID 分配流程

```mermaid
sequenceDiagram
    participant Q as QEMU/mpamd
    participant K as KVM (ioctl)
    participant D as mpam.ko
    participant H as MPAM MSC

    Q->>K: KVM_MPAM_VM_ASSIGN
    Note right of Q: vm_partid=5, host_partid=3<br/>ccap=50%, cpor=0x03
    K->>D: 验证 host_partid 可用
    K->>D: 写 MPAMCFG_PART_SEL = host_partid
    D->>H: 写 MPAMCFG_CMAX = 0x8000
    D->>H: 写 MPAMCFG_CPBM = 0x03
    K->>D: 写 MPAMCFG_EN = host_partid
    D-->>K: 完成
    K-->>Q: 返回 vm_partid

    Note over K,H: VM 运行时

    Q->>K: VM RUN (vCPU 线程切换)
    K->>D: 写 MPAM2_EL2.PARTID_D = host_partid
    Note right of K: vCPU 调度时自动设置

    Q->>K: VM PAUSE/STOP
    K->>D: 写 MPAMCFG_DIS = host_partid
    Note right of Q: 释放资源给其他 VM
```

### 6.3 Guest MPAM 设备模型

Guest 中通过 virtio-mpam 设备暴露有限的 MPAM 操作：

```c
/*
 * virtio-mpam: Guest 可见的 MPAM 寄存器子集
 * 通过 virtio MMIO region 暴露
 *
 * Guest 读写的寄存器会被 QEMU trap,
 * QEMU 将 vm_partid 映射到 host_partid 后转发给 mpam.ko
 */

/* Guest 可见的寄存器 (只读, 由 host 配置) */
#define VIRTIO_MPAM_IDR        0x00  /* 特性位 (host 配置的子集) */
#define VIRTIO_MPAM_CMAX_WD    0x04  /* CMAX 位宽 */
#define VIRTIO_MPAM_CPBM_WD    0x08  /* CPBM 位宽 */

/* Guest 可写的寄存器 (trap 到 host) */
#define VIRTIO_MPAM_MON_SEL    0x80  /* 监控器选择 */
#define VIRTIO_MPAM_CSU_FLT    0x90  /* CSU 过滤 */
#define VIRTIO_MPAM_CSU_CTL    0x98  /* CSU 控制 */
#define VIRTIO_MPAM_CSU_VALUE  0xA0  /* CSU 计数器 (只读) */
```

---

## 7. 模块交互与关键数据流

### 7.1 分区分配数据流

```mermaid
sequenceDiagram
    participant APP as 用户应用
    participant LIB as libmpam.so
    participant DAEMON as mpamd
    participant DRV as mpam.ko
    participant HW as MPAM MSC

    APP->>LIB: mpam_partition_alloc(name, spec)
    LIB->>DAEMON: D-Bus: AllocatePartition(name, spec)

    DAEMON->>DAEMON: 策略检查 & PARTID 池分配
    DAEMON->>DRV: ioctl: MPAM_IOC_ALLOC(partid, spec)

    DRV->>DRV: mpam_partid_configure(partid, spec)
    DRV->>HW: 写 MPAMCFG_PART_SEL = partid
    DRV->>HW: 写 MPAMCFG_CMAX = spec.ccap_cmax
    DRV->>HW: 写 MPAMCFG_CPBM = spec.cpor_cpbm
    DRV->>HW: 写 MPAMCFG_MBW_MAX = spec.mbw_max
    DRV->>HW: 写 MPAMCFG_EN = partid

    DRV-->>DAEMON: 成功
    DAEMON-->>LIB: partid = N
    LIB-->>APP: 成功, partid = N

    APP->>APP: mpam_thread_set_tag(partid)
```

### 7.2 监控与动态调整数据流

```mermaid
sequenceDiagram
    participant HW as MPAM MSC
    participant DRV as mpam.ko
    participant SYS as sysfs
    participant DAEMON as mpamd (监控采集器)
    participant POL as 策略引擎
    participant DRV2 as mpam.ko (配置)
    participant APP as 用户应用

    Note over HW,DRV: 硬件持续计数

    HW->>DRV: CSU/MBWU 计数器更新
    DRV->>SYS: 导出 /sys/.../monitor/csu0/value

    loop 每 100ms
        DAEMON->>SYS: 读取所有监控器
        SYS-->>DAEMON: value 数据
        DAEMON->>DAEMON: 聚合 per-PARTID 统计
        DAEMON->>POL: feed_monitor_data(partid, stats)
    end

    POL->>POL: 判断: partid=1 cache > 80%
    POL->>POL: 决定: ccap_cmax 缩小 0x0800
    POL-->>DAEMON: adjustment

    DAEMON->>DRV2: ioctl: MPAM_IOC_CONFIGURE(partid=1, new_spec)
    DRV2->>HW: 写 MPAMCFG_PART_SEL = 1
    DRV2->>HW: 写 MPAMCFG_CMAX = 0xB800
    DRV2->>HW: 等待 NRDY 清除

    DAEMON->>APP: D-Bus 信号: QuotaAdjusted(partid=1)
```

### 7.3 VM 生命周期数据流

```mermaid
flowchart TD
    START["VM 启动请求"] --> QUERY["mpamd 查询<br/>可用 PARTID 池"]
    QUERY --> ASSIGN["分配 host_partid<br/>配置配额"]
    ASSIGN --> IOCTL["KVM_MPAM_VM_ASSIGN"]
    IOCTL --> MAP["建立<br/>vm_partid -> host_partid 映射"]
    MAP --> RUN["VM 运行<br/>vCPU 调度时写 MPAM2_EL2"]

    RUN --> PAUSE{"VM 暂停/迁移?"}
    PAUSE -->|"是"| DIS["MPAMCFG_DISABLE<br/>释放资源"]
    DIS --> RELEASE["等待恢复"]

    RUN --> STOP{"VM 关闭?"}
    STOP -->|"是"| CLEANUP["释放 PARTID<br/>MPAMCFG_DISABLE<br/>归还池"]
    CLEANUP --> DONE["完成"]

    RELEASE --> RESUME["VM 恢复<br/>MPAMCFG_ENABLE"]
    RESUME --> RUN
```

---

## 8. 可扩展性与解耦设计

### 8.1 分层解耦关系

```mermaid
graph TB
    subgraph "依赖方向: 上层依赖下层接口"
        APP["应用层"]
        LIB["libmpam.so"]
        DAEMON["mpamd"]
        DRV["mpam.ko"]
    end

    APP -->|"C API<br/>libmpam.h"| LIB
    LIB -->|"传输抽象<br/>ioctl/dbus/sysfs"| DAEMON
    LIB -->|"ioctl<br/>/dev/mpamctl"| DRV
    DAEMON -->|"ioctl<br/>/dev/mpamctl"| DRV
    DRV -->|"MMIO/系统寄存器"| HW["MPAM MSC 硬件"]
```

**解耦关键点：**

| 层间 | 解耦方式 | 替换可行性 |
|------|---------|-----------|
| App ↔ libmpam | C API 接口 | 应用可自由切换 libmpam 版本 |
| libmpam ↔ 驱动 | 传输层抽象 (ioctl/dbus/sysfs) | 可切换传输实现 |
| libmpam ↔ mpamd | D-Bus 协议 | 可替换守护进程实现 |
| mpamd ↔ 驱动 | ioctl 接口 | 驱动升级不影响守护进程 |
| 策略引擎 ↔ mpamd | .so 插件接口 | 可加载自定义策略 |

### 8.2 资源类型抽象

```c
/* 统一资源类型接口 -- 新增资源类型只需实现此接口 */
struct mpam_rsrc_ops {
    const char *name;          /* "cache_llc", "memory_ctrl" */
    enum mpam_rsrc_type type;

    /* 配置接口 */
    int (*configure)(struct mpam_msc *msc, uint16_t rsrc_idx,
                     uint16_t partid, const mpam_resource_spec_t *spec);

    /* 监控接口 */
    int (*monitor_create)(struct mpam_msc *msc, uint16_t rsrc_idx);
    int (*monitor_read)(struct mpam_msc *msc, uint16_t rsrc_idx,
                        uint16_t mon_idx, mpam_usage_stats_t *stats);

    /* 能力查询 */
    int (*get_capabilities)(struct mpam_msc *msc, uint16_t rsrc_idx,
                            mpam_rsrc_caps_t *caps);
};

/* 注册表 */
int mpam_rsrc_register(const struct mpam_rsrc_ops *ops);
const struct mpam_rsrc_ops *mpam_rsrc_lookup(enum mpam_rsrc_type type);
```

### 8.3 策略插件加载

```c
/* 策略插件加载 */
#define MPAM_POLICY_PATH "/usr/lib/mpam/policy/"

static int load_policy_plugins(const char *policy_name)
{
    char path[256];
    void *handle;
    struct mpam_policy_ops *ops;

    snprintf(path, sizeof(path), "%s/libmpam_policy_%s.so",
             MPAM_POLICY_PATH, policy_name);
    handle = dlopen(path, RTLD_NOW);
    ops = dlsym(handle, "mpam_policy_ops");

    /* 校验接口版本 */
    if (ops->version != MPAM_POLICY_VERSION)
        return -EINVAL;

    ops->init(&global_params);
    register_policy(ops);
    return 0;
}
```

### 8.4 未来扩展点

| 扩展方向 | 扩展方式 | 接口预留 |
|---------|---------|---------|
| 新资源类型 (如 NPU 缓存) | 实现 `mpam_rsrc_ops` | 注册表已预留 |
| 自定义策略 | 编写 .so 插件 | `mpam_policy_ops` 接口 |
| 容器集成 | cgroup driver | sysfs 属性兼容 cgroup |
| 远程管理 | gRPC/WebSocket | D-Bus 可扩展为网络传输 |
| 热升级 | 驱动接口版本化 | ioctl 命令号分配预留空间 |

---

## 附录：文件布局

```
mpam-linux/
├── kernel/
│   └── mpam/
│       ├── Kconfig
│       ├── Makefile
│       ├── mpam_devices.c        ★ 核心驱动 (Arm upstream, 2726行)
│       │   # 5层对象模型: Class→Component→vMSC→RIS→MSC
│       │   # Platform driver, ACPI 发现, IPI 寄存器访问
│       │   # SRCU 保护的 RCU 链表, 延迟垃圾回收
│       │   # CPU hotplug: discovery→enable→online/offline
│       │   # CSU/MBWU 监控器, NRDY 等待, 溢出修正
│       │   # 错误中断 (PPI/SPI), ESR 清除, 驱动禁用
│       │   # 特性合并: RIS(并集)→vMSC(并集)→Class(交集)
│       │   # 唯一导出: mpam_register_requestor()
│       ├── mpam_internal.h        # 内部头文件 (struct 定义)
│       ├── <linux/arm_mpam.h>     # 公共头文件 (resctrl 接口)
│       ├── <acpi mpam.c>          # ACPI MPAM 表解析 (内核树)
│       ├── <arm64 mpam.c>         # arch MPAM: MPAM2_EL2, FEAT_MPAM
│       ├── mpam_sysfs.c           # sysfs 导出 (扩展建议)
│       ├── mpam_ioctl.c           # ioctl 接口 (扩展建议)
│       ├── mpam_kvm.c             # KVM 扩展 (扩展建议)
│       └── test_mpam_devices.c    # KUnit 测试 (CONFIG_MPAM_KUNIT_TEST)
├── daemon/
│   ├── mpamd.c                /* 主循环 */
│   ├── config.c                /* 配置文件解析 */
│   ├── manager.c               /* 分区管理器 */
│   ├── monitor.c               /* 监控采集器 */
│   ├── policy.c                /* 策略引擎 */
│   ├── dbus.c                  /* D-Bus 服务端 */
│   └── mpam.conf               /* 默认配置 */
├── lib/
│   ├── libmpam.h              /* 公开 API 头文件 */
│   ├── mpam_ctx.c             /* 上下文管理 */
│   ├── mpam_partition.c        /* 分区操作 */
│   ├── mpam_monitor.c         /* 监控操作 */
│   ├── mpam_thread.c          /* 线程标记 (prctl) */
│   ├── transport/
│   │   ├── transport.h         /* 传输层抽象接口 */
│   │   ├── transport_ioctl.c   /* ioctl 实现 */
│   │   ├── transport_dbus.c    /* D-Bus 实现 */
│   │   └── transport_sysfs.c  /* sysfs 实现 */
│   └── CMakeLists.txt
├── tools/
│   ├── mpamctl.c              /* 命令行管理工具 */
│   └── mpam_top.c              /* 实时监控工具 */
└── policy/
    ├── libmpam_policy_static.c      /* 静态策略 */
    ├── libmpam_policy_feedback.c    /* 反馈策略 */
    └── libmpam_policy_guest.c      /* VM 感知策略 */
```

---

*文档基于 Arm IHI 0099B.a MPAM MSC 规范及 MPAM ACS v0.5.0 代码设计生成。*
