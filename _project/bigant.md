---
title: ""
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/bigant/bigant.png
classes: wide
---
<h2 class="title">Staircase Detection Using Image Processing</h2>

<p class="text">
Hexapod robots offer unique mobility advantages, particularly in navigating complex terrains like staircases. However, analyzing their gait in real-world conditions requires precise tracking of movement. This project processes a video of a hexapod robot climbing stairs to detect staircase edges from the video feed, enabling step-by-step analysis without relying on onboard sensors. AprilTags on both the staircase and the robot provide stable reference points for perspective correction and spatial alignment, forming the basis for background extraction, homography transformation, and edge/line detection.
</p>

<figure>
  <img src="/assets/images/bigant/bigant_whole.png" alt="Hexapod Robot and Staircase." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Hexapod Robot and Staircase with AprilTags</figcaption>
</figure>

<h3>Background Extraction</h3>
<p class="text">
The robot is an unwanted moving element when detecting staircase edges. Two approaches were compared: frame averaging (mean across frames) and median blending (per-pixel median across frames). Frame averaging left faint “ghosts” of the robot; median blending removed dynamic elements while preserving the staircase, so it was adopted for a clean background.
</p>

<figure>
  <img src="/assets/images/bigant/background.png" alt="Background via Median Blending." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Median-Blend Background: Robot Removed, Staircase Preserved</figcaption>
</figure>

<h3>Homography Transformation</h3>
<p class="text">
To analyze the staircase structure, perspective distortion must be corrected. Using detected AprilTags as correspondences, a homography warps the frame into a rectified, top-down view. This alignment ensures step edges are horizontal and comparable across frames.
</p>

<figure>
  <img src="/assets/images/bigant/homography.png" alt="Top-Down Rectification via Homography." style="max-width: 60%; display: block; margin: 0 auto;" />
  <figcaption>Top-Down Rectification Using AprilTag-Based Homography</figcaption>
</figure>

<h3>Masking</h3>
<p class="text">
After rectification, masks remove irrelevant regions (surrounding environment), leaving only the staircase. This focuses computation where it matters and improves the reliability of downstream edge and line detection.
</p>

<figure>
  <img src="/assets/images/bigant/masking.png" alt="Masked Rectified View." style="max-width: 50%; display: block; margin: 0 auto;" />
  <figcaption>Masked Rectified View of the Staircase</figcaption>
</figure>

<h3>Radon Transform</h3>
<p class="text">
Stair steps form a set of near-parallel lines. The Radon Transform projects image intensity along many angles to highlight linear structure; the resulting sinogram exposes dominant lines as high-intensity ridges. Here, peaks concentrate near 90°, consistent with horizontal step edges in the rectified view.
</p>

<figure>
  <img src="/assets/images/bigant/radon.png" alt="Radon Transform Sinogram." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Radon Transform Sinogram with Prominent Responses Near 90°</figcaption>
</figure>

<h3>Peak Detection</h3>
<p class="text">
Peak-finding on Radon angle profiles recovers the precise locations of individual step edges. The detected peaks at 90° align with staircase treads, confirming correct rectification and robust line extraction.
</p>

<figure>
  <img src="/assets/images/bigant/peak.png" alt="Peak Detection on Radon Profiles." style="max-width: 70%; display: block; margin: 0 auto;" />
  <figcaption>Peak Detection Identifies Individual Step Edges</figcaption>
</figure>

<h3>Staircase Detection (Overlay)</h3>
<p class="text">
Detected edges are overlaid on the rectified image to produce a clean, structured visualization of the staircase geometry. This output serves as the basis for gait analysis, allowing measurement of the robot’s foot placements relative to the detected step boundaries.
</p>

<figure>
  <img src="/assets/images/bigant/result.png" alt="Final Staircase Edge Overlay." style="max-width: 50%; display: block; margin: 0 auto;" />
  <figcaption>Final Overlay of Detected Stair Edges on Rectified View</figcaption>
</figure>

<h3>Results</h3>
<p class="text">
By combining median-blend background extraction, AprilTag-guided homography, and Radon-based line detection with peak picking, this pipeline enables robust, sensor-free step tracking directly from video. The approach generalizes to other terrain recognition tasks where simple, dependable geometry beats heavyweight modeling. Code is part of an ongoing research effort; feel free to reach out with questions.
</p>

<style>
.title { font-size: 1.1rem; margin-bottom: 0.5rem; }
.text { font-size: 0.95rem; line-height: 1.6; }
figure { margin: 1rem 0; display: flex; flex-direction: column; align-items: center; }
figure img { width: 100%; height: auto; border-radius: 12px; }
figcaption { font-size: 0.8rem; color: #666; margin-top: 0.5rem; text-align: center;}
ul { margin-left: 1rem; }
hr { margin: 1.25rem 0; }
</style>