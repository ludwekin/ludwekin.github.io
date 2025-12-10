---
title: fractal_marker
published: 2026-01-09T09:59:09.454Z
---


source precision_landing_ws/install/setup.bash && ros2 run px4_precision_landing precision_landing_mode

ros2 topic hz /camera/image_raw

unset HEADLESS && make px4_sitl_default gz_x500_fractal

如果我想替换掉这个项目的aruco tag 检测逻辑，换成更robust的fractal marker（我自己找的开源项目） 的逻辑，可以怎么改？

1. 创建Fractal Marker检测器包装类。

2. 修改LandingTargetDetector以支持Fractal Marker。

landing_target_detector.hpp。

3. 修改LandingTargetDetector实现文件。

landing_target_detector.cpp。

4. 创建Fractal Marker消息类型。

FractalMarkerTarget.msg。

5. 更新CMakeLists.txt。

6. 更新配置文件。

landing_target_detector.yaml。

可以通过YAML文件轻松调整各种检测参数。




## 关于这个开源的fractal_marker项目

1. 这个项目使用了IPPE算法（Infinitesimal Plane-based Pose Estimation）来计算rvec和tvec。

2. 这个项目可以用于AR应用，在标记上叠加3D虚拟物体。

3. [fractal_marker_docs](https://docs.google.com/document/u/0/d/1SdsOTjGdu5o8gy2Ot2FDqYDS9ALgyhOBJcJHOZBR7B4/mobilebasic?pli=1#h.kxr001iev8cv).

4. 有意思的是，这个项目可以把虚拟停机坪覆盖到码上面。精度很高。

5. [releases](https://sourceforge.net/projects/aruco/files/).



6. unset HEADLESS
PX4_GZ_WORLD=fractal_marker_landing make px4_sitl gz_x500_mono_cam_down

cp src/circle_pilot/gz/worlds/precision_landing.sdf ~/Desktop/PX4-Autopilot/Tools/simulation/gz/worlds/

cp -r src/circle_pilot/gz/models/landingtag ~/Desktop/PX4-Autopilot/Tools/simulation/gz/models/

cp fractal_marker_4L.png src/circle_pilot/gz/models/landingtag/

unset HEADLESS && PX4_GZ_WORLD=fractal_marker_landing make px4_sitl gz_x500_mono_cam_down


[text](https://app.gazebosim.org/fuel/worlds)

gz sim /home/kaka/Desktop/PX4-Autopilot/Tools/simulation/gz/worlds/precision_landing.sdf --verbose

找到问题了，Gazebo找不到landingtag模型。这是因为Gazebo的模型路径没有包含我们的模型目录。

export GZ_SIM_RESOURCE_PATH=\"$PX4_DIR/Tools/simulation/gz/models:$PX4_DIR/Tools/simulation/gz/worlds\""
echo "   cd $PX4_DIR"
echo "   PX4_GZ_WORLD=precision_landing make px4_sitl gz_x500_mono_cam_down"