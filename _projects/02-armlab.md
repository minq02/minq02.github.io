---
name: RX200 Robotic Arm / Intel Realsense Lidar Camera
tools: [ROS2, Python, OpenCV, Intel Realsense, 5-DOF, Kinematics]
image: https://minq02.github.io/assets/02_armlab.gif
description: Developed autonomous motion control for the RX200 5-DOF robotic arm using ROS2 and Python, with depth and block detection using an Intel Realsense Lidar Camera and OpenCV.
---

# RX200 Robotic Arm / Intel Realsense Lidar Camera
<br>

### Overview
This project involved developing autonomous motion control for the **RX200 Robotic Arm** with **5 Degrees of Freedom (DOF)**, integrating an **Intel Realsense Lidar Camera** for depth sensing and object detection. Using **ROS2 in Python**, the project focused on implementing kinematic calculations and optimizing robotic movements by calculating distances between waypoints.

<br>

### Key Contributions
- **Autonomous Motion Control**: Developed using **ROS2** and **Python** to control the 5-DOF RX200 robotic arm autonomously.
- **Camera Calibration**: Calibrated **Intel Realsense Lidar Camera** parameters (extrinsic/intrinsic) to achieve accurate depth perception and block detection with **OpenCV**.
- **Kinematics Implementation**: Implemented **forward** and **inverse kinematics**, calculating distances between waypoints for smooth, efficient motion.

<br>

### Camera Calibration and Block Detection
- **Camera Calibration**: Calibrated the **extrinsic** and **intrinsic** parameters of the Intel Realsense camera.
    <br>
    *Insert image showing the camera calibration process or visual output*
    <br>
- **Depth and Block Detection**: Performed depth detection and block detection using **OpenCV**, identifying objects for the robotic arm to interact with.
    <br>
    *Insert GIF or image showing block detection in action*
    <br>

<br>

### Kinematics and Motion Control
- **Forward/Inverse Kinematics**: Used to control the arm's movement, calculate angles, and position the end-effector for different tasks.
    <br>
    *Insert image showcasing kinematic calculations or visualizing robot’s motion in RViz or similar tool*
    <br>
- **Waypoint Optimization**: Calculated distances between waypoints to optimize speed and ensure smooth transitions between movements.
    <br>
    *Insert diagram or video of the robotic arm's movement between waypoints*
    <br>

<br>

### Challenges and Results
- Faced challenges with achieving accurate depth perception and precise robot movement, especially when detecting and interacting with objects.
- Successfully implemented autonomous block detection and optimized the robot’s motion control using waypoints and kinematics.
    <br>
    *Insert video or GIF showing RX200 Robotic Arm performing autonomous tasks*
    <br>

<br>

### Future Improvements
- Improve depth detection accuracy to handle more complex object shapes and sizes.
- Enhance kinematic algorithms to optimize motion efficiency for different task types.
- Implement real-time feedback to adapt to dynamic environments and moving objects.
