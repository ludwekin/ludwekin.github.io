---
title: ELRS空中链路
published: 2025-12-11T07:00:27.442Z
category: 遥控链路
tags:
    - elrs
---


1. ELRS 不是把 CRSF 直接“空中广播”。最终在空中传输的是一个 RF PHY Frame（物理层帧）。

    这个帧的内容是 ELRS 自己定义 的。

    再次强调，CRSF 只是 payload 的结构格式。

2. ELRS 物理层用 2 种调制。LoRa加FLRC。

3. 一个完整 ELRS 空中包的结构？

    一是，[Preamble] 可变长度。前导码长度 = 32 bit（可调）。

    二是，[Sync Word] 32 bit。ELRS 使用 32-bit 同步字，每个模式不同。稍后再说。RX 用 sync word 来确认“这是属于我的链路”。用于跳频同步（FHSS）。

    三是，[Header] 8 bit。

    四是，[Payload]  N byte（包含 CRSF 数据 + 控制字段）。

    五是，[FEC Parity] 由编码器生成。

    六是，[CRC] 16 bit。

4. 500/1000Hz 模式下，ELRS采用FLRC。

5. 补充一个重要的概念。airtime（空中时间）= 一个无线数据包在空中占用频率资源的时间长度。airtime = (总发送比特数) / (物理层比特率)。单位是秒（ms、µs）。

6. ELRS遥控器如果用500Hz 模式，那就是 2ms 周期。

7. 说不完了。FLRC 是一种 快速 FSK 变种（Fast Long Range Comms）。见贴“FLRC与无线调制入门.md”。

8. 












