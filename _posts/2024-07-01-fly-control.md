---

title: "Design a Drone PCB，with PX4 Firmware"

published : false

---


Fly control some kind of belongs to control theory catagory.










### fly control, the central chip

F743（商用飞控最经典芯片型号）基本一直缺货。F7其他型号的也是经常缺货。


### gyro

飞控主板给gyro等等芯片供电基本都需要3.3V的电压转换器。我一般先选择 NCP 1117 3.3V regulator。

### IMU

MPU6000,常出现的imu型号，但是停产了。


### pcb layout

电阻电容最好选0402尺寸的。

High-Power and signal cables should be separated as much as is practical.




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
### Quaternions

Summary of Quaternions:

- **Basic Definition:**  
  A quaternion is an extension of complex numbers used to describe rotations in 3D space.  
  It consists of four real numbers:  
  \[
  q = w + xi + yj + zk
  \]  
  where **w** is the real part, and **x, y, z** are the imaginary parts.  
  In flight control and graphics, a **unit quaternion** (length = 1) is commonly used to represent rotation.

- **Why Flight Controllers Use Quaternions:**  
  1. Euler angles are intuitive but suffer from **gimbal lock**, losing rotational freedom at certain angles.  
  2. Rotation matrices fully represent rotation but require 9 elements and accumulate numerical errors.  
  3. Quaternions need only 4 numbers, making storage and computation more efficient.  
  4. Quaternion operations have no singularities, suitable for long-term integration updates.  
  5. Quaternion-vector rotation calculations are faster than matrix multiplication, ideal for real-time control.

- **Applications in Flight Control:**  
  - **Attitude Estimation:** Integrate angular velocity from gyroscopes to update the quaternion.  
  - **Sensor Fusion:** Use accelerometer and magnetometer data to correct quaternion drift.  
  - **Control Laws:** Compute attitude error directly from quaternions to drive PID or other control algorithms.  
  - **Output Control:** Derive the vehicle's attitude relative to Earth from the quaternion for motor power distribution.

**Comparison with Other Methods:**  

| Method          | Advantages                              | Disadvantages                       |
|-----------------|----------------------------------------|-------------------------------------|
| Euler Angles    | Intuitive, easy to understand          | Susceptible to gimbal lock, complex math |
| Rotation Matrix | Complete mathematical representation   | Redundant storage, numerical drift  |
| Quaternion      | Avoids gimbal lock, efficient storage, fast computation, suitable for real-time | Not intuitive, hard to interpret physically |







## references

1. 下图是一个穿越机自组教程图文，可以参考。图文的作者是[小白](https://space.bilibili.com/1089966909)。
![image-center](/assets/images/drone_assembly.png){: .align-center}


### Traditional OSD (AT7456E) vs Digital Video OSD (HDZero / DJI)

| Feature              | Traditional OSD (AT7456E)                          | Digital Video OSD (HDZero / DJI, etc.)             |
|----------------------|---------------------------------------------------|----------------------------------------------------|
| **Signal Type**      | Analog video (PAL / NTSC)                         | Digital video stream (HD, digital transmission)    |
| **Overlay Method**   | Dedicated OSD chip (AT7456E) writes characters directly into analog signal | Flight controller firmware packs data and transmits with the digital video stream |
| **Hardware Dependence** | Requires dedicated OSD chip (AT7456E or compatible models) | No external chip needed, depends on digital system protocol |
| **Display Clarity**  | Low resolution, dot-matrix fonts, limited clarity | High resolution, text and graphics can be vector-based, clearer display |
| **Compatibility**    | Works with almost all analog FPV goggles/receivers | Depends on specific digital systems (DJI O3, HDZero, Walksnail) |
| **Expandability**    | Only fixed fonts and simple symbols supported     | Can display graphical interfaces, full-color elements, even maps |
| **Latency**          | Very low (chip-level overlay)                     | Same as digital video link, typically 20–40ms      |
| **Application Scenarios** | Traditional FPV racing drones, low-cost analog systems | HD aerial filming, racing drones, scenarios requiring high information density |


### SWD interface

 The **SWD interface**, full name **Serial Wire Debug**, is a commonly used **debug interface** for ARM Cortex series chips. It is a simplified alternative to **JTAG**, allowing chip debugging and programming using only **2 signal lines**.



 ### RTK

---

| Document | Type | Content | Link |
|---|---|---|---|
| Carlson Viking RTK White Paper | White Paper | Overview of RTK receiver design and performance | [PDF](https://www.carlsones.com/wp-content/uploads/2025/09/Carlson-Viking-RTK-White-Paper.pdf?utm_source=chatgpt.com) |
| Challenges of Low-Cost GPS/GLONASS RTK for Road Users | Research Paper | Feasibility and challenges of low-cost GNSS devices using RTK | [PDF](https://www.insidegnss.com/auto/novdec13-WP.pdf?utm_source=chatgpt.com) |
| The Role of RTK in the Autonomous System Sensor Suite | White Paper | RTK in autonomous driving sensor fusion, moving baseline RTK, heading estimation | [PDF](https://www.swiftnav.com/sites/default/files/whitepapers/moving_baseline_white_paper_092017.pdf?utm_source=chatgpt.com) |
| Agriculture White Paper: RTK Base Station Networks | White Paper | RTK networks for agriculture and industrial adoption | [PDF](https://www.gpsags.com/media/RTK-Networks-Whitepaper.pdf?utm_source=chatgpt.com) |
| Tersus ExtremeRTK Technology White Paper | White Paper | Hardware, algorithms, anti-jamming, multi-frequency GNSS, GNSS/INS fusion | [PDF](https://www.tersus-gnss.com/assets/upload/file/20210926114705660.pdf?utm_source=chatgpt.com) |
| PPP-RTK Market and Technology Report (EUSPA) | Report | Market and technology trends of PPP-RTK hybrid positioning | [PDF](https://www.euspa.europa.eu/sites/default/files/calls_for_proposals/rd.03_-_ppp-rtk_market_and_technology_report.pdf?utm_source=chatgpt.com) |
| User Guidelines for Single Base Real Time GNSS Positioning (NOAA/NGS) | Guideline | Practical guide for single base RTK, error sources, system setup | [PDF](https://www.ngs.noaa.gov/PUBS_LIB/NGSRealTimeUserGuidelines.v2.1.pdf?utm_source=chatgpt.com) |
| Algorithm to Assist Robust Filter for Tightly Coupled RTK/INS Navigation | Research Paper | RTK tightly coupled with INS using robust Kalman filtering | [PDF](https://www.mdpi.com/2072-4292/14/10/2449?utm_source=chatgpt.com) |
| Crowdsourcing RTK: New GNSS Positioning Framework | Journal Paper | Using crowdsourced GNSS data from vehicles to improve atmospheric correction for RTK | [Article](https://satellite-navigation.springeropen.com/articles/10.1186/s43020-024-00135-8?utm_source=chatgpt.com) |

---




### Dji m200 serials component

#### Xilinx Zynq-7000 系列


#### lc1860

A baseband processor chip developed by **Leadcore Technology** (a subsidiary of Datang Telecom).  


## 










#### Optical Flow Sensor

An optical flow sensor is a device that measures motion of objects, surfaces, or the sensor itself by detecting changes in the pattern of light across an image. It is commonly used in robotics, drones, and navigation systems to estimate velocity, displacement, or position without relying on GPS.


#### GNSS

Quectel LC29H GPS. Recently I need to embed it in a project.

A high-precision GNSS module (Global Navigation Satellite System) produced by Quectel Wireless Solutions.

Other Series of Quectel GNSS Modules (besides LC29)?
| Series   | Description | Typical Applications | Official Link |
|----------|-------------|----------------------|---------------|
| **LC29H** | Dual-band, high-precision GNSS module. Supports RTK (Real-Time Kinematic) and optional Dead Reckoning (DR). | UAVs, autonomous driving, surveying, precision agriculture | [LC29H](https://www.quectel.com/product/gnss-lc29h/?utm_source=chatgpt.com) |
| **LC29T** | High-integrity GNSS timing module. Provides precise timing outputs (10 MHz, PPS). | Telecommunications, power grid, critical infrastructure | [LC29T](https://www.quectel.com/product/gnss-lc29t/?utm_source=chatgpt.com) |
| **LC76G** | Compact, single-band GNSS module. Multi-constellation, high sensitivity, low power. | IoT, asset tracking, general navigation | [LC76G](https://www.quectel.com/product/gnss-lc76g-series/?utm_source=chatgpt.com) |
| **LG69T** | Dual-band GNSS module with optional RTK and DR, automotive-grade reliability. | Automotive navigation, ADAS, UAVs | [LG69T](https://www.quectel.com/product/gnss-lg69t-series/?utm_source=chatgpt.com) |
| **LG77L** | Single-band, multi-constellation, compact and ultra-low power GNSS module. | Asset tracking, wearables, IoT devices | [LG77L](https://www.quectel.com/product/gnss-lg77l-series/?utm_source=chatgpt.com) |


Talking about Quectel. By the way, Quectel is very closed to iot. They can be talked together.

Quectel is a global IoT solutions provider specializing in cellular modules, Wi-Fi/GNSS modules, antennas, RTK corrections and IoT device certification services QuectelQuectel. Founded in 2010 and based in Shanghai, China, the company has grown to employ between 1,001-5,000 people as of July 2024.





#### lidar

VL53LOX.

Maximum range of VL53LOX is 2m.


### imu

BMI088.

I heard of it when I want to buy a board to run PX4. 





## rtk

[rtk explained](https://www.youtube.com/watch?v=ieearzWTCZw)

However, how do antenna detect phase?


The carrier phase measurement?

d=N•Л+Лф

### remote contrller design

Industrial drones typically use 2.4GHz or 900MHz frequencies for control, with some requiring licensed bands for extended range. Consider dual-redundant radio systems for mission-critical applications.

If the drone must be physically connected to fly, 2.4GHz can be avoided.






## PX4

PX4 Ground Control Station.

PX4 ground control station refers to ground control software (QGroundControl, MAVProxy).

PX4's Publish-Subscribe Mechanism.

PX4 includes a uORB (micro Object Request Broker), which is similar to an event system / publish-subscribe framework.

Different modules (sensors, controllers, mission management) don't call each other directly, but interact through message publishing and subscription.

This is a use case of event-driven framework + circular buffer in flight control systems.

Sensor module → publishes IMU_DATA; Estimator (EKF) → subscribes to IMU_DATA, then publishes STATE_ESTIMATE; Controller → subscribes to STATE_ESTIMATE and RC_INPUT, outputs control signals; Motor driver → subscribes to CONTROL_OUTPUT, drives PWM.

MAVLink.

MAVLink is like the "language" between drones and ground control stations, responsible for information transmission, command delivery, and status synchronization. Mastering MAVLink allows you to independently develop ground control software or drone control programs.

PX4 SITL.

SITL (Software-in-the-Loop) is a simulation technique where the actual flight control software runs on your computer, but instead of controlling real hardware, it interfaces with a physics simulator.










## ask

why drone's battery has a output rate term?





### Distance sensor

Distance sensors provide distance measurement that can be used for terrain following, terrain holding (i.e. precision hovering for photography), improved landing behaviour (conditional range aid), warning of regulatory height limits, collision prevention, etc.

One compatible distance sensor for px4 quadcopter is ARK Flow.

ARK Flow is an open source DroneCAN optical flow, distance sensor, and IMU module.



### pixhawk fmu differences in short

Just listed at 2025.

1. **FMUv2 version**: Cortex-M4 core, 1 MB memory, with co-processor  
2. **FMUv3 version**: Cortex-M4 core, 2 MB memory, with co-processor  
3. **FMUv4 version**: Cortex-M4 core, 2 MB memory, without co-processor  
4. **FMUv4 Pro version**: Cortex-M4 core, 2 MB memory, with co-processor  
5. **FMUv5 version**: Cortex-M7 core, 2 MB memory, with co-processor  
6. **FMUv5x version**: Cortex-M7 core, 2 MB memory, with co-processor  
7. **FMUv6x version**: Cortex-M7 core, 2 MB memory, with co-processor  

### pixhawk open-source pcb designs 

Pixhawk has fmu2&fmu1 pcb opened source. Others just have pin-out.

### embed ros2 & px4

Using linux to do this.

Communication between ROS 2 and PX4 uses middleware that implements the XRCE-DDS protocol. This middleware exposes PX4 uORB messages as ROS 2 messages and types, effectively allowing direct access to PX4 from ROS 2 workflows and nodes. The middleware uses uORB message definitions to generate code to serialise and deserialise the messages heading in and out of PX4. These same message definitions are used in ROS 2 applications to allow the messages to be interpreted.



### remote controller


AT9S.

### Tricopter

The Tricopter uses 3 motor / propeller propulsion units with a servo to rotate one of them to compensate for adverse yaw.
Tricopters were popular early on when the brushless motor / propeller units were new and scarce.
They suffer from less than stellar performance and do not scale well to larger sizes.
But they still have some popularity for small, light hobby use applications.
Because this is primarily an outdated design we will not expand further on it in this article.


### px4 and ardupilot

They just has a lot in coomon. Such as marlink.




### Difference Between CAN and DroneCAN

1. CAN (Controller Area Network)
- **Definition**: A low-level, robust communication protocol originally developed by Bosch (1980s).  
- **Purpose**: Provides reliable communication between microcontrollers and devices without a host computer.  
- **Characteristics**:  
  - Multi-master, message-based protocol.  
  - Widely used in automotive, industrial, and robotics.  
  - Defines only the **data link** and **physical layer**.  
  - Does **not** specify how messages are structured for a particular application.  

---

2. DroneCAN
- **Definition**: An **open, higher-level protocol** built on top of CAN, designed for UAVs and robotics.  
- **Origin**: Fork of UAVCAN v0 (after UAVCAN v1 introduced breaking changes).  
- **Purpose**: Standardizes communication for drone components (ESCs, GPS, sensors, flight controllers).  
- **Characteristics**:  
  - Runs over CAN bus.  
  - Defines **message sets** (e.g., battery status, GPS data, ESC control).  
  - Plug-and-play: devices auto-discover each other.  
  - Supported by ArduPilot and PX4.  
  - Open-source, maintained by the DroneCAN community.  

---



### RTOS Used by ArduPilot

ArduPilot does **not depend on a single RTOS**. It uses an **abstraction layer (AP_HAL)** to support different operating systems and bare-metal boards.  

1. **ChibiOS**  
   - Lightweight open-source RTOS.  
   - Default for STM32-based flight controllers (Pixhawk, Cube, etc.).  
   - Provides task scheduling, drivers, and hardware abstraction.  

2. **NuttX**  
   - POSIX-like RTOS (used by PX4).  
   - Historically supported by ArduPilot, but **less common today**.  

3. **Linux**  
   - Runs ArduPilot in SITL (Software In The Loop).  
   - Also used for companion-computer boards (e.g., Raspberry Pi, BeagleBone).  

4. **No RTOS (bare-metal)**  
   - Early ArduPilot boards ran without RTOS, using cooperative scheduling inside the main loop.  
   - Still possible on very limited hardware.  


   
