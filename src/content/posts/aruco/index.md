---
title: aruco
published: 2025-12-22T09:41:35.315Z
---

本文关于aruco。


aruco码一旦视野里面出现了遮挡，比如正方形的边被挡住了,直接就检测不到了咋办?

1. 用叠层/多层aruco码。

2. 改 OpenCV 的 aruco.detectMarkers参数。

3. 方法多得是。。。

4. 换码。

问题背景，下次说。




## 整理

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

