---
title: ""
layout: home
permalink: /
classes: wide
redirect_from:
  - /portfolio/
  - /portfolio
---

## Experience
<table>
  <tbody>
    <tr>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/ar.jpg" alt="ar" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Amazon Robotics</strong> <br> 01/2025 - Present <br> Robotics System Eng Co-op</td>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/hinetics.jpg" alt="hinetics" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Hinetics</strong> <br> 08/2023 - 12/2023 <br> Mechanical Eng Intern</td>
    </tr>
  </tbody>
</table>

## Education
<table>
  <tbody>
    <tr>
      <td style="border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/umich.jpg" alt="umich" width="60"></td>
      <td style="border-bottom-width:0;">
        <strong>University of Michigan</strong> <br> 08/2024 - Present <br> M.S. in Robotics
      </td>
      <td style="border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/uiuc.jpg" alt="umich" width="60"></td>
      <td style="border-bottom-width:0;">
        <strong>University of Illinois Urbana-Champaign</strong> <br> 08/2020 - 05/2024 <br> B.S. in Mechanical Engineering
      </td>
    </tr>
  </tbody>
</table>

## Projects

<div class="container">
  <div class="image-container">
    <a href="/project/bigant">
      <img src="{{site.baseurl}}/assets/images/bigant/bigant.png" alt="bigant">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <a href="/project/bigant" class="title-link">
        <h3>Staircase Detection for Hexapod Robot Using Image Processing</h3>
      </a>
    </div>
    <div class="text-content">
      <p>This project uses computer vision techniques—background extraction, homography, masking, Radon Transform, and peak detection—to accurately identify stair edges for robotic gait analysis and navigation.</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a href="/project/spotmicro">
      <img src="{{site.baseurl}}/assets/images/spotmicro/spotmicro.gif" alt="spotmicro">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <a href="/project/spotmicro" class="title-link">
        <h3>Development and Navigation of an Open-Source Quadruped Robot: SpotMicroAI</h3>
      </a>
    </div>
    <div class="text-content">
      <p>Developed and programmed an open-source SpotMicro, a quadruped robot inspired by Boston Dynamics. Controlled using a Jetson Nano running Ubuntu 20.04, integrated with ROS2 Foxy. Equipped with a 2D LiDAR and an IMU for mapping and navigation.</p>
    </div>
  </div>
</div>

<style>
.container {
  display: flex;
  margin-bottom: 10px;
  gap: 10px;
}

.image-container {
  flex: 0 0 200px;
  height: 100px;
  overflow: hidden;
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: opacity 0.2s;
}

/* Special handling for logo-style images in the experience/education tables */
table img {
  width: 60px;
  height: 60px;
  object-fit: contain;
}

.image-container img:hover {
  opacity: 0.8;
}

.text-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100px;
  justify-content: flex-start;
}

.header-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 0.25rem;
}

.header-row h3 {
  margin: 0;
  font-size: 0.8rem;
  color: #333;
  transition: color 0.2s;
  line-height: 1.2;
}

.title-link {
  text-decoration: none;
  color: inherit;
}

.title-link:hover h3 {
  color: #0066cc;
  text-decoration: underline;
}

.text-content p {
  margin: 0;
  font-size: 0.6rem;
  line-height: 1.4;
  color: #666;
}
</style>