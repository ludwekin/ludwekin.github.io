---

title : "正在复刻Sahko的手环设计，经验分享"

---




**Primary Notice:** 此文未完，项目仍在初期。 [Praesent libero](#). Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.
{: .notice--primary}


## Reproduce an open-source electronics watch

1. 看到了一个 [watch design](https://www.youtube.com/watch?v=dBEupkQBFis) ，还算很帅的。Illustrated below. 有空想试着复刻一下。

![image-center](/assets/images/watch.png){: .align-center}
2. 项目主控采用了 stm32u083kcu6 芯片。供电采用了 软包锂电池（聚合物电池）。







6. Type-C 协议/接口。绕不过去的Type-C 协议。

    - 因为这个 Type-C 接口/协议很通用，所以值得研究一下。
    - 设计pcb的时候，这个Type-C母口接口的选型也挺多样的，便宜的只有2pin，好焊接，贵的有24pin，难焊接。
    - Type-C 接口的 CC（Configuration Channel）针脚 是非常关键的信号通道。
        
        - 线缆插入方向检测：接口有两个 CC（CC1 和 CC2），但插入时只有一个真正接通。控制芯片通过检测哪个 CC 有电平 → 确定插头方向。
        - 角色识别（Host / Device / DRP）：通过在 CC 上加不同的 电阻（Rp, Rd, Ra） 来区分设备是用电端还是供电端。
        - USB Power Delivery (PD) 通信：如果设备支持 PD，CC 线上会传输 BMC 编码（Biphase Mark Coding） 的数字信号。
        - 电流能力协商（USB PD 之前的基础电流）：在没有 USB PD 协议时，CC 还能告诉对方供电能力。
