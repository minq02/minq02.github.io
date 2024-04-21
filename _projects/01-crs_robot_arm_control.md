---
name: CRS Robot Arm Control
tools: [C, MATLAB, SIMULINK]
image: https://minq02.github.io/assets/crs_robot.gif
description:  PID control, inverse dynamics joint control, and impedance control of Catalyst-5 robot arm
---

# CRS Robot Arm Control
<br>

### Brief overview
The project aimed to program a robot arm to perform a series of tasks using position/impedance control. 
<br>

### Video demo
<!-- {% include elements/video.html id="SXJP4yIiKOU" %} -->
<br>

### Task Description
The image shows a schematic representation of the four tasks designed for a robot arm control project:
<br>
<img src="{{ site.url }}{{ site.baseurl }}/assets/1_task.webp" style="height: 399px; width:700px;"/>
<br>
1. **Peg in a hole** 
    * The task involves inserting a peg into a hole with a tight clearance. This requires precise force control to avoid damaging the components.
2. **Obstacle** 
    * The robot arm must navigate around a rectangular obstacle. This requires path planning to create a collision-free route and position control to execute the planned path accurately.
3. **Zigzag groove** 
    * The arm must follow a zigzag groove with a tight clearance, necessitating path planning to determine the optimal path through the groove and force control to maintain the correct path without excessive pressure that could cause deviation or damage.
4. **Egg** 
    * The final task involves carefully lowering an egg onto a spring. This requires delicate force control to ensure that the egg is not broken by the descent.

Each task is designed to test different aspects of the robot arm's control systems, focusing on both the precision of movement and the modulation of applied force.