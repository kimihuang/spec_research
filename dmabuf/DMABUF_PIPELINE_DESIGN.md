# DMA-BUF 多媒体 Pipeline 设计：4路 Camera + VPU + NPU

## 1. 场景概述

本文描述如何利用 DMA-BUF 零拷贝共享和同步机制，构建一个高效的多媒体处理 Pipeline，串联以下组件：

- **4路 Camera（ISP）**：4 个摄像头并行采集，输出 YUV/RAW 帧
- **VPU（Video Processing Unit）**：视频编解码（H.264/H.265/AV1）
- **NPU（Neural Processing Unit）**：AI 推理（目标检测、语义分割等）

### 核心设计原则

| 原则 | 说明 |
|------|------|
| 零拷贝 | 全链路通过 fd 传递 dma-buf，无内存拷贝 |
| 双模式同步 | Pipeline 内部用隐式同步（dma-resv），跨子系统用显式同步（sync_file） |
| 池化分配 | 预分配帧缓冲池，避免运行时分配延迟 |
| Pin 最小化 | 只有正在使用的缓冲区才 pin 在设备地址空间 |

## 2. 整体架构

```mermaid
graph TB
    subgraph "采集层 (Capture)"
        CAM0["Camera 0<br/>(ISP Output)"]
        CAM1["Camera 1<br/>(ISP Output)"]
        CAM2["Camera 2<br/>(ISP Output)"]
        CAM3["Camera 3<br/>(ISP Output)"]
    end

    subgraph "帧缓冲池 (Frame Buffer Pool)"
        POOL["dma_heap 分配<br/>system_heap / cma_heap<br/>预分配 N 帧 x 4路"]
    end

    subgraph "处理层 (Processing)"
        PRE["预处理 (CPU/GPU)<br/>resize / normalize / format convert"]
        VPU_ENC["VPU Encoder<br/>H.264/H.265 编码"]
        VPU_DEC["VPU Decoder<br/>视频解码"]
        NPU["NPU Inference<br/>检测/分割/分类"]
        POST["后处理 (CPU/GPU)<br/>NMS / draw / composite"]
    end

    subgraph "输出层 (Output)"
        NET["网络推流<br/>(encoded bitstream)"]
        STORE["本地存储<br/>(encoded file)"]
        DISP["显示输出<br/>(HDMI/DSI)"]
        APP["应用层<br/>(推理结果)"]
    end

    POOL -->|"dmabuf fd"| CAM0
    POOL -->|"dmabuf fd"| CAM1
    POOL -->|"dmabuf fd"| CAM2
    POOL -->|"dmabuf fd"| CAM3

    CAM0 -->|"out_fence"| PRE
    CAM1 -->|"out_fence"| PRE
    CAM2 -->|"out_fence"| PRE
    CAM3 -->|"out_fence"| PRE

    PRE -->|"out_fence"| NPU
    PRE -->|"out_fence"| VPU_ENC

    NPU -->|"out_fence"| POST
    VPU_ENC -->|"encoded bs"| NET
    VPU_ENC -->|"encoded bs"| STORE

    POST -->|"out_fence"| DISP

    VPU_DEC -->|"decoded frame (dmabuf)"| PRE
    STORE -->|"recorded file"| VPU_DEC

    style POOL fill:#e1f5fe
    style NPU fill:#fff3e0
    style VPU_ENC fill:#e8f5e9
    style VPU_DEC fill:#e8f5e9
```

## 3. Pipeline 模式详解

### 3.1 模式一：实时采集 + AI 推理 + 编码存储

最典型的多路摄像头场景：4路摄像头同时采集，画面经 AI 推理处理后编码存储。

```mermaid
sequenceDiagram
    participant CAM as Camera (ISP)
    participant POOL as Frame Pool
    participant SCHED as Pipeline Scheduler
    participant PRE as Preprocess (CPU)
    participant NPU as NPU
    participant VPU as VPU Encoder
    participant FS as File System

    Note over SCHED: 初始化阶段
    SCHED->>POOL: dma_heap_alloc() x N 帧 x 4路
    SCHED->>SCHED: 构建帧缓冲池 (ring buffer)

    loop 每帧 (4路并行)
        Note over CAM,CAM: Camera 0~3 并行采集

        POOL->>CAM: dequeue 空闲 dmabuf (fd)
        CAM->>CAM: ISP 输出写入 dmabuf
        CAM->>CAM: dma_resv_add_fence(WRITE fence)

        CAM->>SCHED: 传递 dmabuf fd + out_fence (sync_file)
        SCHED->>PRE: 传递 dmabuf fd + in_fence

        Note over PRE: CPU 预处理
        PRE->>PRE: DMA_BUF_IOCTL_SYNC(START)
        PRE->>PRE: resize / color convert / normalize
        PRE->>PRE: DMA_BUF_IOCTL_SYNC(END)
        PRE->>PRE: dma_resv_add_fence(WRITE fence)
        PRE->>SCHED: 传递 dmabuf fd + out_fence

        par 并行分支：AI 推理
            SCHED->>NPU: 传递 dmabuf fd + in_fence
            NPU->>NPU: 推理计算
            NPU->>SCHED: 返回结果 + out_fence
        and 并行分支：视频编码
            SCHED->>VPU: 传递 dmabuf fd + in_fence
            VPU->>VPU: H.264/H.265 编码
            VPU->>FS: 写入编码码流
            VPU->>SCHED: out_fence
        end

        SCHED->>POOL: 回收 dmabuf 到池中
    end
```

### 3.2 模式二：视频文件解码 + AI 推理

离线场景：读取录制文件，VPU 解码后送 NPU 推理。

```mermaid
sequenceDiagram
    participant FS as File System
    participant VPU as VPU Decoder
    participant POOL as Frame Pool
    participant PRE as Preprocess
    participant NPU as NPU
    participant APP as Application

    loop 解码循环
        FS->>VPU: 压缩码流 (H.264 NAL)
        VPU->>POOL: 获取空闲 dmabuf
        VPU->>VPU: 解码写入 dmabuf
        VPU->>VPU: dma_resv_add_fence(WRITE fence)
        VPU->>PRE: dmabuf fd + out_fence (sync_file)

        PRE->>PRE: DMA_BUF_SYNC_START (CPU缓存失效)
        PRE->>PRE: 格式转换 NV12→RGB / normalize
        PRE->>PRE: DMA_BUF_SYNC_END (CPU缓存刷回)
        PRE->>NPU: dmabuf fd + out_fence

        NPU->>NPU: 等待 in_fence 完成
        NPU->>NPU: AI 推理
        NPU->>NPU: 输出结果到输出 dmabuf
        NPU->>NPU: dma_resv_add_fence(OUTPUT, WRITE fence)
        NPU->>APP: 输出 dmabuf fd + out_fence

        APP->>APP: 读取推理结果 (mmap / CPU sync)
        APP->>POOL: 回收 dmabuf
    end
```

### 3.3 模式三：多路拼接 + 环视 + AI 检测

车载场景：4路摄像头画面拼接成环视，同时做目标检测。

```mermaid
graph LR
    subgraph "4路 Camera 输入"
        F0["Cam0<br/>dmabuf_0"]
        F1["Cam1<br/>dmabuf_1"]
        F2["Cam2<br/>dmabuf_2"]
        F3["Cam3<br/>dmabuf_3"]
    end

    subgraph "GPU 合成"
        STITCH["GPU Stitch<br/>环视拼接<br/>输出: dmabuf_stitch"]
    end

    subgraph "NPU 检测 (4路并行)"
        D0["NPU Detect<br/>dmabuf_0"]
        D1["NPU Detect<br/>dmabuf_1"]
        D2["NPU Detect<br/>dmabuf_2"]
        D3["NPU Detect<br/>dmabuf_3"]
    end

    subgraph "后处理"
        OVERLAY["GPU Overlay<br/>绘制检测框<br/>输出: dmabuf_final"]
    end

    F0 --> STITCH
    F1 --> STITCH
    F2 --> STITCH
    F3 --> STITCH

    F0 --> D0
    F1 --> D1
    F2 --> D2
    F3 --> D3

    STITCH -->|"环视帧"| OVERLAY
    D0 -->|"检测结果"| OVERLAY
    D1 -->|"检测结果"| OVERLAY
    D2 -->|"检测结果"| OVERLAY
    D3 -->|"检测结果"| OVERLAY

    OVERLAY -->|"display dmabuf"| DISP["Display / HDMI"]

    style STITCH fill:#e8f5e9
    style OVERLAY fill:#fff3e0
    style DISP fill:#fce4ec
```

## 4. DMA-BUF 使用详解

### 4.1 帧缓冲池设计

```c
/* 帧缓冲池：预分配避免运行时延迟 */
struct frame_pool {
    int heap_fd;                  /* /dev/dma_heap/system 的 fd */
    struct dmabuf_frame *frames;  /* 预分配的帧数组 */
    int count;                    /* 帧数量 */
    int stride;                   /* 单帧字节数 */
    struct mutex lock;
};

struct dmabuf_frame {
    int dmabuf_fd;                /* dma-buf 文件描述符 */
    void *cpu_addr;               /* mmap 映射的 CPU 地址 */
    size_t size;
    bool in_use;
    struct sync_file *out_fence;  /* 最近一次操作的 out_fence */
};
```

```mermaid
graph TD
    subgraph "初始化"
        INIT["open(/dev/dma_heap/system)"] --> ALLOC["dma_heap_alloc() x N"]
        ALLOC --> MMAP["mmap() 每帧"]
        MMAP --> READY["帧池就绪"]
    end

    subgraph "运行时流转"
        READY --> DEQ["dequeue: 取空闲帧"]
        DEQ --> FILL["设备填充数据<br/>(Camera/VPU/GPU)"]
        FILL --> ENQ["enqueue: 帧入处理队列"]
        ENQ --> PROC["NPU/VPU/Display 处理"]
        PROC --> REL["release: 回收到池"]
        REL --> DEQ
    end

    style READY fill:#c8e6c9
    style FILL fill:#bbdefb
    style PROC fill:#ffe0b2
```

**池初始化代码：**

```c
int frame_pool_init(struct frame_pool *pool, int count, int width, int height)
{
    struct dma_heap_allocation_data alloc = { 0 };
    int i, ret;

    pool->heap_fd = open("/dev/dma_heap/system", O_RDONLY | O_CLOEXEC);
    pool->count = count;
    pool->stride = PAGE_ALIGN(width * height * 3 / 2);  /* NV12 格式 */
    pool->frames = calloc(count, sizeof(*pool->frames));
    mutex_init(&pool->lock);

    for (i = 0; i < count; i++) {
        alloc.len = pool->stride;
        alloc.fd_flags = O_RDWR | O_CLOEXEC;
        ret = ioctl(pool->heap_fd, DMA_HEAP_IOCTL_ALLOC, &alloc);
        if (ret < 0)
            return ret;

        pool->frames[i].dmabuf_fd = alloc.fd;
        pool->frames[i].size = pool->stride;
        pool->frames[i].cpu_addr = mmap(NULL, pool->stride,
                                         PROT_READ | PROT_WRITE,
                                         MAP_SHARED, alloc.fd, 0);
        if (pool->frames[i].cpu_addr == MAP_FAILED)
            return -ENOMEM;
    }
    return 0;
}
```

### 4.2 Camera → Preprocess 同步（隐式 + 显式混合）

```mermaid
sequenceDiagram
    participant ISP as Camera ISP Driver
    participant DMABUF as dma-buf (resv)
    participant CPU as CPU Preprocess

    Note over ISP: 1. ISP 完成采集

    ISP->>DMABUF: dma_resv_add_fence(isp_fence, WRITE)
    Note over DMABUF: resv 中有 WRITE fence (ISP)

    ISP->>CPU: 传递 dmabuf_fd + sync_file_create(isp_fence)

    Note over CPU: 2. CPU 等待 ISP 完成

    CPU->>CPU: 方式A: poll(sync_fd, POLLIN)
    CPU->>CPU: 方式B: dma_resv_wait_timeout(resv, WRITE)
    CPU->>CPU: 方式C: NPU 驱动内部等待 (in_fence)

    Note over CPU: 3. CPU 开始预处理

    CPU->>DMABUF: ioctl(DMA_BUF_IOCTL_SYNC, SYNC_START | RW)
    Note over DMABUF: invalidate CPU cache
    CPU->>CPU: resize / color convert / normalize
    CPU->>DMABUF: ioctl(DMA_BUF_IOCTL_SYNC, SYNC_END | RW)
    Note over DMABUF: flush CPU cache

    CPU->>DMABUF: dma_resv_add_fence(cpu_fence, WRITE)
    Note over DMABUF: resv 中 WRITE fence 被 CPU fence 替换

    CPU->>NPU: 传递 dmabuf_fd + sync_file_create(cpu_fence)
```

### 4.3 同步 Fence 的传递方式

```mermaid
graph TD
    subgraph "隐式同步 (dma-resv)"
        I1["设备A 写入<br/>add_fence(WRITE)"] --> I2["设备B 读取前<br/>等待 WRITE fence"]
        I2 --> I3["设备B 写入<br/>add_fence(WRITE) 替换"]
        I3 --> I4["设备C 读取前<br/>等待 WRITE fence"]
    end

    subgraph "显式同步 (sync_file)"
        E1["设备A 导出 out_fence<br/>sync_file_create()"] --> E2["返回 fd 给用户态/调度器"]
        E2 --> E3["传递 fd 给设备B<br/>作为 in_fence"]
        E3 --> E4["设备B 内部<br/>dma_fence_wait(in_fence)"]
    end

    subgraph "选择策略"
        S1{"同一 dma-buf 上的<br/>连续操作？"} -->|"是"| S2["隐式同步<br/>(dma-resv 自动管理)"]
        S1 -->|"否 / 跨子系统调度"| S3["显式同步<br/>(sync_file fd 传递)"]
    end

    style I1 fill:#c8e6c9
    style E1 fill:#bbdefb
```

**使用建议：**

| 场景 | 推荐方式 | 原因 |
|------|---------|------|
| ISP → GPU → Display（同一缓冲区直通） | 隐式同步 | 同一 dma-buf 上的连续流水线，dma-resv 自动管理 |
| Camera → NPU（调度器中转） | 显式同步 | 需要用户态调度器协调多路缓冲区 |
| VPU Decode → NPU（同一驱动内） | 隐式同步 | 驱动内部可自动处理 |
| NPU → App 读取结果 | 显式同步 | 用户态需要知道推理何时完成 |
| 多路 Camera 合并输出 | 显式同步 | 需要等待 4 路全部完成后才开始合成 |

### 4.4 4路 Camera 并行 + NPU 的完整同步设计

```mermaid
sequenceDiagram
    participant C0 as Cam 0
    participant C1 as Cam 1
    participant C2 as Cam 2
    participant C3 as Cam 3
    participant SCHED as Scheduler
    participant NPU as NPU

    Note over C0,C3: 4路 Camera 并行采集（各自独立 dma-buf）

    par Camera 0
        C0->>C0: 写入 dmabuf_0
        C0->>C0: out_fence_0 = sync_file_create(isp_fence_0)
    and Camera 1
        C1->>C1: 写入 dmabuf_1
        C1->>C1: out_fence_1 = sync_file_create(isp_fence_1)
    and Camera 2
        C2->>C2: 写入 dmabuf_2
        C2->>C2: out_fence_2 = sync_file_create(isp_fence_2)
    and Camera 3
        C3->>C3: 写入 dmabuf_3
        C3->>C3: out_fence_3 = sync_file_create(isp_fence_3)
    end

    C0->>SCHED: dmabuf_0 + out_fence_0
    C1->>SCHED: dmabuf_1 + out_fence_1
    C2->>SCHED: dmabuf_2 + out_fence_2
    C3->>SCHED: dmabuf_3 + out_fence_3

    Note over SCHED: 合并 4 路 fence 为一个

    SCHED->>SCHED: merged = SYNC_IOC_MERGE(f0, f1)
    SCHED->>SCHED: merged = SYNC_IOC_MERGE(merged, f2)
    SCHED->>SCHED: merged = SYNC_IOC_MERGE(merged, f3)

    Note over SCHED: 依次提交 4 路到 NPU<br/>每路携带合并后的 in_fence

    SCHED->>NPU: submit(dmabuf_0, in_fence=merged)
    SCHED->>NPU: submit(dmabuf_1, in_fence=merged)
    SCHED->>NPU: submit(dmabuf_2, in_fence=merged)
    SCHED->>NPU: submit(dmabuf_3, in_fence=merged)

    Note over NPU: NPU 内部等待 merged fence<br/>（4路全部采集完成后才开始推理）

    NPU->>NPU: 并行推理 4 路
    NPU->>SCHED: npu_out_fence_0~3

    SCHED->>SCHED: 回收 dmabuf_0~3 到帧池
```

## 5. NPU 推理的 DMA-BUF 交互

### 5.1 NPU 输入输出缓冲区

```mermaid
graph LR
    subgraph "NPU 推理一次前向传播"
        INPUT["输入 dmabuf<br/>(图像帧)<br/>NV12 / RGB / NCHW"] --> MODEL["NPU 模型推理<br/>(固定权重，不在 dmabuf 中)"]
        MODEL --> OUTPUT["输出 dmabuf<br/>(推理结果)<br/>检测框 / 分割图 / 特征图"]
    end

    INPUT_FENCE["in_fence<br/>(等待输入就绪)"] -.->|"wait"| INPUT
    OUTPUT_FENCE["out_fence<br/>(推理完成)"] -.->|"signal"| OUTPUT

    style INPUT fill:#bbdefb
    style OUTPUT fill:#c8e6c9
    style MODEL fill:#fff3e0
```

### 5.2 NPU 多批次推理（Batching）

将 4 路摄像头帧合并为一个 batch 送 NPU，提高利用率：

```mermaid
graph TD
    subgraph "输入: 4路独立 dmabuf"
        B0["dmabuf_0<br/>1920x1080 NV12"]
        B1["dmabuf_1<br/>1920x1080 NV12"]
        B2["dmabuf_2<br/>1920x1080 NV12"]
        B3["dmabuf_3<br/>1920x1080 NV12"]
    end

    subgraph "NPU Batch 输入缓冲区"
        NBATCH["dmabuf_batch<br/>7680x1080 NV12<br/>(或 4x1920x1080 NCHW)"]
    end

    subgraph "NPU"
        NPU_IN["NPU 输入"] --> NPU_CORE["推理 Core"] --> NPU_OUT["NPU 输出<br/>dmabuf_result"]
    end

    B0 -->|"DMA copy / GPU compose"| NBATCH
    B1 -->|"DMA copy / GPU compose"| NBATCH
    B2 -->|"DMA copy / GPU compose"| NBATCH
    B3 -->|"DMA copy / GPU compose"| NBATCH

    NBATCH -->|"in_fence"| NPU_IN
    NPU_OUT -->|"out_fence"| RESULT["推理结果<br/>4路检测框"]

    note1["方案A: GPU 做 batch 拼接<br/>(零拷贝 DMA)"]
    note2["方案B: NPU 驱动内部拼接<br/>(接受多个 dmabuf fd)"]

    style NBATCH fill:#fff9c4
```

**方案 A（推荐）：GPU 做 batch 拼接**

```c
/* GPU 拼接 4 路帧到一个 batch dmabuf，全程零拷贝 */
int gpu_batch_compose(int batch_fd, int cam_fds[4], int width, int height)
{
    /* 1. GPU attach 到 5 个 dmabuf */
    struct dma_buf *batch = dma_buf_get(batch_fd);
    for (int i = 0; i < 4; i++) {
        struct dma_buf *cam = dma_buf_get(cam_fds[i]);
        dma_buf_attach(cam, gpu_device);
        dma_buf_put(cam);
    }
    dma_buf_attach(batch, gpu_device);

    /* 2. GPU DMA map 所有缓冲区 */
    /* 3. GPU 执行 copy/compose kernel */
    /* 4. GPU 输出 add_fence 到 batch dmabuf 的 resv */
    /* 5. 所有操作通过隐式同步自动串行 */
    return 0;
}
```

### 5.3 NPU 推理结果回读

```c
/*
 * NPU 推理完成后，CPU 读取结果的标准流程：
 * 1. 等待 out_fence 完成
 * 2. 执行 CPU 缓存同步
 * 3. 读取数据
 */
int npu_result_read(int result_fd, struct sync_file *out_fence, void *buf, size_t len)
{
    /* 方式1: 通过 sync_file fd 的 poll 等待 */
    struct pollfd pfd = { .fd = dup(sync_file_fd(out_fence)), .events = POLLIN };
    poll(&pfd, 1, -1);  /* 阻塞直到 NPU 完成 */
    close(pfd.fd);

    /* 方式2: 通过 dma-buf 隐式同步导出的 sync_file */
    /* dma_buf_ioctl(dmabuf_fd, DMA_BUF_IOCTL_EXPORT_SYNC_FILE, &arg); */
    /* poll(arg.fd, POLLIN, -1); */

    /* CPU 缓存同步：确保 NPU 写入的数据对 CPU 可见 */
    struct dma_buf_sync sync = { .flags = DMA_BUF_SYNC_START | DMA_BUF_SYNC_READ };
    ioctl(result_fd, DMA_BUF_IOCTL_SYNC, &sync);

    /* 现在可以安全读取结果 */
    void *addr = mmap(NULL, len, PROT_READ, MAP_SHARED, result_fd, 0);
    memcpy(buf, addr, len);

    /* 结束 CPU 访问 */
    sync.flags = DMA_BUF_SYNC_END | DMA_BUF_SYNC_READ;
    ioctl(result_fd, DMA_BUF_IOCTL_SYNC, &sync);

    munmap(addr, len);
    return 0;
}
```

## 6. 内存后端选择

```mermaid
graph TD
    ALLOC{"选择 dma_heap 类型"}

    ALLOC -->|"需要物理连续?"| CONTIG{"是否必须<br/>物理连续?"}

    CONTIG -->|"是"| CMA["CMA Heap<br/>/dev/dma_heap/linux,cma<br/>适用: 老旧 DMA 控制器<br/>某些 ISP/VPU 要求"]
    CONTIG -->|"否"| SYS["System Heap<br/>/dev/dma_heap/system<br/>适用: NPU, GPU, 现代VPU<br/>支持 IOMMU/SMMU"]

    ALLOC -->|"用户态内存转 dmabuf"| UDMA["udmabuf<br/>/dev/udmabuf<br/>适用: QEMU 虚拟化<br/>测试场景"]

    SYS -->|"典型大小"| SIZE1["1920x1080 NV12<br/>≈ 3MB / 帧"]
    CMA -->|"典型大小"| SIZE2["4K NV12<br/>≈ 12MB / 帧"]

    style SYS fill:#c8e6c9
    style CMA fill:#bbdefb
    style UDMA fill:#fff3e0
```

**推荐配置：**

| 组件 | 推荐 Heap | 理由 |
|------|----------|------|
| Camera ISP 输出 | CMA 或 vendor heap | ISP 通常要求物理连续 |
| GPU 中间结果 | system_heap | GPU 有 IOMMU，无需连续 |
| NPU 输入/输出 | system_heap | NPU 有 IOMMU |
| VPU 编码输入 | system_heap | 现代 VPU 支持 scatter-gather |
| VPU 解码输出 | system_heap | 同上 |
| Display 输出 | CMA 或 vendor heap | 显示控制器可能要求连续 |

## 7. Pipeline 调度器设计

```mermaid
stateDiagram-v2
    [*] --> EMPTY: 帧在池中

    EMPTY --> FILLING: dequeue 给 Camera
    FILLING --> FILLED: Camera 写入完成<br/>(out_fence signaled)

    FILLED --> PREPROCESSING: 提交给预处理
    PREPROCESSING --> PREPROCESSED: 预处理完成

    state "并行处理" as PARALLEL {
        PREPROCESSED --> NPU_RUNNING: 提交给 NPU
        PREPROCESSED --> VPU_RUNNING: 提交给 VPU Encoder
        NPU_RUNNING --> NPU_DONE
        VPU_RUNNING --> VPU_DONE
    }

    NPU_DONE --> POSTPROCESSING: 提交给后处理
    VPU_DONE --> DONE: 编码完成
    POSTPROCESSING --> DONE: 后处理完成
    DONE --> EMPTY: 回收到帧池

    note right of PARALLEL: 同一帧可同时<br/>送 NPU 和 VPU<br/>(只读共享)
```

**调度器核心逻辑（伪代码）：**

```c
void pipeline_scheduler_loop(void)
{
    while (running) {
        /* 1. 检查 Camera 输出 */
        for (int cam = 0; cam < 4; cam++) {
            if (camera_frame_ready(cam)) {
                struct dmabuf_frame *frame = frame_pool_dequeue();
                struct sync_file *out_fence = camera_get_out_fence(cam);
                frame->out_fence = out_fence;

                /* 2. 提交预处理 */
                preprocess_submit(frame->dmabuf_fd, out_fence);
            }
        }

        /* 3. 检查预处理完成 */
        while ((frame = preprocess_dequeue())) {
            /* 4. 并行提交 NPU 和 VPU */
            struct sync_file *pre_fence = frame->out_fence;

            /* NPU 推理（需要等预处理完成） */
            npu_submit(frame->dmabuf_fd,
                      sync_file_fd(pre_fence),   /* in_fence */
                      result_pool_dequeue());     /* 输出 dmabuf */

            /* VPU 编码（需要等预处理完成，与 NPU 并行） */
            vpu_encode_submit(frame->dmabuf_fd,
                            sync_file_fd(pre_fence),  /* in_fence */
                            bitstream_fd);
        }

        /* 5. 检查 NPU/VPU 完成，回帧到池 */
        while ((frame = npu_dequeue_done())) {
            /* 读取推理结果，后处理... */
            frame_pool_release(frame);
        }
        while ((frame = vpu_dequeue_done())) {
            frame_pool_release(frame);
        }
    }
}
```

## 8. Fence 合并与等待的最佳实践

### 8.1 多路采集 → 单一 NPU 任务

```mermaid
graph LR
    F0["fence_0"] --> MERGE["SYNC_IOC_MERGE"]
    F1["fence_1"] --> MERGE
    F2["fence_2"] --> MERGE
    F3["fence_3"] --> MERGE
    MERGE --> MF["merged_fence"]
    MF --> NPU["NPU in_fence<br/>(等待4路全部就绪)"]

    style MERGE fill:#fff9c4
```

```c
/* 合并 4 路 Camera fence 为一个 */
int merge_4_cam_fences(int fence_fds[4])
{
    struct sync_merge_data data = { .flags = 0 };
    int merged_fd;

    /* 先合并 fence_0 和 fence_1 */
    data.fd2 = fence_fds[1];
    strncpy(data.name, "cam01", sizeof(data.name));
    int fd = open_sync_file(fence_fds[0]);
    ioctl(fd, SYNC_IOC_MERGE, &data);
    merged_fd = data.fence;
    close(fd);

    /* 合并 fence_2 */
    data.fd2 = fence_fds[2];
    strncpy(data.name, "cam012", sizeof(data.name));
    fd = open_sync_file(merged_fd);
    ioctl(fd, SYNC_IOC_MERGE, &data);
    close(merged_fd);
    merged_fd = data.fence;
    close(fd);

    /* 合并 fence_3 */
    data.fd2 = fence_fds[3];
    strncpy(data.name, "cam0123", sizeof(data.name));
    fd = open_sync_file(merged_fd);
    ioctl(fd, SYNC_IOC_MERGE, &data);
    close(merged_fd);
    merged_fd = data.fence;
    close(fd);

    return merged_fd;
}
```

### 8.2 单帧多消费者（NPU + VPU 并行读）

```mermaid
graph TD
    subgraph "生产者: Camera (WRITE)"
        P1["add_fence(cam_fence, WRITE)"]
    end

    subgraph "消费者: 并行只读"
        C1["NPU: wait(WRITE fence)<br/>然后 add_fence(npu_fence, BOOKKEEP)"]
        C2["VPU: wait(WRITE fence)<br/>然后 add_fence(vpu_fence, BOOKKEEP)"]
    end

    subgraph "回收: 等待所有消费者"
        R1["wait(READ fences)<br/>全部完成后回帧到池"]
    end

    P1 --> C1
    P1 --> C2
    C1 --> R1
    C2 --> R1

    style P1 fill:#ffcdd2
    style C1 fill:#c8e6c9
    style C2 fill:#c8e6c9
    style R1 fill:#bbdefb
```

```c
/*
 * 多消费者场景的同步要点：
 * 1. Camera 写入后添加 WRITE fence
 * 2. NPU 和 VPU 都等待 WRITE fence（可并行，因为只读）
 * 3. NPU/VPU 完成后添加 BOOKKEEP 级别 fence（不阻塞写者）
 * 4. 调度器等待所有 BOOKKEEP fence 完成后回帧
 */

/* 步骤3: 消费者完成后添加 BOOKKEEP fence */
void npu_done_notify(int dmabuf_fd, struct dma_fence *npu_fence)
{
    /* 导入 fence 到 dma-resv，使用 BOOKKEEP 级别 */
    struct dma_buf_import_sync_file arg = {
        .flags = DMA_BUF_SYNC_READ,  /* 读操作 */
        .fd = sync_file_fd_create(npu_fence),
    };
    ioctl(dmabuf_fd, DMA_BUF_IOCTL_IMPORT_SYNC_FILE, &arg);
}

/* 步骤4: 等待所有消费者完成 */
int wait_all_consumers(int dmabuf_fd)
{
    struct dma_buf_export_sync_file arg = {
        .flags = DMA_BUF_SYNC_READ,  /* 等待所有 READ/BOOKKEEP fence */
    };
    /* 导出当前所有 READ 级别 fence */
    ioctl(dmabuf_fd, DMA_BUF_IOCTL_EXPORT_SYNC_FILE, &arg);

    struct pollfd pfd = { .fd = arg.fd, .events = POLLIN };
    poll(&pfd, 1, -1);  /* 阻塞等待 */
    close(arg.fd);
    return 0;
}
```

## 9. 性能优化要点

```mermaid
graph TB
    subgraph "内存优化"
        M1["预分配帧池<br/>避免运行时 alloc/mmap"]
        M2["双层池: 大帧 + 小帧<br/>按分辨率分离"]
        M3["CMA 用于 ISP<br/>System 用于 NPU/VPU"]
    end

    subgraph "同步优化"
        S1["隐式同步用于流水线内部<br/>(dma-resv 自动管理)"]
        S2["显式同步用于跨子系统调度<br/>(sync_file fd 传递)"]
        S3["批量 fence 合并<br/>减少 wait 开销"]
        S4["in_fence 模式<br/>NPU/VPU 驱动内部等待"]
    end

    subgraph "流水线优化"
        P1["双缓冲/三缓冲<br/>流水线并行"]
        P2["NPU batch 推理<br/>提高利用率"]
        P3["GPU 做 resize/compose<br/>释放 CPU/NPU"]
        P4["零拷贝: 全链路 dmabuf fd 传递"]
    end

    style M1 fill:#e8f5e9
    style S1 fill:#e3f2fd
    style P1 fill:#fff3e0
```

### 关键优化建议

| 优化点 | 方法 | 预期效果 |
|--------|------|---------|
| 避免内存拷贝 | 全链路 dma-buf fd 传递，不经过 CPU 中转 | 吞吐量提升 2-5x |
| 减少 fence 等待 | NPU/VPU 使用 in_fence 模式，驱动内等待 | 延迟降低 1-2ms |
| 批量推理 | GPU 拼接 4 路为 batch，NPU 一次处理 | NPU 利用率 4x |
| 预分配帧池 | 启动时分配所有帧，运行时零分配 | 消除帧分配抖动 |
| CPU 缓存同步最小化 | CPU 只在必要时 sync，其余走隐式同步 | 减少 cache flush 开销 |
| 双缓冲流水线 | 采集 N+2 帧时处理第 N 帧 | 帧率不受处理延迟影响 |

## 10. 典型端到端数据流

```mermaid
graph LR
    subgraph "Camera 采集"
        A["ISP DMA 写入<br/>→ dmabuf (NV12)"]
    end

    subgraph "GPU 预处理"
        B["resize 1080p→640x640<br/>NV12→RGB float<br/>GPU kernel 零拷贝"]
    end

    subgraph "NPU 推理"
        C["YOLOv8 检测<br/>输入: 1x3x640x640<br/>输出: 检测框 tensor"]
    end

    subgraph "VPU 编码 (并行)"
        D["H.264 编码<br/>输入: 原 1080p NV12<br/>输出: 编码码流"]
    end

    subgraph "输出"
        E["推理结果<br/>→ 应用层"]
        F["编码码流<br/>→ RTMP/文件"]
    end

    A -->|"dmabuf fd + out_fence"| B
    A -->|"dmabuf fd + out_fence"| D
    B -->|"dmabuf fd + out_fence"| C
    C -->|"result dmabuf"| E
    D -->|"bitstream"| F

    style A fill:#ffcdd2
    style B fill:#c8e6c9
    style C fill:#fff3e0
    style D fill:#e3f2fd
```

**每帧的数据流与 fence 传递：**

```
┌─────────┐   dmabuf_fd    ┌───────────┐   dmabuf_fd    ┌─────┐
│  Camera  │───+ out_fence──→│ Preprocess │───+ out_fence──→│ NPU │
│  (ISP)   │                │  (CPU/GPU) │                │     │
└─────────┘                └───────────┘                └─────┘
     │                                                       │
     │ dmabuf_fd (原始帧)                                     │ result
     │ + out_fence                                           │ dmabuf_fd
     ▼                                                       ▼
┌─────────┐                                            ┌──────────┐
│   VPU   │                                            │   App    │
│ Encoder │                                            │ (结果读取)│
└─────────┘                                            └──────────┘
     │
     │ encoded bitstream
     ▼
┌──────────┐
│  推流/存储  │
└──────────┘
```
