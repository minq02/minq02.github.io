---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/objectslam/gtsam.gif
classes: wide
---
<div class="project-update">
  <strong>Last Project Update: 11/6/2025</strong>
  <p>Currently working on:</p>
  <ul style="margin:0.5rem 0 0 1rem;">
    <li>Include orientation (from PCA) to GTSAM point landmarks</li>
    <li>Fine-tuning Semantic Segmentation Model for higher precision/recall</li>
  </ul>
</div>

<h2 class="title">Object-SLAM for 2D Semantic Mapping (Grounded-SAM&nbsp;2 + GTSAM)</h2>

<p class="text">
A 2D object-centric SLAM system: Grounded-SAM 2 produces semantic landmarks, and a GTSAM factor graph jointly optimizes robot pose (SE(2)) and landmark positions to yield an object-level BEV map and a smoothed trajectory.
</p>

<h3>Demo — Tracking + GTSAM Optimization</h3>
<figure>
  <img src="/assets/images/objectslam/demo.gif" alt="RViz: GTSAM-optimized pose and object map" style="max-width:90%; display:block; margin:0 auto; border-radius:12px;" />
  <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">
    RViz output: incremental GTSAM optimization (poses in SE(2)) with object landmarks.
  </figcaption>
</figure>

<p class="text">
End-to-end (perception → LiDAR-in-mask → tracking → GTSAM) yields a smoother trajectory and a coherent object map. Stable IDs persist across frames; landmarks tug the pose into global consistency.
</p>
<ul>
  <li><strong>Pose:</strong> drift reduced; turns tighten after optimization.</li>
  <li><strong>Landmarks:</strong> buoys posts cluster with reasonable covariance.</li>
</ul>

<h3>System Overview</h3>
<figure>
  <img src="/assets/images/objectslam/plan.png" alt="Pipeline: GPS+IMU→DRIFT→Global Pose; LiDAR/Stereo→Object Detector; Landmark layer → GTSAM → Optimized Trajectory + Object-level BEV." style="max-width: 90%; display: block; margin: 0 auto;" />
  <figcaption>Object-centric SLAM with a GTSAM back-end.</figcaption>
</figure>
<ul>
  <li><strong>GPS+IMU → DRIFT:</strong> supplies a global pose prior.</li>
  <li><strong>LiDAR / Stereo → Detector:</strong> Grounded-SAM 2 / open-vocab models output masks, boxes, labels.</li>
  <li><strong>Landmarks:</strong> class + pose/shape cues with uncertainty.</li>
  <li><strong>Back-end:</strong> GTSAM optimizes poses and landmarks into a coherent object map.</li>
</ul>

<h3>Why GTSAM?</h3>
<figure>
  <img src="/assets/images/objectslam/gtsam_demo.png" alt="GTSAM" style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Initial estimate is shown in green. The GTSAM optimized trajectory, with covariance ellipses, is shown in blue.</figcaption>
</figure>
<p class="text">
GTSAM frames SLAM as a factor graph—variables linked by probabilistic factors—and solves it with Gauss-Newton/LM or iSAM2 for incremental updates. It cleanly fuses odometry and object measurements with proper noise and robust losses.
</p>

<h3>Factors in This Project</h3>
<ul>
  <li><strong>PriorFactor&lt;Pose2&gt;:</strong> anchors the frame at t=0 (tight covariance).</li>
  <li><strong>BetweenFactor&lt;Pose2&gt;:</strong> odometry as (Δx, Δy, Δθ) with Σ<sub>odom</sub>.</li>
  <li><strong>Bearing/Range factors:</strong> <em>BearingRangeFactor&lt;Pose2,Point2&gt;</em> (θ, ρ), or bearing-only when range is weak; robust loss for mask outliers.</li>
</ul>
<p class="text">
Together: prior fixes gauge, odometry propagates motion, object factors enforce global consistency → cleaner trajectory and semantic BEV.
</p>

<h3>Perception — Open-Vocabulary Detection</h3>

<figure style="margin: 1rem 0; display: grid; grid-template-columns: 1fr 1fr; gap: 0.75rem; align-items: center;">
  <div style="text-align:center;">
    <img src="/assets/images/objectslam/yoloworld.gif" alt="YOLO-World detections" style="width:100%; border-radius:12px;" />
    <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">YOLO-World — fast, but many false positives.</figcaption>
  </div>
  <div style="text-align:center;">
    <img src="/assets/images/objectslam/gsam2.gif" alt="Grounded-SAM 2 detections" style="width:100%; border-radius:12px;" />
    <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">Grounded-SAM&nbsp;2 — better detection and segmentation.</figcaption>
  </div>
</figure>

<p class="text">
YOLO-World handled zero-shot prompts (buoy, dock post/face, cone, sign) but produced many false positives. Switching to <strong>Grounded-SAM 2</strong> (GroundingDINO prompts + SAM masks) improved precision and boundary quality, so we proceed with Grounded-SAM 2. I plan to fine-tune it for maritime scenes to raise precision/recall and calibrate confidence scores.
<br><br>
In the future, I’ll fine-tune Grounded-SAM&nbsp;2 to boost detection precision/recall and produce more calibrated confidence scores in maritime scenes.
</p>

<h3>Range from Segmentation — LiDAR Inside the Mask</h3>
<p class="text">
For each Grounded-SAM 2 mask, LiDAR is projected into the camera, and only in-mask points are pooled. Median range and centroid form a robust per-object measurement used by tracking and SLAM.
</p>

<figure>
  <img src="/assets/images/objectslam/perception_output.png" alt="Per-object pooled ranges from LiDAR points inside masks" style="max-width:70%; display:block; margin:0 auto; border-radius:12px;" />
  <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">
    Console print: class, 3D estimate (x, y, z in odom), and number of LiDAR points inside the mask.
  </figcaption>
</figure>

<p class="text">
For each Grounded-SAM 2 mask, LiDAR is projected into the camera, and only in-mask points are pooled. Median range and centroid form a robust per-object measurement used by tracking and SLAM.
</p>
<ul>
  <li><strong>Projection:</strong> LiDAR → camera → pixels; keep points inside the mask polygon.</li>
  <li><strong>Pooled stats:</strong> ρ = median(‖p‖), centroid ĉ = median({p<sub>i</sub>}), quality = {N, spread (MAD/IQR)}.</li>
  <li><strong>Output:</strong> {label, θ, ρ, centroid, Σ, N}; drop if N &lt; N<sub>min</sub> or spread too large.</li>
</ul>
<p class="text">
Masks give clean spatial support, so medians suppress water/glare outliers. The console summary (e.g., “greenbuoy … with 52 points”) reflects these pooled values.
</p>

<h3>Tracking Algorithm</h3>

<figure>
  <img src="/assets/images/objectslam/vs.png" alt="Mahalanobis vs Euclidean gating intuition" style="max-width: 80%; display:block; margin:0 auto; border-radius:12px;" />
  <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">
    Anisotropic uncertainty (range ≫ bearing) creates an <em>elliptical</em> acceptance region. Mahalanobis distance respects this; Euclidean does not.
  </figcaption>
</figure>

<p class="text">
Object detections are associated to existing tracks using Mahalanobis distance. Because range is noisier than bearing in our setup (LiDAR/camera-to-odom projection), the innovation covariance is elongated along range, so we gate with an ellipse rather than a circle.
</p>

<figure>
  <img src="/assets/images/objectslam/tracking.png" alt="Flow: predict → gate → create/update → track ID" style="max-width: 50%; display:block; margin:0.5rem auto 0; border-radius:12px;" />
  <figcaption style="font-size:0.8rem; color:#666; margin-top:0.35rem;">
    Pipeline: new measurement → predict → Mahalanobis gate → create or update → stable Track IDs.
  </figcaption>
</figure>

<p class="text">
Detections are matched to tracks with Mahalanobis gating. Because range is noisier than bearing, the innovation covariance is elongated along range, so the gate is an ellipse, not a circle.
</p>
<ul>
  <li><strong>Predict:</strong> x<sub>pred</sub>, P<sub>pred</sub></li>
  <li><strong>Innovate:</strong> y = z − h(x<sub>pred</sub>), S = H P<sub>pred</sub> Hᵀ + R</li>
  <li><strong>Gate:</strong> d² = yᵀ S⁻¹ y ≤ τ (χ² threshold)</li>
  <li><strong>Assign:</strong> nearest-neighbor (or Hungarian) within gate</li>
  <li><strong>Update:</strong> K = P<sub>pred</sub>HᵀS⁻¹; x = x<sub>pred</sub> + Ky; P = (I − KH)P<sub>pred</sub></li>
  <li><strong>Manage:</strong> create new tracks for ungated detections; age out unmatched tracks</li>
</ul>
<p class="text">
Takeaway: Mahalanobis gating respects the uncertainty geometry, reducing false matches and stabilizing IDs for the GTSAM factors.
</p>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
ul { margin-left: 1rem; }
figure { margin: 1rem 0; display: flex; flex-direction: column; align-items: center; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; margin-top: 0.5rem; text-align: center;}
.project-update { border: 1px solid #d0e6ff; border-left: 4px solid #3b82f6; background: #f5f9ff; padding: 0.85rem 1rem; border-radius: 10px; box-shadow: 0 4px 10px rgba(59,130,246,0.08); margin: 1.25rem 0; }
.project-update strong { display: block; font-size: 0.85rem; letter-spacing: 0.03em; color: #1d4ed8; margin-bottom: 0.35rem; }
.project-update p { margin: 0; font-size: 0.9rem; color: #334155; }
</style>
