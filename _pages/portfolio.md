---
title: ""
layout: home
permalink: /
classes: wide
redirect_from:
  - /portfolio/
  - /portfolio
---
<h2 class="portfolio-section-title">About Me</h2>
<p class="about-me-text">
  I'm a <strong>Master’s student in Robotics</strong> at the <strong>University of Michigan</strong>, advised by Prof. 
  <a href="https://robotics.umich.edu/people/faculty/maani-ghaffari/">Maani Ghaffari</a>
  at the Computational Autonomy and Robotics Laboratory (<a href="https://curly.engin.umich.edu/">CURLY</a>).<br><br>
  My research focuses on robust <strong>3D perception</strong> and <strong>state estimation</strong> in unstructured environments. 
  I am currently architecting a <strong>semantic-aware SLAM</strong> that fuses object detections, geometric primitives, and odometry 
  into a unified <strong>GTSAM factor graph</strong> to achieve metrically consistent mapping.
  <br><br>
  In parallel, I serve as the <strong>sole researcher</strong> for an industry-sponsored project with the <strong>Honda Research Institute</strong>, 
  designing robust 3D pose estimation and localization frameworks for <strong>autonomous marine vessels</strong>. 
  <br><br>
  Previously, I led robotic fleet deployment at <strong>Amazon Robotics</strong> and studied Mechanical Engineering at <strong>UIUC</strong>.
</p>

<h2 class="portfolio-section-title">Work Experience</h2>
<table>
  <tbody>
    <tr>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/ar.jpg" alt="ar" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Amazon Robotics</strong> <br> 01/2025 - 06/2025 <br> Robotics System Eng Co-op</td>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/hinetics.jpg" alt="hinetics" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Hinetics</strong> <br> 08/2023 - 12/2023 <br> Mechanical Eng Intern</td>
    </tr>
  </tbody>
</table>

<h2 class="portfolio-section-title">Education</h2>
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

<h2 class="portfolio-section-title">Active Projects</h2>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/objectslam/gsam2.gif" alt="object-slam">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Object-SLAM: 2D Semantic Mapping with Grounded-SAM 2 + GTSAM</h3>
    </div>
    <div class="link-row">
      <a href="/project/objectslam" class="more-link">Project Page</a>
    </div>
    <div class="text-content">
      <p>Zero-shot masks → tracked landmarks → SE(2) factor graph for a clean, metrically consistent object map and smoothed trajectory.</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/depthany/depthany.gif" alt="depthanything">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Calibrated Monocular Depth on the WAM-V</h3>
    </div>
    <div class="link-row">
      <a href="/project/depthanything" class="more-link">Project Page</a>
    </div>
    <div class="text-content">
      <p>Depth-Anything V2 scaled to meters via inverse-depth fit; project RGBD to colored point clouds and validate against LiDAR in VRX and field runs.</p>
    </div>
  </div>
</div>

<h2 class="portfolio-section-title">Completed Projects</h2>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/nightorb/example.gif" alt="nightorbslam">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Exposure-Robust Masked ORB-SLAM3</h3>
    </div>
    <div class="link-row">
      <a href="https://github.com/minq02/NightMaskedORBSLAM3" class="more-link">Code</a>
      <a href="https://drive.google.com/file/d/1_gdCck7X44pn3_uRaDPN9sXfSpSAttce/view?usp=sharing" class="more-link">PDF</a>
    </div>
    <div class="text-content">
      <p>Lighting-invariant frontend utilizing adaptive gamma correction and exposure-aware masking. Reduced nighttime trajectory error (RMSE) by 9x with only 30ms latency overhead.</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/tem/example.png" alt="temseg">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>TEM Cell Multi-class Segmentation using Attention U-Net</h3>
    </div>
    <div class="link-row">
      <a href="https://github.com/minq02/TEMCellSegmentation" class="more-link">Code</a>
      <a href="https://drive.google.com/file/d/1FIUObf7vHrLs1QLaQfTDSbql5cfIX0gQ/view?usp=sharing" class="more-link">PDF</a>
    </div>
    <div class="text-content">
      <p>Multi-class semantic segmentation of high-res scans using sliding-window inference. Optimized hybrid Cross-Entropy + Dice loss to resolve class imbalance (0.79 mDice).</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/armlab/armlab.gif" alt="armlab">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Vision-Guided 5-DoF Arm: Pick & Place with RealSense</h3>
    </div>
    <div class="link-row">
      <a href="/project/armlab" class="more-link">Project Page</a>
    </div>
    <div class="text-content">
      <p>ROS 2 pipeline with camera calibration, block/depth detection, and IK. Built teach-and-repeat motions and tuned speeds for reliable autonomous grasping.</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/botlab/botlab.gif" alt="botlab">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Mobile Robot: SLAM + A* Exploration</h3>
    </div>
    <div class="link-row">
      <a href="/project/botlab" class="more-link">Project Page</a>
    </div>
    <div class="text-content">
      <p>Implemented PID velocity control with gyrodometry; built occupancy-grid SLAM with MCL in C++; executed A* planning and frontier-based exploration.</p>
    </div>
  </div>
</div>

<div class="container">
  <div class="image-container">
    <a>
      <img src="{{site.baseurl}}/assets/images/bigant/bigant.png" alt="bigant">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <h3>Hexapod Staircase Edge Detection</h3>
    </div>
    <div class="link-row">
      <a href="/project/bigant" class="more-link">Project Page</a>
    </div>
    <div class="text-content">
      <p>AprilTags + homography with background extraction, Radon-based edge finding, and peak detection to localize stair edges for gait analysis.</p>
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
  flex: 0 0 260px;
  height: 130px;
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
  /* opacity: 0.8; */
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
  /* margin-bottom: 0.25rem; */
  margin-bottom: 2px;
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
  font-size: 0.65rem;
  line-height: 1.4;
  color: #474747ff;
}

.portfolio-section-title {
  font-size: 1.1rem;   
  margin-bottom: 0.5rem;
}

.about-me-text {
  font-size: 0.85rem;
  line-height: 1.5;
}

/* --- New "More Info" Link Style --- */
.link-row {
  display: flex;
  gap: 4px;              /* Space between [Project Page] and [Video] */
  align-self: flex-start; /* Stops the row from stretching full width */
  margin-bottom: 4px;     /* Space below the links before text starts */
}

.more-link,
.more-link:visited {
  display: inline-block;
  margin-top: 0;      /* Adjust gap as needed */
  margin-bottom: 2px;
  align-self: flex-start;
  font-size: 0.65rem;    /* The size for the link AND the brackets */
  font-weight: 600;
  /* color: #004ea8;*/
  color: #0077cc;
  text-decoration: none;
  transition: color 0.2s;
}

/* Adds the [ before the link */
.more-link::before {
  content: "[";
  color: #555; /* Brackets are dark grey, not blue */
  margin-right: 1px;
  font-weight: 400; /* Optional: Make brackets thinner than the text */
}

/* Adds the ] after the link */
.more-link::after {
  content: "]";
  color: #555;
  margin-left: 1px;
  font-weight: 400;
}

.more-link:hover {
  color: #003366;
  text-decoration: underline;
}

/* OPTIONAL: If you want the brackets to stay non-underlined on hover */
.more-link:hover::before,
.more-link:hover::after {
  text-decoration: none; 
}

</style>