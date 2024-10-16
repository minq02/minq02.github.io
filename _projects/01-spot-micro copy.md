---
name: SpotMicro Robot / Jetson Nano
tools: [Jetson Nano, ROS2, LiDAR, IMU, Ubuntu]
image: https://minq02.github.io/assets/1_spot_micro.gif
description: Configured Jetson Nano with Ubuntu 20.04 to control the robot, integrated ROS2 Foxy for motor control and communication, and added LiDAR and IMU for environment sensing.
---

# SpotMicro Robot / Jetson Nano
<br>

### Overview
This project focuses on configuring a **Jetson Nano** running **Ubuntu 20.04** to control a **SpotMicro** robot's movements, integrating **ROS2 Foxy** for managing motor control and communication between sensors and actuators, and utilizing sensors such as **a 2D LiDAR** and **MPU6050 IMU** for environment sensing and state estimation.

<br>

### Key Contributions
- **Configured Jetson Nano with Ubuntu 20.04**: Set up the development environment and drivers to control the SpotMicro robot's motors.
- **Integrated ROS2 Foxy**: Established motor control and communication between sensors and actuators using ROS2 Foxy's pub/sub architecture.
- **Added 2D LiDAR and MPU6050 IMU**: Used a **2D LiDAR** for mapping and obstacle detection, and an **MPU6050 IMU** for state estimation and motion tracking.

<br>

### Environment Setup
- Installed and configured **Ubuntu 20.04** on Jetson Nano.
    <br>
    *Insert screenshot or image of Jetson Nano running Ubuntu 20.04*
    <br>
- Installed **ROS2 Foxy**, configured motor control, and integrated sensors for communication.
    <br>
    *Insert image showing ROS2 Foxy running on Jetson Nano, with sensor data visualized*
    <br>

<br>

### Sensor Integration
- **2D LiDAR**: Integrated for real-time environment sensing and mapping.
    <br>
    *Insert GIF or image of LiDAR sensor data in action*
    <br>
- **MPU6050 IMU**: Added for orientation and motion tracking to support state estimation.
    <br>
    *Insert image or GIF showing data from the IMU in RViz or a similar tool*
    <br>

<br>

### Challenges and Results
- Faced challenges in managing low-latency communication between the sensors and actuators in ROS2.
- Achieved real-time control of SpotMicro's movements and sensor-based environment awareness.
    <br>
    *Insert video or GIF showcasing SpotMicro in action, with real-time sensor feedback*
    <br>

<br>

### Future Improvements
- Explore integrating additional sensors for enhanced perception.
- Further refine the real-time control loop to optimize performance.
- Expand the system for more complex autonomous navigation tasks.
