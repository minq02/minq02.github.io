---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/armlab/armlab.gif
classes: wide
---
<h2 class="title">Vision‑Guided Robot Arm Control with Intel RealSense</h2>

<p class="text">
This project turns the Interbotix ReactorX‑200 into a self‑reliant pick–place system. A calibrated RealSense depth camera estimates block poses; a dual‑path inverse kinematics (IK) stack computes feasible grasps; and a ROS 2 execution loop carries out smooth, repeatable motion. The design goal wasn’t merely “move once,” but <em>operate continuously</em> under ordinary lab messiness—lighting changes, minor camera bumps, and imperfect block placement—without human corrections.
</p>

<h3>Video Demo: Click to Grasp &amp; Place</h3>
<iframe
    width="100%"
    height="50%"
    src="https://www.youtube.com/embed/iK9BaI25W5I"
    frameborder="0"
    allow="autoplay; encrypted-media"
    allowfullscreen
>
</iframe>

<h3>Video Demo: Teach &amp; Play</h3>
<iframe
    width="100%"
    height="50%"
    src="https://www.youtube.com/embed/KAYSb8yPItA"
    frameborder="0"
    allow="autoplay; encrypted-media"
    allowfullscreen
>
</iframe>

<h3>System Overview</h3>
<p class="text">
The software stack has three layers: <br>(1) <strong>Perception</strong> (color segmentation + depth, AprilTag‑based extrinsic calibration, homography for a bird’s‑eye view) <br>(2) <strong>Planning</strong> (algebraic IK for speed and a least‑squares IK fallback within joint limits)<br>(3) <strong>Execution</strong> (teach‑and‑repeat primitives plus guarded motion, all in ROS 2).<br><br>The arm publishes/consumes standard messages (<code>/rx200/joint_states</code>, <code>/camera/*</code>, <code>vision_msgs</code>) and re‑checks detections after every sequence to stay robust to drops or bumps.
</p>

<h3>Camera Calibration</h3>
<ul>
  <li><strong>Intrinsics:</strong> RealSense factory intrinsics are used for accuracy; checkerboard calibration was performed as a cross‑check.</li>
  <li><strong>Extrinsics (SolvePnP):</strong> AprilTags placed at 8 known world locations allow automatic camera‑to‑world calibration at startup, making the system robust to small camera bumps.</li>
  <li><strong>Pixel→World:</strong> Depth‑back‑projection (<code>K<sup>−1</sup></code>) recovers camera‑frame points; applying the extrinsic transform yields world poses. A planar homography rectifies to a top‑down view for easier reasoning and grid overlays.</li>
</ul>

<figure>
  <img src="/assets/images/armlab/grid_calibration.png" alt="Calibrated Camera View with Grid Overlaid." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Calibrated Camera View with Grid Overlaid</figcaption>
</figure>

<h3>Block Detection</h3>
<p class="text">
Blocks are segmented by color: HSV ranges for blue/green/yellow/orange/violet, and YCrCb thresholds for red (more robust to lighting drift). Post‑processing uses morphological filtering, contour extraction, and <code>minAreaRect</code> to return centroid, size, and rotation. I gate detections by area and mask out the arm’s workspace buffer to reduce false positives. Error rises at the far edge of the board (larger depth variance), so the planner favors nearer picks first when possible.
</p>

<figure>
  <img src="/assets/images/armlab/color_detection.png" alt="Output of Block Detection." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Output of Block Detections</figcaption>
</figure>

<h3>Forward Kinematics</h3>
<p class="text">
Modeled the 5‑DOF RX200 using the Modified Denavit–Hartenberg convention, deriving transforms from base to end‑effector. Validation involved driving to test poses and comparing FK‑predicted end‑effector coordinates against measured joint angles; the results matched closely, confirming the geometric model.

<figure>
  <img src="/assets/images/armlab/arm-schematic-frames.png" alt="Robot with DH Frames, Rotations, and Measurements." style="max-width: 70%; display: block; margin: 0 auto;"/>
  <figcaption>Robot with DH Frames, Rotations, and Measurements</figcaption>
</figure>

<figure>
  <img src="/assets/images/armlab/arm-dh.png" alt="Table of DH Parameters." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Table of DH Parameters</figcaption>
</figure>

</p>

<h3>Inverse Kinematics</h3>
<ul>
  <li><strong>Algebraic IK (primary):</strong> Closed‑form relations from the end‑effector transform yield joint solutions quickly, with explicit handling of multiple branches (base ±180° and two 3R elbow configurations) and joint‑limit filtering.</li>
  <li><strong>Least‑Squares IK (fallback):</strong> A numerical solver minimizes position error <em>p</em><sub>FK</sub>(θ) → <em>p</em><sub>target</sub> subject to joint bounds. Average positional error ≈ 0.5 cm over 10 trials; runtime ≈ 25 ms vs ≈ 0.13 ms for algebraic IK. I use it only when closed‑form hits limits or singularities.</li>
  <li><strong>Singularities:</strong> I detect and avoid wrist‑aligned cases (e.g., gripper over the base with loss of a rotational DOF) by preferring alternative branches or slightly offset targets.</li>
</ul>

<figure>
  <img src="/assets/images/armlab/arm_IKresult.png" alt="Inverse Kinematics Result Plot for Input Coordinates: x =
−100, y = 125, z = 60." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Inverse Kinematics Result Plot for Input Coordinates: x =
−100, y = 125, z = 60</figcaption>
</figure>

<h3>Teach &amp; Play</h3>
<p class="text">
<em>Teach‑and‑repeat</em> baseline built by manually guiding the arm through a full block‑swap cycle, recording joint angles via ROS 2 bags, and replaying them as a repeatable program. This produced smooth trajectories and served as a fallback behavior while perception matured.
</p>

<h3>Results</h3>
<ul>
  <li><strong>Teach‑and‑Repeat:</strong> Completed 10 continuous swap cycles with smooth, repeatable joint traces.</li>
  <li><strong>IK Accuracy:</strong> Algebraic IK round‑trip (FK→IK) recovered original joint angles nearly identically; LS‑IK achieved ≈ 0.5 cm mean position error over 10 trials.</li>
  <li><strong>Speed:</strong> Algebraic IK ≈ 0.00013 s/solve; LS‑IK ≈ 0.025 s/solve.</li>
</ul>

<figure>
  <img src="/assets/images/armlab/block_stack.jpg" alt="Robot Completing for Highest Block Stack" style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Robot Completing for Highest Block Stack</figcaption>
</figure>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
figure { margin: 1rem 0; display: flex; flex-direction: column; align-items: center; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; margin-top: 0.5rem; text-align: center;}
ul { margin-left: 1rem; }
hr { margin: 1.25rem 0; }
</style>
