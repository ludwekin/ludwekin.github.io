---
title: FPGA图像捕获
published: 2025-12-11T12:44:55.544Z
---


这个文章可就太有意思了。

先来一个简单的问题吧。

为什么台式机不可以插上笔记本的HDMI之后就可以直接显示？

笔记本的HDMI接口是输出口。无法输入HDMI。

要回答这个问题，先熟悉一下TMDS。HDMI 传输的是 TMDS（Transition-Minimized Differential Signaling）高速差分编码信号。

GPU 输出的是 TMDS 编码后的数据。显示器必须对HDMI接口进行 TMDS 解码，才能显示图像。

再讲讲HDMI的原理吧。

还有HDMI和DP的区别？

这个很有意思。

HDMI 2.0 宣称 18 Gbps（Gigabit per second）。

意思是 3 条 TMDS 数据差分对（Data0、Data1、Data2）+ 1 条 TMDS 时钟线，总共能承载的物理层带宽上限。

HDMI 2.0刚好够传输4K 60Hz RGB 的数据。

图像由像素组成，每个像素是3个通道，RGB。各8位。

这样的话每秒60Hz，传输数据量太大了，可以缩小吗？

可以，YCbCr技术/概念出现了。

YCbCr 不是某一个人想到的，而是一整套电视广播工程演化出来的方案，由不同标准化组织逐步定义，最终由 ITU（国际电信联盟）正式规范。

数字视频时代 YUV 被定义成 YCbCr 。

YUV是模拟彩色电视信号。

RGB 和 YCbCr 是两种“颜色表示方式”。它们是可以互相转换的（数学变换）。

HDMI 既能传 RGB 4:4:4，YCbCr 4:4:4，YCbCr 4:2:2，也能传YCbCr 4:2:0 。

怎么做一个fpga的采集卡？要1080p 30hz。

这个的制作过程可以学到很多，特别是对PLL的了解会有帮助。

其实除了采集卡的方案，还有另外一种，就是把笔记本当作一个屏幕用。

后续再说。

PLL可以帮助位对齐。也就是恢复采样时钟。

FPGA 内部都有 PLL / MMCM 模块，可以把输入 TMDS clock 锁定。


	





