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





