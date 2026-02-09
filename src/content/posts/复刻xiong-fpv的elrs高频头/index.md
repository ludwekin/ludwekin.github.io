---
title: 复刻xiong-fpv的ELRS高频头
published: 2026-02-07T04:06:39.189Z
---


这个项目属于oshwhub上面比较好的高频头项目了。

但是稍微有点复杂，文字排版和版本管理不是很易懂，所以我复刻的时候又结合了另一个作者删减版本的设计。


1. ESP32模块，推荐不带天线的ESP32-WROOM UE，或ESP32 U等接口一致的32模块都行。

2. ESP32-WROOM-32UE-N16和ESP32-S3-WROOM-1-N16区别？

    ESP32-WROOM-32UE-N16使用 ESP32（传统系列） 的 Xtensa® LX6 双核处理器。这是一种成熟且广泛使用的低功耗 MCU，适合通用 Wi-Fi/Bluetooth IoT 应用。

    ESP32-S3-WROOM-1-N16使用 ESP32-S3 系列 的 Xtensa® LX7 双核处理器，具备更高性能（CPU 主频可达 240 MHz）、向量指令支持，用于加速机器学习／信号处理等任务。

3. ESP32‑WROOM‑32UE‑N16 和 ESP32‑S3‑WROOM‑1‑N16的引脚兼容吗？

    引脚 数量和位置不同。

    ESP32-WROOM-32UE-N16 的引脚对应传统 ESP32 的 GPIO、UART、SPI、ADC、DAC 等标准外围功能。

    ESP32-S3-WROOM-1-N16 由于是 S3 芯片，其引脚除了标准 GPIO 外，还可能支持额外的功能（更多 ADC 通道、高速 USB、PSRAM 等），并且内部的引脚复用定义也不一样。

4. ESP32-WROOM-32D和ESP32-WROOM-32E区别？

    32E 是 32D 的新版替代，推荐新设计使用 32E。功能与引脚兼容性上两者基本 一致、可互换使用（支持相同 Wi-Fi/蓝牙 + GPIO）。

5. ESP32-WROVER-I和ESP32-WROOM-32D区别？

    WROVER 系列的显著特点是集成了 PSRAM（Pseudo-Static RAM），适合需要大内存的应用（例如图形界面、摄像头缓存、大量数据缓冲等）。这是 WROVER 系列与普通 WROOM 系列最明显的不同点。

6. ESP32-WROOM-32U和ESP32-WROOM-32UE区别？

    搜了，UE更新。

7. 0.96寸TFT彩色显示屏 ，我买的时候，好像商家又标注说是ips工艺的，冲突吗？

    TFT 是显示技术的大类，IPS 是 TFT 里的一种面板工艺。

    真IPS，侧看颜色基本不反。

8. 1.27mm母座/排母通用吗？我记得不是2.54mm/1.25mm/1mm之类的才是标准尺寸吗？

    1.27 mm 是正儿八经的标准，而且是英制阵营的缩小版 2.54。也是0.05"。常见用途是ARM JTAG、SWD、调试口。

    1.25 mm 是日系公制。

    1.0 mm 是公制。

    2.54 mm 是0.1"。

9. IO35接JOYSRTICK。    

10. 0.96寸tft（ips）彩屏，有8个引脚需要接入。

    cs,blk,dc,res,sda,scl,vcc,gnd。

    BL / BLK = BackLight（背光）控制引脚。