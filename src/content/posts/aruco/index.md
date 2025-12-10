---
title: 视觉定位，三种常用码的介绍
published: 2025-12-22T09:41:35.315Z
---

本文关于 aruco , fractal marker 还有 opencv。

## aruco

1. aruco码一旦视野里面出现了遮挡，比如正方形的边被挡住了,直接就检测不到了咋办?

    我搜到的解决办法：

        - 用叠层/多层aruco码

        - 改 OpenCV 的 aruco.detectMarkers参数（没用我试过了）

        - 换码

2. aruco tag我实际测试下来精度完全够用。但是太容易被遮盖而丢失目标了！！这是代码决定的，和aruco无关。



## apirl tag

AprilTag被NASA采用了。

## farctal marker

AprilTag有一个明确的"正面"方向。

AprilTagDetector 不负责位姿。

既然april tag本来有代码可以用（官方有库和项目），为啥我还需要opencv呢？

AprilTag 官方 C 库只干一件事，输入一张灰度图，输出Tag 的 ID + 四个角点。

而opencv有很多基础又好用的功能。所以这俩一般要一起用。

当 相机正对 AprilTag，Tag 没有旋转 时，













## 知识点整理

对于一个边长为 L 的正方形 ArUco，通常定义四个角的坐标为：

obj_points = np.array([
    [-L/2,  L/2, 0],   # 左上角
    [ L/2,  L/2, 0],   # 右上角
    [ L/2, -L/2, 0],   # 右下角
    [-L/2, -L/2, 0]    # 左下角
], dtype=np.float32)

obj_points：标记在 物体坐标系下的真实 3D 坐标（比如 ArUco 四角点，单位米）。不是世界坐标系，而是相对于标记本身中心或某个原点。

corners[i]：图像中标记的四个角的像素坐标。

camera_matrix 和 dist_coeffs：相机内参和畸变系数。

flags=cv2.SOLVEPNP_IPPE_SQUARE：使用 IPPE 算法专门求方形平面标记的位姿，稳定且精度高。  