---
title: 对ELRS开源代码库的学习和理解
published: 2025-12-11T15:35:00.403Z
category: 遥控链路
tags:
    - elrs
---


1. ESP8266 对 SX1281 的每一次操作都是 SPI 指令。

    一，设置调制方式（FLRC），spiWrite({0x8A, 0x01})。

    二，设置频率，spiWrite({0x86, freq_bytes[0], freq_bytes[1], freq_bytes[2]})。

    三，将 CRSF payload 写到 SX1281 FIFO，spiWrite({0x0E, 0x00, ...payload})。

    四，触发发射，spiWrite({0x83, 0x00, 0x00})。

2. 探索SX1281的功能和作用。

    ELRS 将不同 RF 芯片（如 SX127x、SX1280、LR1121）封装成统一驱动接口；利用 HAL 封装不同频段、电源管理与调制特性。这让 ELRS 可以跨多种无线芯片实现统一软件栈。不难理解。

3. ELRS的遥测处理。

    Telemetry System将不同来源数据（MAVLink、GPS、HoTT、内置传感器）统一转换为 CRSF Frame。

    ELRS supports both 900MHz and 2.4GHz frequency bands with packet rates up to 1000Hz on 2.4GHz and 200Hz on 900MHz.

4. 为什么 ELRS 要自己做 Parameter System？

5. 2025年elrs彻底支持mavlink，这代表了什么？

    接收机直接输出原生MAVLink到飞控。ELRS 3.5+版本实现了单链路集成RC控制 + 双向完整MAVLink遥测。

6. MAVSDK 的仓库中有一个 Git 子模块（submodule）叫做 proto。


