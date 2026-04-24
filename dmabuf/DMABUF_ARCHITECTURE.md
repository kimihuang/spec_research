# DMA-BUF 子系统架构分析

## 1. 概述

DMA-BUF 是 Linux 内核中用于跨设备、跨子系统共享缓冲区的通用框架。它允许不同的硬件设备（GPU、VPU、ISP、显示控制器等）和内核子系统之间高效地共享内存缓冲区，同时提供同步机制来协调对这些缓冲区的并发访问。

### 核心设计目标

- **零拷贝缓冲区共享**：通过文件描述符（FD）在进程和设备间传递缓冲区，无需数据拷贝
- **跨设备同步**：通过 dma-fence 机制实现异步 DMA 操作的跨驱动同步
- **统一抽象**：为不同内存后端（连续内存、非连续内存、用户态内存）提供统一接口
- **隐式/显式同步**：支持 dma-resv 隐式同步和 sync_file 显式同步两种模式

## 2. 整体架构

```mermaid
graph TB
    subgraph "用户态 (Userspace)"
        APP[应用程序]
        LIB[libdmabufheap<br/>Android C++ 库]
    end

    subgraph "内核态 - 分配入口"
        DH["/dev/dma_heap/&lt;name&gt;<br/>dma-heap 框架"]
        UDM["/dev/udmabuf<br/>用户态 dmabuf"]
        PRIME["DRM PRIME<br/>GEM 导入/导出"]
    end

    subgraph "内核态 - 核心框架"
        DB["dma-buf 框架<br/>dma-buf.c"]
        RESV["dma-resv<br/>预留对象"]
        FENCE["dma-fence<br/>同步原语"]
        SF["sync_file<br/>用户态同步"]
    end

    subgraph "内核态 - Fence 容器"
        FA["dma-fence-array<br/>聚合多 fence"]
        FC["dma-fence-chain<br/>时序链 fence"]
        FU["dma-fence-unwrap<br/>fence 解包/合并"]
    end

    subgraph "内核态 - 内存后端"
        SH["system_heap<br/>系统内存(非连续)"]
        CH["cma_heap<br/>CMA 连续内存"]
    end

    subgraph "硬件设备"
        GPU[GPU]
        DSP[DSP/VPU]
        DISP[显示控制器]
    end

    APP -->|"ioctl DMA_HEAP_IOCTL_ALLOC"| DH
    APP -->|"ioctl UDMABUF_CREATE"| UDM
    LIB -->|"DMA_HEAP_IOCTL_ALLOC"| DH
    LIB -->|"DMA_BUF_IOCTL_SYNC"| DB

    DH -->|"allocate()"| SH
    DH -->|"allocate()"| CH

    SH -->|"dma_buf_export()"| DB
    CH -->|"dma_buf_export()"| DB
    UDM -->|"dma_buf_export()"| DB
    PRIME -->|"dma_buf_export/import"| DB

    DB -->|"内嵌"| RESV
    DB -->|"attach/map"| GPU
    DB -->|"attach/map"| DSP
    DB -->|"attach/map"| DISP

    RESV -->|"包含"| FENCE
    FENCE -->|"聚合"| FA
    FENCE -->|"链接"| FC
    FA -.->|"unwrap"| FU
    FC -.->|"unwrap"| FU

    FENCE -->|"包装为 FD"| SF
    SF -->|"poll/wait"| APP
```

## 3. 模块详解

### 3.1 dma-buf 核心框架 (`dma-buf.c`)

dma-buf 是整个子系统的核心，定义了 `struct dma_buf` 结构体和与之关联的文件操作。

**主要职责：**

| 功能 | API | 说明 |
|------|-----|------|
| 导出缓冲区 | `dma_buf_export()` | 创建 dma-buf 文件描述符 |
| 获取 FD | `dma_buf_fd()` | 将 dma-buf 关联到文件描述符 |
| 设备附加 | `dma_buf_attach()` | 设备附加到缓冲区，获取 sg_table |
| DMA 映射 | `dma_buf_map/unmap()` | 建立/解除设备 DMA 映射 |
| mmap | `dma_buf_mmap()` | 用户态映射 |
| vmap | `dma_buf_vmap/vunmap()` | 内核态虚拟映射 |
| CPU 访问同步 | `begin/end_cpu_access` | CPU 缓存一致性操作 |
| 同步文件导出 | `dma_buf_sync_file_export()` | 导出隐式 fence 为 sync_file |
| 同步文件导入 | `dma_buf_sync_file_import()` | 导入 sync_file 为隐式 fence |
| poll | `dma_buf_poll()` | 用户态 poll 查询 fence 状态 |

**文件系统注册：**

dma-buf 使用伪文件系统（pseudo filesystem）实现，魔法数为 `DMA_BUF_MAGIC`。每个 dma-buf 都是一个匿名 inode 文件，通过 `anon_inode_getfile()` 创建。

```mermaid
sequenceDiagram
    participant Heap as dma_heap
    participant DB as dma_buf_export
    participant FS as pseudo_fs
    participant App as 用户态

    Heap->>DB: dma_buf_export(exp_info)
    DB->>FS: get_unused_fd_flags()
    DB->>FS: anon_inode_getfile("dmabuf", fops, dmabuf)
    DB->>FS: fd_install(fd, file)
    DB-->>Heap: struct dma_buf *
    Heap-->>App: fd (文件描述符)

    App->>FS: mmap(fd, ...)
    FS->>DB: dma_buf_mmap_internal()
    DB->>DB: ops->mmap(dmabuf, vma)
```

### 3.2 dma-fence 同步原语 (`dma-fence.c`)

dma-fence 是跨设备、跨驱动的异步同步原语，代表一个 DMA 操作的完成信号。

**核心概念：**

- **Context**：每个执行上下文有唯一的 64-bit context ID，同一 context 内的 fence 全序
- **Seqno**：同一 context 内递增的序列号，用于比较 fence 顺序
- **Signal**：fence 从未信号变为已信号的单向状态转换

```mermaid
stateDiagram-v2
    [*] --> Unsignaled: dma_fence_init()
    Unsignaled --> Signaled: dma_fence_signal()
    Signaled --> [*]: dma_fence_put() (refcount=0)

    note right of Unsignaled: 可注册回调 callback<br/>可执行 wait
    note right of Signaled: 回调已执行<br/>等待者已唤醒<br/>timestamp 已记录
```

**关键操作：**

| 操作 | 函数 | 说明 |
|------|------|------|
| 初始化 | `dma_fence_init()` | 设置 context、seqno、ops、lock |
| 发信号 | `dma_fence_signal()` | 设置 SIGNALED 标志，执行所有回调 |
| 等待 | `dma_fence_wait()` | 阻塞等待 fence 信号 |
| 添加回调 | `dma_fence_add_callback()` | 注册信号完成回调 |
| 获取引用 | `dma_fence_get/put()` | 引用计数管理 |
| 设置截止时间 | `dma_fence_set_deadline()` | 提示信号方优先级（电源管理） |

**Lockdep 死锁检测：**

通过 `dma_fence_begin_signalling()` 和 `dma_fence_end_signalling()` 注解 fence 发信号的临界区，让 lockdep 能够检测 `dma_fence_wait()` 与 `dma_fence_signal()` 之间的死锁。

### 3.3 dma-fence-array (`dma-fence-array.c`)

将多个 fence 聚合为一个，支持 **全部完成才信号** 和 **任一完成即信号** 两种模式。

```mermaid
graph LR
    F1[fence 1] -->|"callback"| ARR[dma_fence_array]
    F2[fence 2] -->|"callback"| ARR
    F3[fence 3] -->|"callback"| ARR
    ARR -->|"所有完成时<br/>signal()"| OUT[聚合结果]

    style ARR fill:#f9f,stroke:#333
```

**工作机制：**
1. `enable_signaling` 时为每个子 fence 注册回调
2. 每个子 fence 完成时，原子递减 `num_pending` 计数器
3. 当 `num_pending` 降为 0 时，通过 `irq_work` 发信号
4. 错误传播：第一个子 fence 的错误会被记录到聚合结果中

### 3.4 dma-fence-chain (`dma-fence-chain.c`)

将 fence 按时序链接成链，支持 64-bit 序列号，用于时间线同步。

```mermaid
graph LR
    C1["chain node<br/>seqno=100"] -->|"prev"| C2["chain node<br/>seqno=101"]
    C2 -->|"prev"| C3["chain node<br/>seqno=102"]
    C3 -->|"prev"| C4["chain node<br/>seqno=103"]

    C1 -.->|"contained"| F1[fence A]
    C2 -.->|"contained"| F2[fence B]
    C3 -.->|"contained"| F3[fence C]
    C4 -.->|"contained"| F4[fence D]
```

**RCU 垃圾回收：** 已信号的 chain 节点会被跳过并通过 RCU 安全释放，避免遍历已完成的节点。

### 3.5 dma-fence-unwrap (`dma-fence-unwrap.c`)

提供 fence 容器（array、chain）的扁平化解包和合并工具：

- `dma_fence_unwrap_first/next`：迭代嵌套容器中的所有叶 fence
- `dma_fence_merge()`：合并多个 fence 为一个（自动去重同 context fence）

### 3.6 dma-resv 预留对象 (`dma-resv.c`)

管理关联到一个共享资源（如缓冲区）的所有 fence，是隐式同步的核心。

**Fence 使用级别：**

| 级别 | 枚举值 | 说明 |
|------|--------|------|
| KERNEL | 0 | 内核内部使用，不暴露给用户态 |
| WRITE | 1 | 独占写操作 |
| READ | 2 | 共享读操作 |
| BOOKKEEP | 3 | 记账用途，包含所有 fence |

**RCU 保护的迭代器：**

```mermaid
sequenceDiagram
    participant Reader as 无锁读者
    participant Writer as 写者 (持锁)

    Note over Writer: dma_resv_lock()
    Writer->>Writer: reserve_fences()
    Writer->>Writer: add_fence(new_fence)
    Note over Writer: dma_resv_unlock()

    Note over Reader: RCU read lock
    Reader->>Reader: iter_first() → 遍历 fences
    Note over Reader: 若期间 list 被替换 → 自动 restart
    Note over Reader: RCU read unlock
```

### 3.7 dma-heap 分配框架 (`dma-heap.c`)

为用户态提供统一的缓冲区分配接口，每个 heap 注册为 `/dev/dma_heap/<name>` 字符设备。

```mermaid
graph TD
    USER["用户态 open(/dev/dma_heap/system)"]
    USER -->|"DMA_HEAP_IOCTL_ALLOC"| IOCTL[dma_heap_ioctl]
    IOCTL -->|"heap->ops->allocate()"| ALLOC[dma_heap_buffer_alloc]
    ALLOC -->|"PAGE_ALIGN(len)"| SIZE[页对齐大小]
    SIZE --> ALLOC2[具体 heap 实现]
    ALLOC2 -->|"dma_buf_export()"| DMBUF[dma_buf]
    DMBUF -->|"dma_buf_fd()"| FD[返回 fd 给用户态]
```

### 3.8 sync_file (`sync_file.c`)

将 dma-fence 包装为文件描述符，支持用户态显式同步。

**IOCTL 接口：**

| 命令 | 说明 |
|------|------|
| `SYNC_IOC_MERGE` | 合并两个 sync_file 为一个新的 |
| `SYNC_IOC_FILE_INFO` | 查询 fence 状态、时间戳、驱动信息 |
| `SYNC_IOC_SET_DEADLINE` | 设置截止时间提示 |

**用户态同步模型：**

```mermaid
sequenceDiagram
    participant GPU as GPU 驱动
    participant APP as 应用程序
    participant DISP as 显示驱动

    GPU->>GPU: 提交渲染任务
    GPU->>APP: sync_file fd (out_fence)
    APP->>APP: poll(fd, POLLIN) / read(fd)
    GPU-->>APP: fence signal → 可读
    APP->>DISP: 传递 fd 作为 in_fence
    DISP->>DISP: 等待 fd 对应 fence 完成后开始显示
```

### 3.9 内存后端实现

#### system_heap (`heaps/system_heap.c`)

使用内核 buddy 分配器分配 **非连续物理内存**，通过 scatter-gather table 描述。

```mermaid
graph LR
    REQ[分配请求<br/>N 页] --> O8["尝试 order-8<br/>1MB 连续页"]
    O8 -->|"失败"| O4["尝试 order-4<br/>64KB 连续页"]
    O4 -->|"失败"| O0["尝试 order-0<br/>4KB 页"]
    O0 -->|"失败"| ERR[ENOMEM]
    O8 -->|"成功"| SG[构建 sg_table]
    O4 -->|"成功"| SG
    O0 -->|"成功"| SG
    SG --> EXPORT[dma_buf_export]
```

#### cma_heap (`heaps/cma_heap.c`)

使用 **CMA（连续内存分配器）** 分配物理连续内存，适用于对 DMA 地址连续性有要求的设备。

#### udmabuf (`udmabuf.c`)

将用户态 memfd 的页面 pin 住后转换为 dma-buf，适用于 QEMU 虚拟化场景。

```mermaid
sequenceDiagram
    participant APP as 用户进程
    participant UDM as /dev/udmabuf
    participant MEMFD as memfd
    participant DMABUF as dma-buf

    APP->>MEMFD: memfd_create() + 写入数据
    APP->>UDM: ioctl(UDMABUF_CREATE, memfd, offset, size)
    UDM->>MEMFD: check_memfd_seals()
    UDM->>MEMFD: memfd_pin_folios() → pin 住页面
    UDM->>DMABUF: dma_buf_export()
    UDM-->>APP: 返回 dmabuf fd
    APP->>APP: 传递 fd 给 GPU/VFIO 等设备
```

### 3.10 DRM PRIME (`drm_prime.c`)

DRM 子系统与 dma-buf 的桥接层，支持：
- **Export**：将 GEM buffer object 导出为 dma-buf FD
- **Import**：将 dma-buf FD 导入为 GEM buffer object
- 通过 rbtree 缓存 handle↔dmabuf 映射，确保同一对象只对应一个 handle

## 4. 关键数据结构

### 4.1 struct dma_buf (核心缓冲区对象)

```c
struct dma_buf {
    size_t size;                      // 缓冲区字节大小
    struct file *file;                // 关联的文件对象
    struct list_head attachments;     // 所有 device attachment 链表
    const struct dma_buf_ops *ops;    // 操作函数表
    struct mutex lock;                // 自旋锁
    void *priv;                       // 导出者私有数据

    struct dma_resv *resv;            // 预留对象（包含隐式 fence）

    wait_queue_head_t poll;           // poll 等待队列
    struct dma_buf_poll_cb_t cb_in;   // 读方向 poll 回调
    struct dma_buf_poll_cb_t cb_out;  // 写方向 poll 回调

    struct list_head list_node;       // 全局 dma-buf 列表节点
    char *name;                       // 调试用名称
    spinlock_t name_lock;             // name 访问锁
    struct module *owner;             // 拥有模块
    int vmapping_counter;             // 内核映射引用计数
};
```

### 4.2 struct dma_fence (同步原语)

```c
struct dma_fence {
    spinlock_t *lock;                 // 自旋锁（必须 irq-safe）
    const struct dma_fence_ops *ops;  // 操作函数表
    union {
        struct kref refcount;         // 引用计数
    };
    struct rcu_head rcu;              // RCU 释放头

    u64 context;                      // 执行上下文 ID（全局唯一）
    u64 seqno;                        // 上下文内序列号
    unsigned long flags;              // 状态标志
    ktime_t timestamp;                // 信号时间戳
    int error;                        // 完成错误码

    struct list_head cb_list;         // 回调链表
};
```

### 4.3 struct dma_resv (预留对象)

```c
struct dma_resv {
    struct ww_mutex lock;                     // 可重入互斥锁
    struct dma_resv_list __rcu *fences;       // RCU 保护的 fence 数组
};

// 内部 fence 列表结构
struct dma_resv_list {
    struct rcu_head rcu;
    u32 num_fences;                 // 当前 fence 数量
    u32 max_fences;                 // 预分配最大数量
    struct dma_fence __rcu *table[];// fence 指针 + usage 标志
};
// table[i] 低 2 位存储 dma_resv_usage 枚举值
```

### 4.4 struct dma_fence_array (fence 聚合)

```c
struct dma_fence_array {
    struct dma_fence base;          // 基类 fence
    spinlock_t lock;                // 独立锁
    atomic_t num_pending;           // 等待完成的子 fence 数
    struct dma_fence **fences;      // 子 fence 数组
    unsigned int num_fences;        // 子 fence 数量
    struct irq_work work;           // 信号通知 work
    struct dma_fence_array_cb callbacks[]; // 每个 fence 的回调
};
```

### 4.5 struct dma_fence_chain (fence 时序链)

```c
struct dma_fence_chain {
    struct dma_fence base;          // 基类 fence
    spinlock_t lock;
    struct dma_fence_cb cb;         // 当前等待的回调
    struct irq_work work;           // 重新 arm 回调的 work
    struct dma_fence __rcu *prev;   // 前一个 chain 节点
    struct dma_fence *fence;        // 包含的实际 fence
    uint64_t prev_seqno;            // 前节点的 seqno
};
```

### 4.6 struct dma_heap (堆设备)

```c
struct dma_heap {
    const char *name;               // 堆名称（如 "system"）
    const struct dma_heap_ops *ops; // 分配操作
    void *priv;                     // 堆私有数据
    dev_t heap_devt;                // 设备号
    struct list_head list;          // 全局堆列表
    struct cdev heap_cdev;          // 字符设备
};
```

### 4.7 struct dma_buf_ops (操作函数表)

```c
struct dma_buf_ops {
    int (*attach)(struct dma_buf *, struct dma_buf_attachment *);
    void (*detach)(struct dma_buf *, struct dma_buf_attachment *);
    struct sg_table *(*map_dma_buf)(struct dma_buf_attachment *,
                                     enum dma_data_direction);
    void (*unmap_dma_buf)(struct dma_buf_attachment *,
                           struct sg_table *, enum dma_data_direction);
    void (*release)(struct dma_buf *);
    int (*mmap)(struct dma_buf *, struct vm_area_struct *);
    int (*vmap)(struct dma_buf *, struct iosys_map *);
    void (*vunmap)(struct dma_buf *, struct iosys_map *);
    int (*begin_cpu_access)(struct dma_buf *, enum dma_data_direction);
    int (*end_cpu_access)(struct dma_buf *, enum dma_data_direction);
};
```

### 4.8 struct dma_fence_ops (fence 操作函数表)

```c
struct dma_fence_ops {
    const char *(*get_driver_name)(struct dma_fence *);
    const char *(*get_timeline_name)(struct dma_fence *);
    bool (*enable_signaling)(struct dma_fence *);
    bool (*signaled)(struct dma_fence *);
    void (*wait)(struct dma_fence *, bool intr, signed long timeout);
    void (*release)(struct dma_fence *);
    void (*fence_value_str)(struct dma_fence *, char *buf, size_t);
    void (*timeline_value_str)(struct dma_fence *, char *buf, size_t);
    void (*set_deadline)(struct dma_fence *, ktime_t deadline);
};
```

## 5. 关键链路分析

### 5.1 用户态缓冲区分配全链路

```mermaid
sequenceDiagram
    participant App
    participant HeapDev as /dev/dma_heap/system
    participant Heap as system_heap
    participant DB as dma_buf
    participant Buddy as Buddy Alloc

    App->>HeapDev: open("/dev/dma_heap/system")
    App->>HeapDev: ioctl(DMA_HEAP_IOCTL_ALLOC, {len=1MB})
    HeapDev->>HeapDev: dma_heap_ioctl_allocate()
    HeapDev->>HeapDev: dma_heap_buffer_alloc(heap, len)
    HeapDev->>Heap: heap->ops->allocate() → system_heap_allocate()

    loop 按需分配页块
        Heap->>Buddy: alloc_pages(order=8) → 1MB
        alt order=8 成功
            Heap->>Heap: 添加到 sg_table
        else order=8 失败
            Heap->>Buddy: alloc_pages(order=4) → 64KB
            Heap->>Buddy: alloc_pages(order=0) → 4KB
        end
    end

    Heap->>DB: dma_buf_export(exp_info)
    DB->>DB: get_unused_fd_flags()
    DB->>DB: anon_inode_getfile("dmabuf", fops, dmabuf)
    DB->>DB: fd_install(fd, file)
    DB-->>HeapDev: dma_buf *
    HeapDev-->>App: fd

    Note over App: 后续可通过 mmap(fd) 映射到用户态
```

### 5.2 跨设备缓冲区共享链路

```mermaid
sequenceDiagram
    participant GPU as GPU 驱动
    participant DB as dma_buf
    participant DISP as 显示控制器

    GPU->>DB: dma_buf_attach(dmabuf, gpu_device)
    DB->>DB: ops->attach() → 创建 attachment
    GPU->>DB: dma_buf_map_attachment(attachment, DMA_TO_DEVICE)
    DB->>DB: ops->map_dma_buf() → dma_map_sgtable()
    DB-->>GPU: sg_table (含 DMA 地址)

    Note over GPU: GPU 使用 DMA 地址进行渲染

    GPU->>DISP: 传递 dmabuf fd

    DISP->>DB: dma_buf_attach(dmabuf, display_device)
    DB->>DB: ops->attach()
    DISP->>DB: dma_buf_map_attachment(attachment, DMA_FROM_DEVICE)
    DB->>DB: ops->map_dma_buf() → dma_map_sgtable()
    DB-->>DISP: sg_table

    Note over DISP: 显示控制器使用同一物理内存
```

### 5.3 隐式同步链路 (dma-resv)

```mermaid
sequenceDiagram
    participant GPU as GPU 驱动
    participant RESV as dma_resv
    participant FENCE as dma_fence
    participant DISP as 显示控制器

    GPU->>FENCE: 创建 out_fence (seqno=N)
    GPU->>RESV: dma_resv_add_fence(resv, out_fence, WRITE)
    Note over RESV: WRITE fence 表示 GPU 正在写缓冲区

    DISP->>RESV: dma_resv_lock(resv)
    DISP->>RESV: dma_resv_for_each_fence(resv, READ, fence)
    Note over DISP: 发现 WRITE fence 未信号

    DISP->>FENCE: dma_fence_wait(out_fence)
    Note over GPU: GPU 完成渲染
    GPU->>FENCE: dma_fence_signal(out_fence)
    FENCE-->>DISP: 唤醒！
    Note over DISP: 可以安全读取缓冲区
```

### 5.4 显式同步链路 (sync_file)

```mermaid
sequenceDiagram
    participant GPU as GPU 驱动
    participant SF_OUT as out_fence (sync_file)
    participant APP as 应用程序
    participant SF_IN as in_fence (sync_file)
    participant DISP as 显示驱动

    GPU->>SF_OUT: sync_file_create(fence)
    GPU-->>APP: 返回 out_fence fd

    APP->>APP: poll(out_fence_fd, POLLIN)
    Note over APP: 阻塞直到 GPU 完成
    GPU->>SF_OUT: dma_fence_signal()
    SF_OUT-->>APP: poll 返回 EPOLLIN

    APP->>SF_IN: SYNC_IOC_MERGE(out_fence, extra_fence)
    SF_IN-->>APP: merged_fence fd
    APP->>DISP: 传递 merged_fence 作为 in_fence
    DISP->>SF_IN: sync_file_get_fence(fd)
    DISP->>DISP: 等待 merged_fence 完成后开始扫描
```

### 5.5 dma-buf 生命周期管理

```mermaid
graph TD
    CREATE["dma_buf_export()<br/>创建 dma_buf"] -->|"get_unused_fd_flags()"| FD[分配 fd]
    CREATE -->|"anon_inode_getfile()"| FILE[创建 file 对象]
    CREATE -->|"__dma_buf_list_add()"| LIST[加入全局列表]

    FD -->|"用户态持有 fd"| REF[refcount++]
    FILE -->|"其他内核模块引用"| REF

    REF -->|"dma_buf_get()"| REF2[refcount++]
    REF2 -->|"dma_buf_put()"| DEC[refcount--]
    DEC -->|"refcount > 0"| REF2

    DEC -->|"refcount = 0"| RELEASE[dentry_ops.d_release]
    RELEASE -->|"ops->release()"| DRV[驱动释放私有数据]
    RELEASE -->|"dma_resv_fini()"| RESV_FINI[清理 resv]
    RELEASE -->|"module_put() + kfree()"| FREE[释放 dma_buf]

    style CREATE fill:#bbf
    style RELEASE fill:#fbb
    style FREE fill:#f99
```

## 6. 用户态使用方式

### 6.1 通过 dma-heap 接口分配（推荐）

```c
// 1. 打开堆设备
int heap_fd = open("/dev/dma_heap/system", O_RDONLY);

// 2. 分配缓冲区
struct dma_heap_allocation_data alloc = {
    .len = 1024 * 1024,      // 1MB
    .fd_flags = O_RDWR | O_CLOEXEC,
    .heap_flags = 0,
};
ioctl(heap_fd, DMA_HEAP_IOCTL_ALLOC, &alloc);
int dmabuf_fd = alloc.fd;

// 3. mmap 到用户态
void *ptr = mmap(NULL, 1024*1024, PROT_READ|PROT_WRITE, MAP_SHARED, dmabuf_fd, 0);

// 4. CPU 缓存同步
struct dma_buf_sync sync = { .flags = DMA_BUF_SYNC_START | DMA_BUF_SYNC_RW };
ioctl(dmabuf_fd, DMA_BUF_IOCTL_SYNC, &sync);
// ... 读写 ptr ...
sync.flags = DMA_BUF_SYNC_END | DMA_BUF_SYNC_RW;
ioctl(dmabuf_fd, DMA_BUF_IOCTL_SYNC, &sync);

// 5. 传递给设备驱动（通过特定驱动 ioctl）
// 6. 显式同步
struct sync_fence_info info;
struct sync_file_info fi = { .num_fences = 0 };
ioctl(dmabuf_fd, DMA_BUF_IOCTL_SYNC_FILE, &fi);  // 导出 fence
poll(fi.fence, POLLIN, -1);  // 等待设备操作完成
```

### 6.2 通过 Android libdmabufheap（封装库）

```cpp
BufferAllocator alloc;
// 自动探测 dma_heap 和 legacy ion
int fd = alloc.Alloc("system", size);
int fd = alloc.AllocSystem(true /*cpu_access*/, size);
alloc.CpuSyncStart(fd, SyncType::kSyncReadWrite);
alloc.CpuSyncEnd(fd, SyncType::kSyncReadWrite);
```

**自动回退机制：**

```mermaid
graph TD
    ALLOC["alloc.Alloc('system', size)"] -->|"尝试"| DMAHEAP["open(/dev/dma_heap/system)"]
    DMAHEAP -->|"成功"| DALLOC["DMA_HEAP_IOCTL_ALLOC"]
    DMAHEAP -->|"失败(ENOENT)"| IONCHECK{"/dev/ion 可用?"}
    IONCHECK -->|"是"| IONALLOC["ion_alloc_fd()"]
    IONCHECK -->|"否"| FAIL["返回错误"]
```

### 6.3 通过 udmabuf 接口（QEMU 场景）

```c
// 1. 创建 memfd 并写入数据
int memfd = memfd_create("guest-fb", MFD_ALLOW_SEALING);
ftruncate(memfd, size);
write(memfd, data, size);
fcntl(memfd, F_ADD_SEALS, F_SEAL_SHRINK | F_SEAL_WRITE | F_SEAL_FUTURE_WRITE);

// 2. 转换为 dmabuf
struct udmabuf_create create = { .memfd = memfd, .offset = 0, .size = size };
int udmabuf_fd = open("/dev/udmabuf", O_RDWR);
ioctl(udmabuf_fd, UDMABUF_CREATE, &create);
int dmabuf_fd = create.fd;  // 可传递给 VFIO/GPU
```

## 7. 内核模块使用方式

### 7.1 导出 dma-buf

```c
// 1. 实现 dma_buf_ops
static const struct dma_buf_ops my_ops = {
    .attach = my_attach,
    .detach = my_detach,
    .map_dma_buf = my_map,
    .unmap_dma_buf = my_unmap,
    .release = my_release,
    .mmap = my_mmap,
    .begin_cpu_access = my_begin_cpu,
    .end_cpu_access = my_end_cpu,
};

// 2. 导出
DEFINE_DMA_BUF_EXPORT_INFO(exp_info);
exp_info.ops = &my_ops;
exp_info.size = buf_size;
exp_info.priv = my_private;
struct dma_buf *dmabuf = dma_buf_export(&exp_info);
int fd = dma_buf_fd(dmabuf, O_CLOEXEC);
```

### 7.2 导入 dma-buf 并 DMA 映射

```c
struct dma_buf *dmabuf = dma_buf_get(fd);
struct dma_buf_attachment *attach = dma_buf_attach(dmabuf, my_dev);
struct sg_table *sgt = dma_buf_map_attachment(attach, DMA_TO_DEVICE);
// 使用 sgt 中的 DMA 地址
// ...
dma_buf_unmap_attachment(attach, sgt, DMA_TO_DEVICE);
dma_buf_detach(dmabuf, attach);
dma_buf_put(dmabuf);
```

### 7.3 注册自定义 dma-heap

```c
static struct dma_buf *my_heap_allocate(struct dma_heap *heap,
                                         unsigned long len,
                                         u32 fd_flags, u64 heap_flags) {
    // 分配内存、构建 sg_table、调用 dma_buf_export
}

static const struct dma_heap_ops my_heap_ops = {
    .allocate = my_heap_allocate,
};

static int __init my_heap_init(void) {
    struct dma_heap_export_info exp_info = {
        .name = "my_custom_heap",
        .ops = &my_heap_ops,
        .priv = NULL,
    };
    return PTR_ERR_OR_ZERO(dma_heap_add(&exp_info));
}
module_init(my_heap_init);
```

## 8. 构建系统

```mermaid
graph TD
    KCONFIG["Kconfig"] -->|"CONFIG_DMABUF_HEAPS"| HEAP["dma-heap.c + heaps/*.c"]
    KCONFIG -->|"CONFIG_SYNC_FILE"| SF["sync_file.c"]
    KCONFIG -->|"CONFIG_SW_SYNC"| SWSYNC["sw_sync.c"]
    KCONFIG -->|"CONFIG_UDMABUF"| UDMABUF["udmabuf.c"]
    KCONFIG -->|"CONFIG_DMABUF_SELFTESTS"| SELFTEST["selftest.c + st-*.c"]

    subgraph "无条件编译"
        CORE["dma-buf.c<br/>dma-fence.c<br/>dma-resv.c<br/>dma-fence-array.c<br/>dma-fence-chain.c<br/>dma-fence-unwrap.c<br/>dma-buf-mapping.c"]
    end
```

## 9. 同步模型对比

```mermaid
graph TB
    subgraph "隐式同步 (Implicit)"
        I1[dma_buf.resv] --> I2[包含 WRITE/READ fence]
        I2 --> I3[设备自动等待相关 fence]
        I3 --> I4[用户态通过 poll() 查询状态]
    end

    subgraph "显式同步 (Explicit)"
        E1[驱动创建 out_fence] --> E2["sync_file_create()"]
        E2 --> E3[返回 sync_file fd 给用户态]
        E3 --> E4[用户态传递 fd 给下一设备]
        E4 --> E5[下一设备将 fd 作为 in_fence]
        E5 --> E6[设备自动等待 fence]
    end

    style I1 fill:#bfb
    style E1 fill:#fbb
```

## 10. 文件清单与角色

| 文件 | 角色 |
|------|------|
| `dma-buf.c` | 核心：dma_buf 结构体、文件操作、导出/导入、poll、mmap |
| `dma-fence.c` | 核心：dma_fence 同步原语、signal/wait/callback |
| `dma-resv.c` | 核心：dma_resv 预留对象、fence 容器管理 |
| `dma-fence-array.c` | 扩展：多 fence 聚合容器 |
| `dma-fence-chain.c` | 扩展：时序链 fence 容器（64-bit seqno） |
| `dma-fence-unwrap.c` | 工具：fence 解包、去重、合并 |
| `dma-heap.c` | 框架：用户态分配入口（/dev/dma_heap/） |
| `sync_file.c` | 接口：用户态显式同步（sync_file fd） |
| `sw_sync.c` | 调试：软件 sync timeline（debugfs） |
| `udmabuf.c` | 特殊：用户态内存转 dma-buf |
| `heaps/system_heap.c` | 后端：系统内存分配器（非连续） |
| `heaps/cma_heap.c` | 后端：CMA 连续内存分配器 |
| `drm_prime.c` | 桥接：DRM GEM ↔ dma-buf 转换 |
| `libdmabufheap/` | 用户态：Android 分配封装库 |
