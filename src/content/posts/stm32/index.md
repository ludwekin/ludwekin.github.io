---
title: STM32控制逻辑
published: 2025-12-13T04:35:33.037Z
---


STM32的SRAM，地址从0x2000_0000 开始。

STM32的Flash，地址一般从 0x0800_0000 开始。

SRAM 里典型放栈 (stack)，堆 (heap)，全局变量和DMA buffer。

对了，扯一下，STM32F103 没有 CCM，所以这也是STM32F103不太适合作为飞控主芯片的原因。

SRAM 要给 DMA、外设共享。而CCM只连CPU。

再提一嘴，飞控的Flash放算法、任务代码。SRAM用来传感器 DMA、通信。CCM负责姿态、PID、调度。
