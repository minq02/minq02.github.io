---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/botlab/botlab.gif
classes: wide
---
<div class="project-update">
  <strong>Last Project Update: 10/16/2025</strong>
  <p>To-Do List: <br>○ Benchmark against different mono depth models<br>○ Log-linear model with inverse depth w/ robust least squares for outliers</p>
</div>

<h2 class="title">Monocular Depth Estimation on the WAM-V</h2>

<p class="text">
This project builds an object-aware perception and mapping stack for an Autonomous Surface Vehicle (ASV) using the WAM-V platform and the VRX simulation environment. We fuse camera, LiDAR, IMU, and GPS to create an object-level map and reliable short-horizon geometry for tasks like waypoint following and trailer docking. A monocular depth model (Depth Anything V2) provides dense relative depth from a single RGB stream; we then calibrate it to metric scale and feed it to our SLAM back-end and local planner.
</p>

<h3>Introduction - WAM-V & VRX</h3>
<p class="text">
<strong>WAM-V</strong> (Wave Adaptive Modular Vessel) is a lightweight, catamaran-style ASV with modular mounting for sensors and compute. Its compliant pontoons filter wave energy, which helps keep sensors usable in choppy conditions while still reflecting realistic marine dynamics. <br><br>

<strong>VRX (Virtual RobotX)</strong> is a Gazebo-based maritime simulator that mirrors the real RobotX challenges: navigation around buoys, station-keeping, docking, and signage interpretation. It provides controllable wind/wave fields, ground-truth trajectories, and standardized tasks so we can iterate in sim, benchmark perception, then transfer to field runs.
</p>

<h3>Introduction - Depth Anything V2</h3>
<p>
<strong>Depth Anything V2</strong> is a foundation model that predicts dense relative depth from a single RGB image. It’s trained across diverse scenes, which gives strong zero-shot performance and good boundary preservation—handy when the world is mostly water, rails, docks, and trailers. The catch: outputs aren’t in meters, and reflective water can confuse local patches.<br><br>

Monocular models predict relative structure, not absolute distance. The raw output has an arbitrary dynamic range and differs per scene. A simple linear normalization (e.g., scaling between the 1st–99th percentile and mapping to [0,1]) helps visualization but does not recover true meters. Consequences:
</p>
<ul>
  <li>Far ranges get compressed; near objects appear stretched.</li>
  <li>Scale drifts between scenes, breaking consistency across runs.</li>
<ul>

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
