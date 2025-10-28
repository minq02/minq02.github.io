---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/bigant/bigant.png
classes: wide
---
<h2 class="title">Vision‑Guided Robot Arm Control with Intel RealSense</h2>

<p class="text">
This project turns the Interbotix ReactorX‑200 into a self‑reliant pick–place system. A calibrated RealSense depth camera estimates block poses; a dual‑path inverse kinematics (IK) stack computes feasible grasps; and a ROS 2 execution loop carries out smooth, repeatable motion. The design goal wasn’t merely “move once,” but <em>operate continuously</em> under ordinary lab messiness—lighting changes, minor camera bumps, and imperfect block placement—without human corrections.
</p>

<h3>Video Demo</h3>
<iframe src="https://youtu.be/iK9BaI25W5I" width="100%" height="420" frameborder="0" allowfullscreen></iframe>

<figure>
  <img src="/assets/images/armlab-setup.jpg" alt="System setup: ReactorX‑200, RealSense, and block field." />
  <figcaption>System setup: RX200 and block field.</figcaption>
</figure>

<hr/>

<h3>System Overview</h3>
<p class="text">
The software stack has three layers: (1) <strong>Perception</strong> (color segmentation + depth, AprilTag‑based extrinsic calibration, homography for a bird’s‑eye view), (2) <strong>Planning</strong> (algebraic IK for speed and a least‑squares IK fallback within joint limits), and (3) <strong>Execution</strong> (teach‑and‑repeat primitives plus guarded motion, all in ROS 2). The arm publishes/consumes standard messages (<code>/rx200/joint_states</code>, <code>/camera/*</code>, <code>vision_msgs</code>) and re‑checks detections after every sequence to stay robust to drops or bumps.
</p>

<h3>Teach &amp; Play</h3>
<p class="text">
We built a <em>teach‑and‑repeat</em> baseline by manually guiding the arm through a full block‑swap cycle, recording joint angles via ROS 2 bags, and replaying them as a repeatable program. This produced smooth trajectories and served as a fallback behavior while perception matured.
</p>

<h3>Forward Kinematics</h3>
<p class="text">
We modeled the 5‑DOF RX200 using the Modified Denavit–Hartenberg convention, deriving transforms from base to end‑effector. Validation involved driving to test poses and comparing FK‑predicted end‑effector coordinates against measured joint angles; the results matched closely, confirming the geometric model.
</p>

<h3>Inverse Kinematics</h3>
<ul>
  <li><strong>Algebraic IK (primary):</strong> Closed‑form relations from the end‑effector transform yield joint solutions quickly, with explicit handling of multiple branches (base ±180° and two 3R elbow configurations) and joint‑limit filtering.</li>
  <li><strong>Least‑Squares IK (fallback):</strong> A numerical solver minimizes position error <em>p</em><sub>FK</sub>(θ) → <em>p</em><sub>target</sub> subject to joint bounds. Average positional error ≈ 0.5 cm over 10 trials; runtime ≈ 25 ms vs ≈ 0.13 ms for algebraic IK. We use it only when closed‑form hits limits or singularities.</li>
  <li><strong>Singularities:</strong> We detect and avoid wrist‑aligned cases (e.g., gripper over the base with loss of a rotational DOF) by preferring alternative branches or slightly offset targets.</li>
</ul>

<h3>Camera Calibration</h3>
<ul>
  <li><strong>Intrinsics:</strong> RealSense factory intrinsics are used for accuracy; checkerboard calibration was performed as a cross‑check.</li>
  <li><strong>Extrinsics (SolvePnP):</strong> AprilTags placed at 8 known world locations allow automatic camera‑to‑world calibration at startup, making the system robust to small camera bumps.</li>
  <li><strong>Pixel→World:</strong> Depth‑back‑projection (<code>K<sup>−1</sup></code>) recovers camera‑frame points; applying the extrinsic transform yields world poses. A planar homography rectifies to a top‑down view for easier reasoning and grid overlays.</li>
</ul>

<h3>Block Detection</h3>
<p class="text">
Blocks are segmented by color: HSV ranges for blue/green/yellow/orange/violet, and YCrCb thresholds for red (more robust to lighting drift). Post‑processing uses morphological filtering, contour extraction, and <code>minAreaRect</code> to return centroid, size, and rotation. We gate detections by area and mask out the arm’s workspace buffer to reduce false positives. Error rises at the far edge of the board (larger depth variance), so the planner favors nearer picks first when possible.
</p>

<h3>Results</h3>
<ul>
  <li><strong>Teach‑and‑Repeat:</strong> Completed 10 continuous swap cycles with smooth, repeatable joint traces.</li>
  <li><strong>IK Accuracy:</strong> Algebraic IK round‑trip (FK→IK) recovered original joint angles nearly identically; LS‑IK achieved ≈ 0.5 cm mean position error over 10 trials.</li>
  <li><strong>Speed:</strong> Algebraic IK ≈ 0.00013 s/solve; LS‑IK ≈ 0.025 s/solve.</li>
</ul>

<h3>Competition Tasks</h3>
<ul>
  <li><strong>Line ’em up!:</strong> Detect, sort by size/color, and line blocks precisely; re‑checks correct mis‑placements.</li>
  <li><strong>Sort ’n Stack:</strong> Reuses the sorting logic, then builds stacks in color order. Next iteration would interleave sorting and stacking for speed.</li>
  <li><strong>To the Sky!:</strong> Stacked 8 large blocks using depth‑aware z‑placement. Limited by top‑grasp‑only IK; side grasps would extend the max height.</li>
  <li><strong>Free Throw (Design):</strong> A catapult‑style launcher driven by the arm, with adjustable band tension and launch angle; planned servo actuation for auto‑aim via vision.</li>
</ul>

<h3>Lessons &amp; Next Steps</h3>
<ul>
  <li>Promote richer grasp sets (side/top) and orientation targets in IK to expand dexterous workspace.</li>
  <li>Add simple visual servoing near contact to cut residual placement error.</li>
  <li>Fuse temporal detections (track‑and‑confirm) to down‑weight spurious color blobs in tricky lighting.</li>
</ul>

<h3>Tech Stack</h3>
<p class="text">
ROS 2 (rclpy), OpenCV (HSV/YCrCb segmentation, <code>solvePnP</code>, <code>findHomography</code>), AprilTag detection, RealSense depth, custom algebraic + least‑squares IK, RViz/rosbag tooling.
</p>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
figure { margin: 1rem 0; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; }
ul { margin-left: 1rem; }
hr { margin: 1.25rem 0; }
</style>