---
title: macOS开发ESP32 S3经验分享
tags:
    - ESP32
    - "macOS "
published: 2025-12-04T12:36:26.000Z
date: 2025-12-02T16:00:00.000Z
category: ESP32
---

简单来说，怎么做？

在VS CODE里面下载ESP IDF插件跟着指示一步步来就行。

第一次配好环境写代码会有个坑：

写代码的时候，VSCode 的 includePath 没有加载 ESP-IDF 的头文件路径，所以写的比如，#include "driver/rmt_tx.h"、#include "driver/gpio.h"、#include "esp_log.h"等等代码会显示报错。

解决办法：

在 VSCode 里执行 ESP-IDF: Add vscode configuration folder，输入ESP-IDF: Add vscode configuration folder 即可。

OK，我们继续，下面分享一些开发经验。

Espressif IoT Development Framework Configuration，这个是我们下ESP IDF的时候它自带的，一般看到这个界面，就是 ESP32 项目配置的交互式菜单。它是基于ncurses的图形化终端菜单，不是 GUI 窗口，但用键盘可以操作。简而言之，这是我们在配置 ESP32 项目编译和烧录参数的地方，操作后会生成 sdkconfig 文件，控制项目的各种行为。

看不懂不要紧，等实际上手遇到这个界面你就知道我说的是什么意思了。


通常 ESP32 有两种方式烧录程序，UART和JTAG Flash。

执行idf.py flash 的默认方式，就是用 USB 串口烧录。

一般的ESP32 S3开发板，板载俩 USB TYPE-C 口。

一个是接到了FTDI/CH340C，一个接到了ESP32芯片。

接FTDI/CH340C的插口，如果你要用 idf.py flash，默认就是连这个口。

用这个口刷入程序，就是传统 UART 下载方式。

直连ESP芯片的插口，是ESP32-S3 原生 USB。

macOS 插上这两个TYPE-C口，打开终端，输入 ls /dev/tty.*。

会发现不一样的结果，那我的输出结果示例，插上普通UART串口会显示/dev/tty.usbserial-A5069RR4。

插上芯片直连插口会显示usbmodem101。

对了。ESP32-S3 内置 USB，它支持三种多模式。
1是USB CDC（串口）。
2是USB-JTAG（调试）。
3是USB-DFU（刷机）。

如果要用 JTAG，必须插 ESP32-S3 的 原生 USB 那个口。

插 ESP32-S3 的 原生 USB 那个口，就可以打开OpenOCD。

OpenOCD = Open On-Chip Debugger。

作用就是把你的电脑（VSCode / GDB / Python 脚本）和芯片内部的调试接口（JTAG / SWD / USB-JTAG）连接起来。

OpenOCD 会在 端口 3333 启动 GDB server。用 VSCode / PlatformIO / Eclipse 调试时，GDB client 会连接这个端口，进行高级调试。

用JTAG也可以烧录程序，在我的例子里面，需要在 VS CODE 的这个项目的settings.json里面保证以下代码："idf.monitorPort": "/dev/tty.usbmodem101"和"idf.flashType": "JTAG"。

















