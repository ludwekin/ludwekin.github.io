---

title : "Control Theory"

published : true

---

### Sliding Mode Control, SMC

滑模控制（Sliding Mode Control, SMC）是在 20世纪50年代末至60年代初由前苏联学者 В.И.Уткин (Vladimir I. Utkin) 等人系统提出的。Utkin 在 1977 年发表的论文和后来出版的《Variable Structure Systems with Sliding Modes》奠定了 SMC 的理论基础。 

### Variable Structure Control, VSC

变结构控制理论 (Variable Structure Control, VSC) 是一种非线性控制方法。它的基本思想是：  

    - 不固定采用一种控制律，而是根据系统状态在不同区域切换不同的控制律。  
    - 通过这种切换，使系统在动态过程中表现出期望的稳定性。  

### spwm

SPWM (Sinusoidal PWM, 正弦脉宽调制)
    - 一种特殊的 PWM，用 正弦波信号 去调制三角波载波，输出脉冲宽度随正弦规律变化。  
    - 等效输出电压的基波接近正弦波，谐波含量低。  
    - 应用：逆变器（DC → AC）、UPS、电动车驱动。  

### pwm

PWM (Pulse Width Modulation, 脉宽调制)**  
    - 用固定频率的脉冲，通过改变“高电平时间/周期”的占空比来控制电压或功率。  
    - 波形是矩形脉冲。  
    - 应用：电机调速、LED 调光、开关电源。  
