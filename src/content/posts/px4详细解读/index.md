---
title: PX4
published: 2025-12-13T15:02:26.394Z
---


说了详细，难道这真的说得完吗？哈哈当然不可能。

PX4 的链接脚本？

PX4的的“真实内存分层图”。

![alt text](<Screenshot 2025-12-13 at 11.06.15 PM.png>)

PX4 极少 malloc。

这个我确实听说过，航空电子里面，特别是战斗机，代码不malloc。

MPC 接收 EKF2 的状态。

再接收你给 PX4 的指令：

Offboard / Mission / Position mode：

目标位置

目标速度

目标加速度（可选）

然后计算出：

期望加速度 ax ay az

等价的：

期望推力

期望姿态（roll / pitch）

然后交给 姿态控制器。

为什么 PX4 用 MPC 而不是 PID？

PX4 用的是“简化版 MPC”。

EKF2：眼睛 + 大脑，判断“我现在在哪”

MPC：高级驾驶员，规划“怎么踩油门和方向盘”

姿态控制器：执行层，控制电机


3000000 baud 大约对应 0.3 MB/s 的有效数据速率（粗略地说，常说“300 KB/s”）。

8N1 是串口（UART）通信中最常见的配置格式，全称是 8 data bits, No parity, 1 stop bit（8 数据位、无奇偶校验位、1 停止位）。

每个字节结束后发送 1 个停止位，表示本帧结束。

8N1 是最标准、最常用的串口配置。

要理解一下px4的几个"手动"模式。

Manual mode和Acro mode和Stabilized区别？？


px4项目里面，有很多pr开头的分支，是啥意思?

pr-* 分支 = 用来测试某个 GitHub Pull Request 的“影子分支”。


https://github.com/PX4/PX4-Autopilot/tree/main/msg，这个库理解一下？

构建 PX4 固件时，CMake 系统会自动处理这些 .msg 文件，生成对应的 C++ 头文件和源码（位于 build 目录下的 uORB/topics），供模块发布（publish）和订阅（subscribe）使用。

这些消息还会被同步到 px4_msgs ROS/ROS2 包。

qgc里面的mavlink console,可以输入help。

可用的 shell 命令?
  
  
  cd       echo     free     mkfatfs  rm       test     usleep   
  [        cp       exec     help     mount    rmdir    time     
  ?        date     exit     kill     mv       set      true     
  break    df       export   ls       ps       sleep    umount   
  cat      dmesg    false    mkdir    pwd      source   unset


唉，commander模块。


MPC_XY_VEL_MAX, 这里咋会有个MPC呢？

对了，很奇怪，好像在室内，我连香橙派的wifi也更稳定，连px4飞控（915MHz）也更稳定。


分享一下几个调试/研发真机时候的危险时刻。

1. 遥控器没连上，很奇怪为什么，我直接qgc点起飞。。。然后特别危险天上一直晃。我赶紧又在qgc点land。
2. 下球形无人机，上面没有保护网。land后不稳定，疯狂打转。出事后我每次飞行前检查遥控器的紧急停转功能是否正常。
3. 手持模拟飞行，电机没有限速，高速振动。很危险。不能放手，然后遥控器还没接入。。。最后别人从我手上的无人机拔掉了电池接头。

/fs/microsd/etc/extras.txt，这是放xrce_dds_client start -t serial -d /dev/ttyS5 -b 115200的地方。通过qgc可以改？



不确定，但是可以访问到。最好还是在sd卡里面改。

MPC_Z_VEL 设置成1m/s，对于我们的十寸四旋翼来说是起飞不起来的。当然0.5m/s也不行。