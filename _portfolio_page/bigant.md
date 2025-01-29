---
title: "Staircase Detection for Hexapod Robot Using Visual Processing"
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/bigant/bigant.png
classes: wide
---

## Project Overview* 

![apriltag-visualization]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/bigant_whole.png)

This project focuses on detecting staircases using computer vision techniques, enabling accurate analysis for robotic navigation. The process includes:  

1. Background Extraction – Removes moving elements using median blending to isolate the staircase.  
2. Homography Transformation – Uses AprilTag detections to correct perspective and generate a rectified top-down view.  
3. Masking – Filters out unnecessary regions to focus on the staircase.  
4. Radon Transform – Identifies stair edges by detecting linear structures in the image.  
5. Peak Detection – Extracts step locations by analyzing intensity profiles.  
6. Staircase Detection – Overlays detected edges onto the image for a clean, structured staircase view.  

This method ensures precise stair edge identification, supporting gait analysis and robotic motion planning.

---

## Part I: Background Extraction

This project ...

## Part II: Homography Transformation

This project ...

## Part III: Masking

This project ...

## Part IV: Radon Transform

This project ...

## Part V: Peak Detection

This project ...

## Part VI: Staircase Detection

This project ...
