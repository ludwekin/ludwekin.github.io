---
title: ELRS接收机，开源自制
published: 2025-12-13T14:40:55.979Z
---

想要做好这件事。

参考了很多版本的PCB LAYOUT设计。

原理图似乎不复杂。

ESP-M1（嘉立创编号C19949056）（四博智联出品）和ESP8286区别？

ESP-M1它本质是一个带 MCU + TCP/IP + Wi-Fi 射频 + Flash 的 SoC 模块。

所以ESP-M1 是 模块（Module）。

等等哦，对于ESP8266EX我一直有个问题，所谓的“内置完整 TCP/IP 协议栈”到底是什么底层含义？？？？？？

ESP8266 其实是一个统称，或者说就是一个平台。

很多商家/开发商给这个ESP8266EX这个芯片二次包装成了很多出名的模块。

比如ESP-01。ESP-12F。ESP-M1。等等。

ESP8266EX本身没有 Flash 。等等？？

ESP8266EX本身没有 Flash 。STM32F103芯片就有 flash 吗？

这还真是个好问题，首先肯定是有的。而且 STM32 自带 flash 是核心卖点之一。

ESP8266EX 为什么没有 Flash？

ESP8266EX 的定位是通信 SoC（Wi-Fi SoC）。

它设计时假设固件体积会较大。于是 Espressif 做了这个决定-不在芯片内集成 Flash。

CRSF 协议连接遥控手柄与 ELRS 发射模块。





