---
title: CocoDrama飞控复刻记载
published: 2026-01-30T08:48:32.136Z
---


分两个版本，V1和V2。

个人觉得pcb的设计比urmouky的要好一点。

up主的装机配置是5寸机，用的mark4。

有意思的是电池组用的是6s，21700。

电机配的是丰云2306，6s版本。桨叶由此适配破风5146。

XC6206P332MR 是一颗 3.3V 低压差线性稳压器（LDO）。

SS8050在飞控上一般有三个可选的作用。

    1. 电流放大。比如给beeper供电。
    
    2. 电平反相。

    3. MCU 控制 VTX 上电/MCU 控制 摄像头 5V。


SS8050 和 SS8550 是一对常用的互补小信号/中功率晶体管。

SS34 是肖特基二极管（3A / 40V）。用在飞控上基本就是防反接。

1N5822 为什么也叫 1N5822？

1N5822 = JEDEC 标准编号。只要电气参数符合，这颗管子就“有资格”叫 1N5822。

JEDEC 全名 Joint Electron Device Engineering Council（联合电子器件工程委员会），主要贡献就是统一“型号命名规则”。

SMAJ33A的VRWM（长期不导通）为33V。用来保护飞控，防止电机刹车反灌/ESC 开关尖峰/插电火花。

J33A适合飞控配6s电池组的使用场景。

常见射频接口有哪些？

SMA，TNC，N Type，IPEX / U.FL，MMCX，MCX，SMB / SMC。

常见封装类型（表面贴装 SMD/SMT）？

SMA（常用于二极管），SMAJ / SMB / SMC（常用于TVS、整流二极管），SOD-xxx，DFN / QFN / LQFP（用于芯片），DIP / PDIP，TO-220 / TO-247 / TO-263（功率型封装，有散热片），0402 / 0603 / 0805 / 1206（阻容）。