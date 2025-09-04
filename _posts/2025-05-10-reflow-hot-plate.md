---

title : "Mini Reflow Hot Plate"

published : false

---

## Goal

设计一台迷你回流焊接台（更准确说是迷你回流热板 / Mini Reflow Hot Plate），USB-C PD 协议输入，可跑标准回流曲线（preheat→soak→reflow→cool），功率约 60–100 W，峰值温度 230–250 °C。

## Key Specs

	- 输入（Input）：USB-C PD，**20 V/3 A（60 W）**起步；若支持 EPR 可到 28–36 V/5 A（140–180 W），但更复杂。
	- 加热平台（Heater Plate）：小尺寸铝板或厚铜块（例如 50×50×6 mm），表面黑化/阳极氧化提升吸热与温场均匀性。
	- 传感（Sensing）：K 型热电偶 + 放大/数字转换器（或高温 NTC/RTD），靠近贴片区域埋入/贴附。
	- 控制（Control）：MCU 运行回流曲线状态机 + PID，具备安全联锁（过温、开路传感器、看门狗）。
	- 输出驱动（Power Stage）：低压大电流 MOSFET PWM 驱动电阻加热器；必要时加入电流/温度保险。
	- 界面（UI）：旋钮+小屏（如 0.96” OLED）或仅按键+LED 指示；可选 USB-C/UART 升级与数据记录。