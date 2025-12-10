---
title: fly logs
published: 2025-12-23T09:03:39.514Z
---


挖呀挖呀挖。

叫我挖掘机小王。

从飞行控制器通电启动的那一刻起，TIMESTAMP = 0，然后持续递增。单位微秒。

.ulg 文件是 PX4 使用的二进制日志文件格式（ULog 格式）。它记录了飞行中的传感器原始数据、状态估计、控制指令等信息。

PlotJuggler我用不明白。

PX4 Flight Review，好用。

pyulog，正在用，好用!

'''

pip install pyulog
ulog2csv your_file.ulg

'''


解析第一张图。

原点 (0,0)：无人机起飞时的位置（home position，或者日志开始时的位置参考点）。
橙色线：Estimated Setpoint（估计的位置设定点，即飞行任务中无人机“应该去”的目标位置）
蓝色线：GPS (projected)（实际的 GPS 投影位置，即无人机根据 GPS 测到的实际位置）。



Max Tilt Angle。这个倾斜角是 pitch 和 roll 的综合最大值。

Roll Angle（横滚角） 的图表。

绿色线（Roll Setpoint）是飞行控制器期望的横滚角。