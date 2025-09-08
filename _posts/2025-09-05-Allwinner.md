---

title : "Allwinner H618"

---


全志芯片，刚学习嵌入式的时候就听说过了，刚好部门需要开边缘计算相关的业务，我借到了一块 板载H68开发版。

搜了资料，全志科技是一家中国的SoC（System on Chip）设计公司。全志购买了 ARM 授权，在自家芯片里集成 ARM CPU 核心。 

### ARM 架构和安卓手机市场

ARM 在安卓手机市场占额非常之高，截止20250906，现在的安卓手机很多机型都搭载了 ARM 架构芯片。我在下面贴一个近期发布的 安卓手机和芯片 对照表。

```markdown
# Android 手机近期使用的 ARM SoC 列表

##  具体机型与芯片

- **Samsung Galaxy S25 FE**  
  使用 Exynos 2400 芯片（ARM 架构）  [oai_citation:0‡Tom's Guide](https://www.tomsguide.com/phones/samsung-phones/samsung-galaxy-s25-fe-specs-price-galaxy-ai-features?utm_source=chatgpt.com) [oai_citation:1‡Android Central](https://www.androidcentral.com/phones/samsung-galaxy/samsung-galaxy-s25-fe-galaxy-tab-s11-ultra-hands-on?utm_source=chatgpt.com)

- **Google Pixel 10 / Pixel 10 Pro**  
  采用 Google 自研的 Tensor G5 SoC  
  - 架构：1× Cortex-X4 + 5× Cortex-A725 + 2× Cortex-A520  [oai_citation:2‡Wikipedia](https://en.wikipedia.org/wiki/Pixel_10?utm_source=chatgpt.com)

- **Sony Xperia 1 VII**  
  配备 Qualcomm Snapdragon 8 Elite  
  - 架构详解：2× “Kryo Prime”（基于 Cortex-X4） + 3× Kryo Gold（Cortex-A720） + 2× Kryo Gold（Cortex-A720）+ 2× Kryo Silver（Cortex-A520）  [oai_citation:3‡Wikipedia](https://en.wikipedia.org/wiki/Sony_Xperia_1_VII?utm_source=chatgpt.com)

- **ASUS Zenfone 12 Ultra**  
  搭载 Snapdragon 8 Elite SoC  [oai_citation:4‡Wikipedia](https://en.wikipedia.org/wiki/Zenfone_12_Ultra?utm_source=chatgpt.com)

- **Fairphone 6**  
  使用 Qualcomm SM7635 芯片（中端 ARM SoC），内含 Adreno 810 GPU  [oai_citation:5‡Wikipedia](https://en.wikipedia.org/wiki/Fairphone_6?utm_source=chatgpt.com)

- **Vivo Y400 5G**  
  搭载 Snapdragon 4 Gen 2（架构：2× Cortex-A78 + 6× Cortex-A55）  [oai_citation:6‡Wikipedia](https://en.wikipedia.org/wiki/Vivo_Y400?utm_source=chatgpt.com)

- **Realme GT 7 系列（即将发布）**  
  预计将采用 MediaTek Dimensity 9300+ 芯片  [oai_citation:7‡Indiatimes](https://indiatimes.com/technology/realme-gt-7-series-teased-in-india-by-the-brand-coming-soon-with-android-15-12gb-of-ram-mediatek-dimensity-9300-soc-and-more-658318.html?utm_source=chatgpt.com)

##  总结表格

| 机型                      | 发布时间    | SoC（ARM 架构）            |
|---------------------------|-------------|----------------------------|
| Samsung Galaxy S25 FE     | 2025       | Exynos 2400               |
| Google Pixel 10 / 10 Pro  | 2025-08-28 | Tensor G5 (Cortex 架构)  |
| Sony Xperia 1 VII         | 2025-06-04 | Snapdragon 8 Elite        |
| ASUS Zenfone 12 Ultra     | 2025-02-06 | Snapdragon 8 Elite        |
| Fairphone 6               | 2025-06-25 | Qualcomm SM7635 (Adreno 810) |
| Vivo Y400 5G              | 2025-08-04 | Snapdragon 4 Gen 2        |
| Realme GT 7 系列          | 待发布      | Dimensity 9300+           |

```
从上面这些这些资料也可以发现，手机搭载的并不是单单一个 ARM 芯片，而是组合起来的多个芯片。

上面的 Google Pixel 10，架构是 1× Cortex-X4 + 5× Cortex-A725 + 2× Cortex-A520。这是个值得分析的组合方式，Cortex-X4作为超大核，3–5× Cortex-A725是作为大核，2–4× Cortex-A520则是作为节能小核。这种组合叫做 **异构多核 (big.LITTLE / DynamIQ)**。那怎么实现多核心调度呢？这也是个待研究的问题。

下面也具体陈列一下我感兴趣的 Cortex-A53 和 Cortex-A 的资料。

```markdown

# Cortex-A53 和 Cortex-A 的介绍，资料来源为GPT

1. **Cortex-A 系列**  
   - **定义**：ARM 架构中面向应用处理器的系列。`A` 代表 **Application**。  
   - **特点**：强调高性能，运行操作系统（如 Linux、Android）。  
   - **包含成员**：Cortex-A5、A7、A8、A9、A15、A35、A53、A55、A57、A72、A73、A75、A76、A77、A78、X 系列等。  

2. **Cortex-A53**  
   - **具体型号**：Cortex-A 系列里的一款 64 位处理器。  
   - **架构**：ARMv8-A（支持 64 位和 32 位指令集）。  
   - **定位**：低功耗 + 中等性能，常见于智能手机、机顶盒、嵌入式板卡。  
   - **特点**：  
     - 支持 64 位计算。  
     - 支持对称多核（big.LITTLE 架构里常与 Cortex-A57 或 A72 搭配）。  
     - 在低功耗场景（如 IoT、入门级移动设备）广泛使用。

```

---

## Linux CFS 调度器 (Completely Fair Scheduler)

多核心调度问题。

多核调度不是硬件自动完成，而是由 **操作系统内核（如 Linux/Android 内核）** 负责。内核根据优先级、负载均衡，将任务分配到不同核心。实际中，Android 内核默认使用 Linux CFS 调度器 (Completely Fair Scheduler)。

```markdown
# Linux CFS（Completely Fair Scheduler）怎么编程实现

> 先答：CFS=按“虚拟运行时间 vtime”在红黑树上公平选择下一个任务。实现落点在 `kernel/sched/fair.c`，关键是维护 `vruntime`、红黑树、时钟更新与负载均衡。

## 最小工作原理（实现要点）
1. **核心数据**  
   - `struct sched_entity`：进程在 CFS 的表示，含 `vruntime`、权重 `load.weight`。  
   - `struct cfs_rq`：就绪队列，每个 CPU 一个，含红黑树 `tasks_timeline`（按 `vruntime` 排序）。  
2. **关键公式**  
   - `delta_vruntime = delta_exec * NICE_0_LOAD / se->load.weight`  
   - 直观：权重高（更“重”的进程）长同样时间，`vruntime` 增长更慢 → 更常被选中。  
3. **调度循环**  
   - 计时器中断触发 → `update_curr()` 增量更新当前任务 `vruntime`。  
   - 入队：`enqueue_task_fair()` 把 `sched_entity` 插入红黑树（按 `vruntime`）。  
   - 选取：`pick_next_task_fair()` 取红黑树最左节点（最小 `vruntime`）。  
   - 出队：`dequeue_task_fair()` 从红黑树删除。  

## 2. 代码入口（Linux 内核）
- 主要文件：`kernel/sched/core.c`、`kernel/sched/fair.c`、`kernel/sched/sched.h`  
- 重要结构：`task_struct` → `sched_entity se` → `cfs_rq`  
- 重要函数（都在 `fair.c`）：  
  - `enqueue_task_fair()` / `dequeue_task_fair()`  
  - `pick_next_task_fair()`  
  - `update_curr()`、`place_entity()`、`calc_delta_fair()`  
  - `task_tick_fair()`（每个时钟节拍更新）  
```

上面的资料来源GPT，资料里面出现了 红黑树和perf，这是我感兴趣的地方。

我把上面这段晦涩的资料稍微简化一下。

1. `vruntime`就是虚拟运行时间。红黑树算法，能够快速找到 vruntime 最小的任务。 
2. CPU 每个 tick（比如 1ms）会触发中断，内核就会更新当前运行任务的`vruntime`。
3. 对于多核 CPU，每个 CPU 有一棵红黑树。 系统内核会定期检查 → 如果某个 CPU 核忙，另一个空，就迁移一些任务。 

但是这里，安卓系统，为什么会牵扯到Linux内核，读者有的可能会疑惑。下面顺便解释一下。

1. Android 操作系统的底层内核就是 **Linux 内核**。Google 是 Android 操作系统的开发者和维护者。  
2. Google 从 Linux 内核裁剪和扩展，加入了 Binder、wakelock、low memory killer 等机制。  
3. Android 厂商（高通、联发科、三星等）在 CFS 基础上会做优化，比如性能模式、省电模式。

对了，从上面这个资料，是不是看到熟悉的 Linux 内核裁剪和扩展？这也算是嵌入式系统的一个职业分支。

## 回到全志开发板

回到主线，全志这块开发板的应用场景。

```markdown

# 全志 (Allwinner) 芯片的应用场景

1. **消费电子**  
   - 平板电脑（早期国产平板常见 Allwinner A 系列）  
   - 低成本智能手机（已逐渐减少）  
   - 智能电视盒子 (OTT box, TV box)  

2. **智能硬件 / IoT**  
   - 智能音箱、智能家居控制器  
   - 电子书阅读器  
   - 车载娱乐系统、导航仪  
   - 学习机、教育平板  

3. **嵌入式与工业控制**  
   - 人机交互界面 (HMI) 设备  
   - 工业控制板卡  
   - 医疗电子（便携显示终端）  
   - 广告机、信息展示屏  

4. **优势场景**  
   - 成本敏感产品（低价 SoC）  
   - 对 GPU/CPU 性能要求不极高，但需要视频编解码功能（H.264/H.265）  
   - 大规模出货的消费级设备  

# 学科领域  
- 嵌入式系统 (Embedded Systems)  
- 消费电子 (Consumer Electronics)  
- 物联网 (IoT)  

# 相关职业  
- 嵌入式软件工程师  
- 硬件研发工程师  
- 消费电子产品工程师  
```

1. 上面的GPT资料，结合我的互联网搜索，有几个方向我比较关心，视频网站搜全志芯片，出现的结果有很多都是 安卓电视盒子。
2. 全志好像自己设计了 vpu 封装到了 SoC 里面。因此 全志可以广泛应用于需要视频编解码（H.264/H.265/VP9）的场景。


## 未完成的部分，以待下次编写




