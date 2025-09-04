---
title : "嵌入式设备,部署视觉模型来实现目标追踪" 

published : false

---

## 有哪些轻量的视觉模型？

 1. MOSSE (Minimum Output Sum of Squared Error)
    - **提出时间**：2009 年  
    - **核心思想**：利用相关滤波（Correlation Filter）快速学习目标的外观模板，并在后续帧中通过卷积计算找到目标位置。  
    - **特点**：
        - 速度非常快（当时能达到几百 FPS）。  
        - 在光照变化、尺度变化较小的情况下效果好。  
        - 但对目标的外观变化和遮挡比较敏感。  
    - **典型应用**：实时跟踪小目标、低算力设备。
2. KCF (Kernelized Correlation Filters)
    - **提出时间**：2015 年左右  
    - **核心思想**：在 MOSSE 的基础上，引入核技巧（Kernel Trick），提升了特征表示能力（不仅仅用灰度，还可用 HOG 特征）。  
    - **特点**：
        - 精度比 MOSSE 更高，尤其在复杂背景下更稳健。  
        - 可以处理一定的外观变化和部分遮挡。  
        - 计算量比 MOSSE 大，但仍能保持实时性。  
    - **典型应用**：无人机跟踪、人/车目标跟踪、机器人视觉。

---


## SIMD 简介

1. SIMD = Single Instruction, Multiple Data.
2. 基本思想：一条指令可以同时处理一组数据（向量运算）。



---


## ESP32-S3 支持 SIMD 吗？

1. ESP32-S3 芯片里有一个 向量扩展 (Vector Extensions)，它支持 单指令多数据 (SIMD)，并且专门为 AI / DSP 加速 做了优化。  
2. 怎么调用？
    - 可以通过 ESP-DSP 库 或 ESP-NN 库 来调用这些 SIMD 指令。
        - [ESP-DSP](https://github.com/espressif/esp-dsp) 提供 FFT、卷积、矩阵乘法的 API。  
        - [ESP-NN](https://github.com/espressif/esp-nn) 提供 int8 神经网络推理的加速。










---
