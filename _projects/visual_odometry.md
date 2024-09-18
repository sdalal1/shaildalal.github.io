---
layout: page
title: Visual Odometry on KITTI Dataset
description: visual odometry setup on the Kitti dataset on python
img: assets/img/Orb_feature.png
importance: 5
category: work
---

# Project : Visual Odometry Report Summary

# Visual Odometry Overview

## Introduction
Visual Odometry (VO) is a technique used to estimate a robot's position and orientation by analyzing the motion of visual features captured by a camera over time. VO plays a crucial role in autonomous navigation systems, enabling robots to understand their environment without relying on external references like GPS.

For this project, a complete pipeline of Visual odometry was created and implemented on the Kitti Dataset which ius used for benchmarking tasks like Visual odometry, 3D object Detection and scene understanding. The code was developed in Python using primarily basic packages and OpenCV. While Python is typically not used for real-time applications due to its slower processing capabilities, it excels in visualization and offers a vast array of powerful libraries.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include youtube.html  id="2FxoWn2sxzM" %}
    </div>
</div>

<br>
## Methods

The visual odometry pipeline was performed on the grayscale images from the dataset. The entire pipeline consisted of following steps

##### <ins>Disparity Map</ins>

Taking images from both the left and right camera and create a complete image with features being enhanced.
There are two methods used, one is Stereo BM (Block Model) and Stereo SGBM (Semi-Global Block Model). These models are used to convert the images from a stereo setup to a disparity map.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/disparity_BM.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        BM Disparity Map
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/disparity_SGBM.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        SGBM Disparity Map
    </div>
</div>
</div>


##### <ins>Depth Map</ins>

One of the important feature which needs to be recovered is the depth. This is done using the stereo camera setup and the disparity map generated. For each pixel, the depth is calculate using the formula Z = fb/d. 

- Here Z = Depth, f = focal distance of the camera, b = baseline distance between two cameras and d = disparity between two pixels. This formula is generated using simple geometry from the image below.
<div class = "row mt-3">
    <div class="col-sm mt-2 mt-md-0">
    <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.liquid loading="eager" width="40%" path="assets/img/Stereo_setup.png" title="Raw terrain" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

This formula implemented on both the types of disparity maps to generate a depth map. The center of the image can be seen to have the least disparity but higher depth information.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/depth_map_BM.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        BM Depth Map
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/depth_map_SGBM.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        SGBM Depth Map
    </div>
</div>
</div>

##### <ins>Feature Extraction</ins>

The information from the depth map and the disparity map is used to extract important features. These features are supposed to be change invariant where they are matched with the features extracted from the next frame.

The two ways I implemented this was using SIFT and ORB feature detection. More information about them could be found in the attached report.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/SIFT_feature.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        SIFT Feature Detection
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Orb_feature.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        Orb Feature Detection
    </div>
</div>
</div>

##### <ins>Feature Matching</ins>
From the features that are detected, we pick two frames one after the other and match the features. This matching is then used to estimate the change in position.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Sift_feature_matching.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        SIFT Feature Matching
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Orb_feature_matching.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        Orb Feature Matching
    </div>
</div>
</div>

## Results

For my results, the SIFT matching worked really well while the ORB error keeps on accumulating. The results are as follows.

|            | SIFT (Dataset 01) | ORB (Dataset 01)   |
|------------|------------|------------|
| MAE        | 911.82     | 2891.26    |
| Time       | 3m 0.32 s  | 1m 37.23 s |
| Time/Frame | 0.167 s    | 0.08831 s  |


The final pictures are below:

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/trajectory_03_sift.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        SIFT
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/trajectory_03_orb.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        Orb
    </div>
</div>
</div>

## Challenges
Several challenges are associated with Visual Odometry:

- **Scale Ambiguity:** Without additional sensors, VO struggles with absolute scale, making it difficult to distinguish between a small object close to the camera and a large object far away.
- **Illumination Changes:** Variations in lighting can impact feature detection and tracking.
- **Motion Blur:** Fast movement can cause blur, leading to inaccurate feature detection.

## Possible Improvements

- Integrate the Lidar data with a filter to estimate the pose better
- Use Mapping techniques to improve trajectory estimate

## Applications
Visual Odometry is applied in various domains, including:

- **Autonomous Vehicles:** Used for navigation in environments where GPS signals are unreliable.
- **Robotics:** Enables mobile robots to navigate complex environments.
- **Augmented Reality:** Helps align virtual objects with the real world by tracking the user's movement.

## Conclusion
While Visual Odometry is a powerful tool for estimating motion, it is often complemented by other sensors, such as IMUs or LiDAR, to address its limitations. Integrating Visual Odometry with other SLAM (Simultaneous Localization and Mapping) techniques enhances robustness, especially in dynamic and unstructured environments.


## Acknowledgments

- Worked With : [Graham Clifford](https://graham-clifford.com/)
- Kitti Dataset
- Inspiration - [Visual Odometry for Beginners](https://www.youtube.com/playlist?list=PLrHDCRerOaI9HfgZDbiEncG5dx7S3Nz6X)

## Links

Github - [Computer Vision Project](https://github.com/sdalal1/Computer_vision_projects/tree/main/computer_vision_final_project)
Report - [Visual Odometry Report](/assets/pdf/Visual%20Odometry%20Report.pdf)