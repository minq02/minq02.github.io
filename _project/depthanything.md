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
  <p>Currently working on..</p>
  <ul style="margin: 0.5rem 0 0 1rem;">
    <li>Benchmarking against other monocular depth models (e.g., Apple Depth Pro, ZoeDepth, LeReS) on VRX/field clips</li>
    <li>Log-linear inverse-depth calibration with robust least squares (Huber/Tukey) to handle outliers/specular water</li>
    <li>Auto-calibration: collect LiDAR↔image correspondences and fit parameters via maximum-likelihood (MLE)</li>
  </ul>
</div>

<h2 class="title">Monocular Depth Estimation on the WAM-V</h2>

<p class="text">
This project benchmarks a calibrated monocular depth pipeline (<a href="https://github.com/DepthAnything/Depth-Anything-V2">Depth Anything V2</a>) against LiDAR on the WAM-V platform, both in VRX (<a href="https://github.com/osrf/vrx">Virtual RobotX</a>) Gazebo-based simulation and in field footage. The aim is to understand where monocular depth can replace or complement LiDAR for near-term navigation and docking, and to quantify how much calibration reduces scale/bias errors.
</p>

<h3>Introducing WAM-V & VRX</h3>

<figure>
  <img src="/assets/images/depthany/vrx.png" alt="WAM-V in VRX Simulation." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>WAM-V in VRX Simulation</figcaption>
</figure>

<p class="text">
<strong>WAM-V</strong> (Wave Adaptive Modular Vessel) is a lightweight, catamaran-style ASV with modular mounting for sensors and compute. Its compliant pontoons filter wave energy, which helps keep sensors usable in choppy conditions while still reflecting realistic marine dynamics. <br><br>

<strong>VRX</strong> (Virtual RobotX) is a Gazebo-based maritime simulator that mirrors the real RobotX challenges: navigation around buoys, station-keeping, docking, and signage interpretation. It provides controllable wind/wave fields, ground-truth trajectories, and standardized tasks so we can iterate in sim, benchmark perception, then transfer to field runs.
</p>

<h3>Depth Anything V2</h3>
<p>
<strong>Depth Anything V2</strong> is a foundation model that predicts dense relative depth from a single RGB image. It’s trained across diverse scenes, which gives strong zero-shot performance and good boundary preservation—handy when the world is mostly water, rails, docks, and trailers. The catch: outputs aren’t in meters, and reflective water can confuse local patches.<br><br>

Monocular models predict relative structure, not absolute distance. The raw output has an arbitrary dynamic range and differs per scene. A simple linear normalization (e.g., scaling between the 1st–99th percentile and mapping to [0,1]) helps visualization but does not recover true meters. Consequences:
</p>
<ul>
  <li>Far ranges get compressed; near objects appear stretched.</li>
  <li>Scale drifts between scenes, breaking consistency across runs.</li>
</ul>

<h3>VRX Visualization (Relative Depth)</h3>

<figure>
  <img src="/assets/images/depthany/relative.gif"
       alt="WAM-V in VRX Simulation."
       style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Relative Depth Visualization of Depth Anything V2</figcaption>
</figure>

<p class="text">
This clip shows Depth Anything V2 running on the WAM-V camera feed in the VRX simulator. The model’s output is visualized as a normalized color map to highlight relative structure: nearer regions appear “hotter,” farther regions “cooler.” The normalization is percentile-based (e.g., 2–98%) on each frame so the full colormap stays informative even as the scene changes. This is great for seeing edges (dock face, rails, buoys) and for confirming that the network preserves object boundaries on water.
</p>

<h3>VRX Projection — Linear Metric Mapping</h3>
<p>
After visualizing relative depth, I applied a simple <em>linear</em> mapping to convert the model’s output to a nominal metric scale and back-projected each pixel (RGB + depth) into 3D to publish a colored point cloud in RViz.<br><br>
<strong>Model output:</strong> relative depth <em>r</em> in arbitrary units (higher values → closer).<br>
<strong>Linear mapping (attempt):</strong> map <em>r ∈ [0,1]</em> to metric depth <em>Z ∈ [z<sub>min</sub>, z<sub>max</sub>]</em> using<br>
<span style="white-space:nowrap;">Z = z<sub>min</sub> + (1 − r) · (z<sub>max</sub> − z<sub>min</sub>)</span><br><br>
</p>

<figure>
  <img src="/assets/images/depthany/linear.gif"
       alt="WAM-V in VRX Simulation."
       style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Depth Anything Projection with Linear Metric Mapping (LiDAR in red)</figcaption>
</figure>

<p>
This produces a dense, colored point cloud that looks locally plausible, but it exposes the limits of linear scaling.
</p>
<ul>
  <li><strong>Scale mismatch across scenes:</strong> the same object lands at different depths depending on scene content.</li>
  <li><strong>Far-range compression:</strong> distant structures bunch into thick, smeared planes.</li>
  <li><strong>Near-range stretch:</strong> close objects expand in depth, inflating clearances.</li>
</ul>
<p>
<strong>Root cause:</strong> the model’s relative depth is <em>not</em> linearly related to true metric distance (it behaves more like inverse depth). A linear map cannot correct this, motivating a calibrated non-linear mapping (e.g., reciprocal or monotone spline) using a few LiDAR-referenced anchors.
</p>

<h3>Calibration — Non-Linear Metric Mapping</h3>
<p>
To recover <em>metric</em> depth, I calibrated the model’s relative depth against LiDAR. I projected LiDAR into the camera optical frame and sampled the depth at a small region of interest (ROI). At the same ROI, I read the model’s relative depth <em>r</em>. With matched pairs (r, z), I fit a simple inverse-depth curve.
</p>

<p>
<strong>Calibration setup:</strong><br>
• Fixed camera pose; reference targets in the 5–10&nbsp;m range.<br>
• Extracted relative depth at the same pixel locations from the model output.<br>
• Extracted metric depth from LiDAR projected into the camera frame.
</p>

<figure>
  <img src="/assets/images/depthany/calibration.png" alt="Calibration Setup." style="max-width: 100%; display: block; margin: 0 auto;" />
  <figcaption>Relative Depth compared against LiDAR Depth</figcaption>
</figure>

<p>
<strong>Fitted model:</strong><br>
<span style="white-space:nowrap;">Z(r) = 904.4&nbsp;/&nbsp;r + 1.62</span><br>
This non-linear mapping captures the inverse-depth behavior, reduces far-range compression, and stabilizes scale across scenes.
</p>

<ul>
  <li><strong>Non-linear relation captured:</strong> relative depth is not linearly proportional to meters.</li>
  <li><strong>Inverse-depth behavior:</strong> far distances expand appropriately; near distances don’t over-stretch.</li>
  <li><strong>Improved geometry:</strong> dock planes thicken less; ranges align more closely with LiDAR.</li>
</ul>

<h3>Results — Post-Calibration</h3>

<figure>
  <img src="/assets/images/depthany/nonlinear.gif"
       alt="WAM-V in VRX Simulation."
       style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Depth Anything Projection with Calibrated Metric Mapping (LiDAR in red)</figcaption>
</figure>

<figure>
  <img src="/assets/images/depthany/far.gif"
       alt="WAM-V in VRX Simulation."
       style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Far Range Demonstration with Increased Pointcloud Size</figcaption>
</figure>

<p>
After applying the non-linear calibration, the metric depth looks substantially better. In RViz overlays, LiDAR points align closely with the calibrated Depth Anything V2 depth at corresponding pixels, and the water/ground plane appears much flatter—evidence that the scale is now well-behaved in meters.
</p>

<ul>
  <li><strong>LiDAR agreement:</strong> Depth at sampled regions matches LiDAR within small residuals; edges (dock face, rails) line up cleanly.</li>
  <li><strong>Flat, stable plane:</strong> The water/ground plane no longer “bows” or thickens with distance, indicating correct metric scaling.</li>
  <li><strong>Far-range fidelity:</strong> Increasing the pixel stride (larger ROI sampling) reveals distant structures—like shoreline trees—projected with coherent depth, even at long range.</li>
  <li><strong>Denser, useful maps:</strong> The colored point cloud is now metrically meaningful for planning near docks and trailers, with improved consistency across scenes.</li>
</ul>

<h3>Next Steps</h3>
<p>
I am planning on benchmarking against other mono depth estimation models and making calibration more robust/automatic.
</p>
<ul>
  <li><strong>Model benchmarking:</strong> Compare calibrated Depth Anything V2 against other monocular models (e.g., Apple Depth Pro, ZoeDepth, LeReS) on identical VRX and field clips.</li>
  <li><strong>Calibration model upgrade:</strong> Fit a log-linear inverse-depth mapping with robust least squares (Huber/Tukey) to handle outliers and specular water patches.</li>
  <li><strong>Auto-calibration (MLE):</strong> Automatically gather LiDAR↔image correspondences, then maximize likelihood to estimate scale parameters (and per-scene bias) without manual targets.</li>
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
