---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/botlab/botlab.gif
classes: wide
---
<h2 class="title">Mobile Robot Autonomous Navigation: SLAM & A*</h2>

<p class="text">
We took an MBot (Classic) from bare hardware to closed-loop autonomous navigation. The stack includes wheel-speed feed-forward with PID, gyrodometry, occupancy-grid mapping from 2D LiDAR, Monte Carlo Localization (MCL), lightweight SLAM, A* planning, and frontier exploration—enough to build a map, localize within it, and reach goals through clutter.
</p>

<h3>Video Demo</h3>
<iframe
    width="100%"
    height="50%"
    src="https://www.youtube.com/embed/7jnVit4HQhw"
    frameborder="0"
    allow="autoplay; encrypted-media"
    allowfullscreen
></iframe>

<h3>System Overview</h3>
<p class="text">
Three layers keep the robot honest under sensor noise and wheel slip:
<br>(1) <strong>Control</strong> — feed-forward from wheel calibration + PID feedback; motion model uses Rotate–Translate–Rotate (RTR)
<br>(2) <strong>Mapping &amp; Localization</strong> — log-odds occupancy grid + particle-filter (MCL) with LiDAR likelihoods
<br>(3) <strong>Planning</strong> — 8-connected A* on an inflated costmap, plus simple frontier exploration for unknown space
</p>

<h3>Hardware</h3>
<figure>
  <img src="/assets/images/botlab/mbot.jpg" alt="MBot Classic." style="max-width: 50%; display: block; margin: 0 auto;" />
</figure>
<ul>
  <li>MBot (Classic), differential drive</li>
  <li>Jetson Nano controller</li>
  <li>Wheel encoders (per-wheel)</li>
  <li>2D LiDAR + 3-axis IMU</li>
  <li>Battery + power/regulation board</li>
</ul>

<h3>Calibration &amp; Low-Level Control</h3>
<p class="text">
We measured wheel speed vs PWM to derive a linear feed-forward model (<code>PWM = m·v + b</code>) and wrapped it with a per-wheel PID for accurate tracking. RTR motion (spin→drive→spin) reduces heading drift compared to naive differential drive. This layer exposes “go-to” primitives for the planner.
</p>

<figure>
  <img src="/assets/images/botlab/path.png" alt="MBot Classic." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>The robot’s dead reckoning estimated pose when driving around a 1 meter square four times</figcaption>
</figure>

<figure>
  <img src="/assets/images/botlab/lav.png" alt="MBot Classic." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>The robot’s linear and rotational velocity as it drives around a 1 meter square four times..</figcaption>
</figure>

<p class="text">
We validated odometry with a 1 m square dead-reckoning test: each edge runs RTR (rotate→translate 1 m→rotate) while logging encoder ticks, gyro yaw, and wheel speeds at 50–100 Hz. Pure encoder odom drifts in heading; fusing the gyro tightens the loop so the path closes near the start with small, repeatable error. The velocity trace shows clean ramp/cruise/ramp; tuned PID prevents bowing or spiraling, making the “go-to” primitive metrically reliable for A*/MCL.
</p>

<h3>Mapping (Occupancy Grid)</h3>
<p class="text">
LiDAR scans are integrated into a log-odds occupancy grid using Bresenham raycasting to mark free vs occupied space. We inflate obstacles to account for the robot footprint and planning safety margins.
</p>

<h3>Localization (MCL)</h3>
<p class="text">
A particle filter tracks pose on the grid. The RTR odometry drives the motion update; the LiDAR grid-correlation likelihood gives the measurement update. We resample by effective sample size to avoid particle deprivation.
</p>

<figure>
  <img src="/assets/images/botlab/drive-maze.png" alt="Occupancy grid map" style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Built occupancy grid; darker cells are higher occupancy probability.</figcaption>
</figure>

<h3>SLAM</h3>
<p class="text">
Combining the mapper with MCL yields a lightweight RBPF-style SLAM loop: odometry proposes; the map and LiDAR likelihood correct; the grid evolves as the pose graph stabilizes. This reduces drift and tightens alignment over time.
</p>

<figure>
  <img src="/assets/images/botlab/slam_diagram.png" alt="SLAM block diagram" style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>SLAM Block Diagram</figcaption>
</figure>

<h3>Planning &amp; Exploration</h3>
<p class="text">
A* plans on an 8-connected lattice with an admissible diagonal heuristic. Unknowns carry neutral costs; inflated obstacles discourage risky shortcuts. Frontier exploration (free cells adjacent to unknown) gives a fast baseline for autonomous coverage.
</p>

<figure>
  <img src="/assets/images/botlab/astar.png" alt="Planned vs driven path on the grid" style="max-width: 60%; display: block; margin: 0 auto;" />
  <figcaption>Planned astar path (green) of our robot along with the actual driven path (blue) and the recorded path using odometry (yellow)</figcaption>
</figure>

<h3>Results</h3>
<ul>
  <li><strong>Control:</strong> Stable wheel-speed tracking and clean turns with RTR.</li>
  <li><strong>Mapping/SLAM:</strong> Maps align with maze geometry; localization error drops as particles converge.</li>
  <li><strong>Planning:</strong> Reliable goal reaching in open and maze-like layouts, with predictable runtime.</li>
</ul>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
figure { margin: 1rem 0; display: flex; flex-direction: column; align-items: center; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; margin-top: 0.5rem; text-align: center; }
ul { margin-left: 1rem; }
hr { margin: 1.25rem 0; }
</style>
