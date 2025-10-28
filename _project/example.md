---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/botlab/botlab.gif
classes: wide
---
<!-- <div class="project-update">
  <strong>[09/25/2025] Project Update</strong>
  <p>Currently expanding the frontier exploration planner to handle cluttered lab layouts and logging new datasets for online SLAM tuning.</p>
</div> -->

<h2 class="title">Mobile Robot Autonomous Navigation: SLAM & A*</h2>

<p class="text">
This project included building, calibrating, and programming a differential‑drive <strong>MBot</strong> (Jetson Nano, magnetic wheel encoders, 2D LiDAR, and 3‑axis MEMS IMU) to go from <em>raw hardware</em> to <em>map‑aware autonomy</em>. The stack covers <strong>feed‑forward + PID wheel control</strong>, <strong>gyrodometry</strong>, <strong>occupancy‑grid mapping</strong>, <strong>Monte Carlo Localization (MCL)</strong>,
<strong>SLAM integration</strong>, <strong>A*</strong> path planning, and <strong>frontier exploration</strong>.
</p>

<h3>System Overview</h3>
<p class="text">
The pipeline breaks into Action (Control), Perception (Mapping &amp; Sensing), and Reasoning (Planning &amp; Exploration):
</p>
<ul>
  <li><strong>Action</strong>: PWM feed‑forward from wheel speed calibration + PID feedback. A high‑level RTR (Rotate–Translate–Rotate) motion controller executes waypoint motion.</li>
  <li><strong>Perception</strong>: LiDAR occupancy grid via Bresenham raytracing; IMU‑aided odometry (<em>gyrodometry</em>); AprilTag‑assisted routines for vision navigation experiments.</li>
  <li><strong>Reasoning</strong>: Particle‑filter MCL on the grid; A* search on an 8‑connected lattice; frontier detection (free‑adjacent‑to‑unknown) for autonomous exploration.</li>
</ul>

<h3>Platform &amp; Hardware</h3>
<div class="hardware-overview">
  <figure>
    <img src="/assets/images/botlab/mbot.jpg" alt="MBot Classic." style="max-width: 50%; display: block; margin: 0 auto;" />
  </figure>
  <ul>
    <li>MBot (Classic) differential‑drive base</li>
    <li>Jetson Nano + driver board</li>
    <li>Magnetic encoders (per wheel)</li>
    <li>2D LiDAR, 3‑axis MEMS IMU</li>
  </ul>
</div>

<h3>Control Stack</h3>
<p class="text">
Calibration, state estimation, and motion control share one loop so the robot can follow commanded paths while rejecting drift and bias.
</p>
<ul class="stack-list">
  <li><strong>Calibration:</strong> Determine motor and encoder polarities plus the commanded‑speed → PWM map (<code>PWM = m·v + b</code>). Multiple trials per wheel/direction provide feed‑forward terms and priors for tuning.</li>
  <li><strong>Gyrodometry:</strong> Fuse encoders with the IMU gyro so heading stays stable through slip; sanity checks show 1.0 m/s commands yield ≈ 0.53 m forward with minor lateral drift, and 3.14 rad/s spins reach ≈ 150°/s.</li>
  <li><strong>PID Velocity Control:</strong> Tighten wheel tracking around the feed‑forward setpoint with <code>Kp = 0.001</code>, <code>Kd = 0.001</code>, <code>Ki = 0.002</code>, plus low‑pass filtering and accel limits to prevent slip.</li>
  <li><strong>RTR Motion Layer:</strong> Execute waypoints via <em>Rotate→Translate→Rotate</em> segments, using modest heading gains (<code>Kp≈1.0</code>) to keep overshoot low and trajectories repeatable.</li>
</ul>

<h3>Mapping &amp; Localization</h3>
<p class="text">
The perception stack maintains a consistent map while localizing the robot against it, even as new space is explored.
</p>
<ul class="stack-list">
  <li><strong>Occupancy Grid Mapping:</strong> Ray‑cast LiDAR returns with Bresenham traversal into a log‑odds grid; hand‑carried maze runs validated carving of free space and obstacle updates.</li>
  <li><strong>Monte Carlo Localization:</strong> Estimate <code>(x, y, θ)</code> with a particle filter using RTR‑based motion noise (<code>k₁ = 0.005</code>, <code>k₂ = 0.0025</code>) and scan likelihoods; 100→1000 particles scale from ≈20→156 µs per update.</li>
  <li><strong>RBPF SLAM:</strong> Synchronize odometry, scan updates, and grid writes so the particle filter and map co-evolve; pose error drops steadily as the filter converges.</li>
</ul>

<h3>Planning &amp; Exploration</h3>
<p class="text">
High-level planners consume the live map and localization estimates to pick goals and navigate through unknown space.
</p>
<ul class="stack-list">
  <li><strong>A* Grid Planner:</strong> Search an 8-connected lattice with a diagonal-distance heuristic and inflation zones for safety; benchmarks across empty, convex, and maze-like worlds show predictable cost growth in tight passages.</li>
  <li><strong>Frontier Exploration:</strong> Detect free-adjacent-to-unknown cells, cluster them, pick a centroid goal, path with A*, and execute via RTR to close the loop from mapping to autonomous discovery.</li>
  <li><strong>Unknown Start Recovery:</strong> Seed particles across free space, let MCL converge with new scans, then lock a home reference once the filter stabilizes.</li>
</ul>

<h3>Results</h3>
<ul>
  <li><strong>Control</strong>: Wheel speeds track within a tight band after PID tuning; RTR paths are smooth with reduced overshoot.</li>
  <li><strong>Mapping/SLAM</strong>: Occupancy grids align well with hand‑measured layouts; odometry error trends down as MCL converges.</li>
  <li><strong>Planning</strong>: A* solves standard scenes quickly; worst‑case runtimes occur in tight corridors and mazes as expected.</li>
</ul>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
ul { margin-left: 1rem; }
figure { margin: 1rem 0; display: flex; flex-direction: column; align-items: center; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; margin-top: 0.5rem; text-align: center;}
.project-update { border: 1px solid #d0e6ff; border-left: 4px solid #3b82f6; background: #f5f9ff; padding: 0.85rem 1rem; border-radius: 10px; box-shadow: 0 4px 10px rgba(59, 130, 246, 0.08); margin: 1.25rem 0; }
.project-update strong { display: block; font-size: 0.85rem; letter-spacing: 0.03em; color: #1d4ed8; margin-bottom: 0.35rem; }
.project-update p { margin: 0; font-size: 0.9rem; color: #334155; }
.hardware-overview { display: flex; flex-direction: column; gap: 0.75rem; align-items: center; }
.hardware-overview ul { margin: 0; list-style: disc; padding-left: 1.25rem; }
.stack-list { margin: 0 0 1rem 0; padding-left: 1.1rem; }
.stack-list li { margin-bottom: 0.6rem; }
hr { margin: 1.25rem 0; }
</style>
