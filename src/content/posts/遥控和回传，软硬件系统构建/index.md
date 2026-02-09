---
title: 遥控和回传，软硬件系统构建
published: 2026-02-07T03:44:31.396Z
---


我看到了一个有意思的方案，商品遥控器（比如RadioMaster-tx16s）的JR仓接自定义的发射机。

这个发射机其实就是一个esp32-s3模块。然后接收机也是esp32-s3模块。

协议就用的espnow。

其实遥控器也可以换成任意的遥控器。

1. 射频模块的NRF_CE和NRF_IRQ引脚干嘛的？

    CE = Chip Enable。

    IRQ = Interrupt Request。

2. 关于遥控器的摇杆，一开始我一直以为需要给左摇杆配一个上下不回正，左右回正的型号，结果买不到。

    今天我看了大疆实体店的遥控器，他们的就是左右摇杆都回正。可以参考一下。

## 开源硬件参考

1. [text](https://oshwhub.com/eda_sbnvoqqej/kai-yuan-si-zhou-yao-kong-qi)，f103遥控器。

用的16*16大号航模摇杆。


