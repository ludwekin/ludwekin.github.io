---
title: ELRS项目
published: 2025-12-11T15:35:00.403Z
---


难。

ESP8266 对 SX1281 的每一次操作都是 SPI 指令。

一，设置调制方式（FLRC），spiWrite({0x8A, 0x01})。

二，设置频率，spiWrite({0x86, freq_bytes[0], freq_bytes[1], freq_bytes[2]})。

三，将 CRSF payload 写到 SX1281 FIFO，spiWrite({0x0E, 0x00, ...payload})。

四，触发发射，spiWrite({0x83, 0x00, 0x00})。

探索SX1281的工作。
