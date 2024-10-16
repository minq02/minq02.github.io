---
name: Soccer Tracking Analysis
tools: [Python, YOLOv8, Roboflow]
image: https://minq02.github.io/assets/2_soccer_tracking.gif
description: Fine-tuned YOLOv8 for soccer player and ball detection, applied HSV color filtering for team identification and ball possession tracking
---

# Soccer Tracking Analysis
<br>

### Overview
The project involved fine-tuning the YOLOv8 model with approximately 2,000 custom-labeled images to detect soccer players and the ball, followed by applying HSV color filtering to analyze ball possession.

<br>

### Key Contributions
- Fine-tuned YOLOv8 with a custom-labeled dataset for detecting soccer players and the ball.
- Used ~2,000 labeled images to enhance the accuracy of the object detection model.
- Applied HSV color filtering to distinguish teams by uniform colors, allowing ball possession tracking.
- Implemented data augmentation techniques (grayscale, hue, saturation, cutout) to improve model robustness.

<br>

### Data Collection and Processing
- **Object Detection Model**: Built using Roboflow and YOLOv8, the model detects soccer players and the ball in real-game footage.
    <br>
    <img src="{{ site.url }}{{ site.baseurl }}/assets/2_raw_detection.gif" style="height: 395px; width:700px;"/>
    <br>
- **Labeling and Augmentation**: Augmented the dataset with grayscale, hue, and cutout transformations to improve detection accuracy.
    <br>
    <img src="{{ site.url }}{{ site.baseurl }}/assets/2_image_sets.gif" style="height: 342px; width:700px;"/>
    <br>

<br>

### Ball Possession Analysis
- Used HSV color masking to distinguish between players based on their uniforms.
- Calculated ball possession based on the masked pixel area of each team.

<br>

### Challenges and Results
- Overcame challenges in fine-tuning YOLOv8 to detect fast-moving objects in a cluttered environment.
- Used color-based segmentation to track ball possession, though there are challenges with occlusions and overlapping players.

    <br>
    <img src="{{ site.url }}{{ site.baseurl }}/assets/2_soccer_tracking_analysis.gif" style="height: 401px; width:700px;"/>
    <br>


<br>

### Future Improvements
- Further fine-tuning of the model for detecting overlapping players.
- Refining the possession tracking algorithm to better handle occlusions and complex scenes.
- Expanding the system for real-time game statistics tracking and visualization.
