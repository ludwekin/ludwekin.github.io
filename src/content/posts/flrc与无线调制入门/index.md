---
title: FLRC与无线调制入门
published: 2025-12-11T07:26:41.627Z
---


FLRC 核心思想是用连续变化的正弦波频率来表示二进制 0/1 。

拿ELRS发射器/遥控器来说，MCU 写 bit 数据到 SX1281 的 FIFO ，SX1281 内部调制器把 bit 转成频率偏移，然后晶振 PLL 输出 RF 波形。

ELRS 使用 SPI 总线 和 SX1281 芯片通信。

我画了一个遥控器的原理图和PCB。感兴趣可以打开一鉴。

见“UAV遥控器设计.md”。

LoRa 是如何调制的？LoRa 是 Chirp Spread Spectrum (CSS)。


