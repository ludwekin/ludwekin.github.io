---
title: ELRS遥控器集成发射机开源自制
published: 2025-12-13T15:28:16.683Z
---

TXOTAConnector.h。

CRSFParser.h。

CRSFRouter.h。CRSF路由、双天线/双射频管理。

TXModuleEndpoint.h。

TXOTAConnector.h。

rxtx_common.h。

我现在想做一个集成发射机的遥控器。

核心任务是写一个最小 TX 固件。

手柄输入可以统一封装成 HandsetAdapter 类，把所有输入映射到 channels[16]。

摇杆/旋钮模拟信号通过 ADC 采样。STM32 的 ADC 可以 DMA 直接写入内存，不占 CPU 时间。ELRS 发射需要固定周期发送帧（比如 500 Hz）。

可以用 定时器中断触发发送函数。发送数据直接写 SPI 到 RF 模块。

CPU 在中断里拿到缓存好的通道数据发送。

难点？读取 DMA buffer 的摇杆值，这个我想想。

F103 128 KB 足够哈哈哈。