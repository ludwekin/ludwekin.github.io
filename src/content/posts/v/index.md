---
title: offboard & px4 & aruco
published: 2025-12-22T09:51:06.619Z
---


offboard模式怎么给px4发送xy 地球固联坐标系位置要求?


可以通过 SET_POSITION_TARGET_LOCAL_NED 或 SET_POSITION_TARGET_GLOBAL_INT 消息发送位置。

SET_POSITION_TARGET_LOCAL_NED，坐标系是Local NED（相对飞控起飞点，X北，Y东，Z下）。

px4内部怎么估计自己的速度呢？