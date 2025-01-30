---
title: "Staircase Detection for Hexapod Robot Using Visual Processing"
author_profile: true
toc: false
key: -2
header:
  teaser: /assets/images/bigant/bigant.png
classes: wide
---

Hexapod robots offer unique mobility advantages, particularly in navigating complex terrains like staircases. However, analyzing their gait in real-world conditions requires precise tracking of movement. This project focuses on processing a video of a hexapod robot climbing stairs using computer vision techniques. The objective is to detect staircase edges from the video feed, enabling step-by-step analysis of the robot's movement without relying on onboard sensors.

![bigant]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/bigant_whole.png)

To ensure accurate localization, AprilTags are strategically placed on both the staircase and the robot itself. These tags provide robust reference points for perspective correction and precise spatial alignment, forming the foundation for further processing techniques such as background extraction, homography transformation, and edge detection.

## Background Extraction

One of the primary challenges in analyzing a video feed is isolating static structures from moving objects. In this case, the robot itself is an unwanted moving element when detecting the staircase edges. Two common approaches for background extraction—frame averaging and median blending—were evaluated.

![background]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/background.png)

Frame averaging, which computes the mean of pixel intensities across multiple frames, retained faint "ghost" traces of the moving robot, making it unsuitable for clean background removal. Median blending, on the other hand, selects the median pixel value across frames, effectively eliminating the dynamic elements while preserving static components like the staircase structure. Given its superior performance, median blending was adopted as the primary method for background extraction, ensuring a clean and consistent representation of the staircase.

## Homography Transformation

To analyze the staircase structure effectively, perspective distortions in the video need to be corrected. Homography transformation, a technique that warps an image based on known correspondences, was applied using the detected AprilTags.

![homography]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/homography.png)

By mapping AprilTag coordinates from the video frames to a predefined reference plane, this transformation generated a rectified, top-down view of the staircase. This corrected perspective is crucial for subsequent processing steps, as it ensures that stair edges are aligned correctly and can be analyzed without skewing artifacts.

## Masking

![masking]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/masking.png)

Once the rectified view was obtained, masking techniques were employed to filter out unnecessary areas. The objective was to remove irrelevant regions such as the surrounding environment, leaving only the staircase and its structural features. This step significantly improved the accuracy of subsequent edge detection processes by focusing computations solely on the staircase.

## Radon Transform

The staircase consists of a series of parallel steps, making line detection a key component of the analysis. The Radon Transform, a mathematical technique commonly used for feature extraction in images, was applied to identify prominent linear structures.

![radon]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/radon.png)

By projecting the image intensities along multiple angles, the Radon Transform highlighted dominant edge features corresponding to the staircase steps. The resulting sinogram—a graphical representation of detected lines—served as the foundation for extracting step boundaries. From the sinogram, high-intensity points are observed around 90 degrees, indicating that the staircase edges are well-detected. This is expected, as the steps are horizontal and align with the 90-degree orientation in the transform.

## Peak Detection

![peak]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/peak.png)

With the staircase edges detected via the Radon Transform, peak detection algorithms were implemented to precisely identify the locations of individual steps. Intensity profiles were analyzed to extract prominent peaks corresponding to step edges, ensuring a structured and reliable detection method. At 90 degrees, the coordinates of the detected peaks must align with the staircase edges, confirming the accuracy of the step detection process.

## Staircase Detection

The final stage involved overlaying the detected edges onto the original rectified image, generating a structured, clean visualization of the staircase. This output serves as the groundwork for further gait analysis, allowing precise evaluation of the hexapod robot’s step positioning relative to the detected edges.

![result]({{ site.url }}{{ site.baseurl }}/assets/images/bigant/result.png)

By leveraging a combination of computer vision techniques—including background extraction, homography transformation, and line detection—this project enables robust, sensor-free step tracking based purely on video analysis. This work has implications for improving autonomous navigation in legged robots and can be extended to other scenarios requiring real-time terrain recognition.
