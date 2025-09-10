---

title: "Design a Drone PCB，with PX4 Firmware"

published : false

---

### fly control, the central chip

F743（商用飞控最经典芯片型号）基本一直缺货。F7其他型号的也是经常缺货。


### gyro

飞控主板给gyro等等芯片供电基本都需要3.3V的电压转换器。我一般先选择 NCP 1117 3.3V regulator。

### IMU

MPU6000,常出现的imu型号，但是停产了。


### pcb layout

电阻电容最好选0402尺寸的。

High-Power and signal cables should be separated as much as is practical.


## PX4
 
 ### PX4 上位机
 PX4 上位机就是地面控制软件（QGroundControl、MAVProxy）。

 ### PX4的发布和订阅机制

 PX4 里面有一个 uORB（micro Object Request Broker），类似 事件系统 / 发布-订阅框架。

    - 各模块（传感器、控制器、任务管理）不直接调用对方，而是通过消息发布和订阅交互。

    - 这就是 事件驱动框架 + 环形缓冲区 在飞控中的使用案例。
    - 传感器模块 → 发布 IMU_DATA；估计器 (EKF) → 订阅 IMU_DATA，然后发布STATE_ESTIMATE；控制器 → 订阅 STATE_ESTIMATE 和 RC_INPUT，输出控制量；电机驱动 → 订阅 CONTROL_OUTPUT，驱动 PWM。


 ### MAVLink 

 MAVLink 就像无人机和上位机之间的“语言”，负责信息传递、指令下发和状态同步。掌握 MAVLink 可以让你独立开发上位机软件或无人机控制程序。








4. PX4 上位机就是地面控制软件（QGroundControl、MAVProxy）。
5. 
6. 估计器 (EKF)是什么？

    EKF（扩展卡尔曼滤波，Extended Kalman Filter）是飞控系统里一个核心大脑的角色，主要用于状态估计。或者说，是一种用于多传感器融合的状态估计算法，帮助飞控系统实时得到可靠的位置、速度、姿态。它是无人机飞行稳定和导航的核心算法之一。
7. eFuse 是有实体的。

    - eFuse 的实体通常就是一个很短的 电阻，连接在芯片内部的逻辑电路里面。所以一旦写入就不可逆。
8. SBUS（Serial BUS） 是 Futaba 公司最早提出的一种 单线串行通信协议，主要用于 遥控器和接收机之间的数据传输。这个协议把多个通道（一般 16 个以上）的 PWM 信号 打包成串行数据帧，通过 单根信号线 传输给飞控或伺服。
9. 怎么在stm32fxxxxxxx的数据手册上找到，电源供电方面的说明？比如我看视频都要并联一个电容来着。

    我看了数据手册，但是没找到相关的说明，后面发现了这个要在硬件设计指南上面找。
10. usb接口的d+和d-连到stm32，要加电阻的。作用是阻抗匹配，减少反射和 EMI。
11. 4 pin JST-GH这是什么？

    - JST：一家日本连接器厂商（Japan Solderless Terminal）。
    - GH 系列：JST 出品的一类小型连接器系列。
    - 4 Pin JST-GH：指 GH 系列里的 4 芯（4 针脚）连接器。



---

## 项目经验

1. The following basic concepts should be kept in mind when designing drone cabling:

    - High-Power and signal cables should be separated as much as is practical.
    - Cable lengths should be the minimum needed to enable easy handling of wired components. The wire tension should be adequate to survive possible airframe deformations even in a crash landing (wires must not be the first thing to break).
    - Cable loops to reduce excess length should be avoided - use shorter lengths!
    - For digital signals you can decrease the baudrate to reduce radiated energy and increase the robustness of data transfer. This means that you may be able to use longer cables when high data rates are not needed.
2. GPS receivers and magnetometers are generally very sensitive to EMI. Therefore these should be mounted far away from RF sources (high-power cabling, ESCs, radio modems and its antenna). This may be insufficient if the cabling is badly designed.

---

### 飞控芯片为什么需要 Hardware Floating Point Acceleration?
    
    - 飞控（Flight Controller）要实时处理 姿态解算、传感器融合、控制律计算。
    - 常用的卡尔曼滤波（Kalman Filter）、四元数更新、PID/模型预测控制，都涉及大量的矩阵运算、三角函数和浮点乘加。


## 飞控程序

1. 无人机飞控程序组成：
    - 传感器数据处理
        - 加速度计
        - gyro
        - 气压计
        - GPS 
        - 光流传感器
        - 超声波传感器
        - 激光雷达
        - 摄像头
    - 数据处理
        - 去零漂移
        - 去温漂移
        - 卡尔曼滤波
    - 状态估计
    - 姿态计算
    - 地面站通信
    - 多重 PID 算法
    - 电机控制
    - 电源检/监测
    - 安全保护
    - 日志系统
2.  所以无人机飞控程序绝对不是 在mcu内用while编程 实现的。这样每个模块都耦合在一起，怎么维护？所以有没有调度办法，而且可以实现解耦呢？有的，那就是 RTOS。
3. 全世界使用量预估最大的 RTOS，应该就是 FreeRTOS。


## 飞控核心概念



### 卡尔曼滤波

卡尔曼滤波总结：

    - 基本定义：
        卡尔曼滤波是一种最优估计算法，用于在存在噪声的情况下估计系统的状态。它基于线性系统模型，结合传感器测量值和预测值，在最小化误差的意义下得到最优估计。由 Rudolf E. Kalman 在1960年提出，广泛应用于导航、控制和信号处理。

    - 算法核心思想
        - 系统状态通过数学模型预测。
        - 传感器测量提供观测值，但含有噪声。
        - 卡尔曼滤波通过权衡预测值和观测值，得到更精确的状态估计。
        - 权衡的关键是卡尔曼增益，它由预测误差和测量噪声共同决定。

主要步骤：
    - 预测
        - 根据系统模型，从上一个状态推算出当前的预测状态和误差协方差。
    - 更新
        - 结合传感器测量，计算卡尔曼增益。
        - 用测量值修正预测状态，得到新的估计状态。
        - 更新误差协方差，反映新的不确定性水平。

在飞控中的应用：
    - 融合IMU数据（陀螺仪和加速度计），得到更稳定的姿态。
    - 融合GPS数据，提供平滑的位置和速度估计。
    - 融合气压计和磁力计，提高高度和航向的可靠性。
    - 在动态环境中减少单一传感器噪声对飞行稳定性的影响。

优势：
    - 能在噪声和不确定性环境下得到最优估计。
    - 实时性好，适合嵌入式系统。
    - 计算量相对可控。

局限性：
    - 标准卡尔曼滤波要求系统是线性且噪声服从高斯分布。
    - 在非线性系统中需要扩展卡尔曼滤波（EKF）或无迹卡尔曼滤波（UKF）。
    - 对系统模型的依赖较强，如果建模不准效果会下降。

相关研究领域
    - 控制工程：飞控、机器人导航、自动驾驶。
    - 信号处理：目标跟踪、图像处理。
    - 航空航天：导航与定位系统。
    - 数学与概率论：状态估计、随机过程建模。

---

### 四元数

四元数总结：

    - 基本定义：四元数是一种扩展复数的数学工具，可以用来描述三维空间中的旋转。它由四个实数构成 q = w + xi + yj + zk。其中 w 称为实部，x y z 称为虚部。在飞控和图形学中，通常用一个长度为1的单位四元数表示旋转。

    - 为什么飞控使用四元数？
        1. 欧拉角虽然直观，但存在万向节死锁问题，某些角度下旋转自由度丢失。
        2. 旋转矩阵能完整表示旋转，但存储需要9个元素，数值运算容易累积误差。
        3. 四元数只需要4个数，存储和运算更高效。
        4. 四元数运算中没有奇异点，适合长时间积分更新。
        5. 四元数和向量的旋转计算比矩阵更快，适合实时控制。

    - 飞控中的应用：
        - 姿态解算：通过陀螺仪测得角速度，积分更新四元数。
        - 传感器融合：利用加速度计和磁力计的数据，校正四元数的漂移。
        - 控制律：四元数直接用于计算姿态误差，驱动PID或其他控制算法。
        - 输出控制：由四元数推导得到无人机机体相对地球的姿态，用于电机功率分配。

四元数与其他方法的对比：
    - 欧拉角
        - 优点：直观，易于理解。
        - 缺点：存在万向节死锁，数学计算复杂。
    - 旋转矩阵
        - 优点：数学表达完整直观。
        - 缺点：存储冗余，数值容易漂移。
    - 四元数
        - 优点：避免死锁，存储高效，运算快速，适合实时应用。
        - 缺点：不直观，不容易直接看出物理含义。

相关研究领域：
    - 数学与线性代数：提供理论基础。
    - 计算机图形学：角色和相机的旋转计算。
    - 控制工程与航空电子：飞控、机器人、航天姿态控制。








## 参考资料

1. 下图是一个穿越机自组教程图文，可以参考。图文的作者是[小白](https://space.bilibili.com/1089966909)。
![image-center](/assets/images/drone_assembly.png){: .align-center}



### 传统 OSD (AT7456E) 与 数字视频 OSD (HDZero/DJI) 的对比表

```markdown
**传统 OSD (AT7456E) vs 数字视频 OSD (HDZero / DJI)**

| 特性                 | 传统 OSD (AT7456E)                          | 数字视频 OSD (HDZero / DJI 等)                 |
|----------------------|---------------------------------------------|------------------------------------------------|
| **信号类型**         | 模拟视频 (PAL / NTSC)                       | 数字视频流 (HD, 数字传输)                      |
| **叠加方式**         | 独立 OSD 芯片（AT7456E）在模拟信号上直接写入字符 | 由飞控固件将数据打包，随数字视频流一起传输      |
| **硬件依赖**         | 需要专门 OSD 芯片（AT7456E 或兼容型号）      | 不需要外部芯片，依赖数字系统协议                |
| **显示清晰度**       | 分辨率低，字符为点阵字体，清晰度受限         | 高分辨率，文字和图形可矢量化，显示更清晰        |
| **兼容性**           | 几乎所有模拟 FPV 眼镜/接收机可用             | 依赖具体数字系统（DJI O3, HDZero, Walksnail）  |
| **扩展性**           | 仅能显示固定字体和简单符号                  | 可显示图形化界面、全彩元素甚至地图              |
| **延迟**             | 极低（芯片级叠加）                          | 与数字视频链路一致，一般 20–40ms               |
| **应用场景**         | 传统 FPV 竞速无人机，低成本模拟系统          | 高清航拍、竞速无人机、需要高信息量显示的场景    |

**结论**  
- AT7456E 方案适合低成本、模拟 FPV 系统。  
- 数字 OSD 方案适合需要高分辨率和扩展功能的现代无人机。  
```