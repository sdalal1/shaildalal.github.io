---
layout: page
title: Bot-ROS
description: Pointilism painting robot
img: assets/img/franka_botros.png
importance: 1
category: work
---

## Project: Pointillism Painting with Emika Franka Panda Arm

**Description:**

The project was to engineer a sophisticated dot-painting system. The primary objective was to leverage the Emika Franka Panda 7 DOF robotic arm to create captivating pointillism artwork. To achieve this, we integrated cutting-edge technologies encompassing computer vision, color detection, and precision trajectory planning. Notably, the team developed intricate algorithms specifically tailored to transform images into a series of precise points suitable for the pointillism-based painting technique.

**Image Transformation:**
   - Engaged in the intricate process of transforming high-resolution images into a series of meticulously placed dots. This involved meticulous planning and execution to ensure the accuracy and fidelity of the pointillism rendition.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/S_edge_detection.png" title="S image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Edge detection on an image and then converting it to dots
</div>

**Computer Vision Techniques:**
   - Implemented advanced computer vision techniques, including color tracking, and thresholding, to identify the location of brushes and various paint colors on the palette. This enabled the robot arm to precisely interact with the palette and paintbrushes.
   - I was responsible for the color and palette detection algorithm. This algorithm uses an Intel realsense to get the loaction of the palette april tag and create a mask around it. A color detection algorithm was used in this mask to find location of centroid of 6 different colors.
   - The ROS package called color captured the image from the realsense, then change the color pattern from BGR to HSV. For different colors, the lower and upper hue, satuaration and value were found using a trackbar and then fed into the code for permanent color detection. The algorith finds all the conotours in that value range and then finds the centroid of the largest one. 
   - A very imporatant part of this algorithm was to convert cartesian coordinates to the pixel value. To do that the camera instrinsic values were needed which are published on the camera_info topic. The position of the apriltag is in cartesian coordinates which are converted to pixels which is used to create a mask. The centroid of each color is then found in the pixel coordinate and then converted to the cartesian coordinates. These points were then published on the TF tree with respect to the camera frame. 

**ROS MoveIt API Implementation:**
   - Skillfully utilized a custom MoveIt API tailored explicitly for the Franka Panda arm. This facilitated precise trajectory planning and execution, ensuring the robot arm moved accurately to create the desired pointillism patterns.
   - The API created has multiple commands which can be called in another scripts. These constraint different aspects of the robot. 
<ul>
<ul>
<li>The plan path to posiiton command moves the end-effector to a position passed.</li>
<li>The plan path to orientation moves the end-effector to the specified orientation in terms of quaternians.</li>
<li>The plan path to position orienation moves the franka to the designated position with the orientation passed as the orientation of the end-effector.</li>
<li>The plan path to cartesian plans a path to waypoints with the orientation constraint through the path.</li>
</ul>
</ul>

**Successful System Delivery:**
   - Delivered a fully functional and efficient system capable of translating digital images into captivating pointillism artwork. The successful execution demonstrated the effectiveness and reliability of our system's capabilities. The video is as follows:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/S_speedup.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>

<div class="caption">
    A video of the fraka painting a 'S' logo with orange border
</div>

**Future Enhancements:**

Future enhancements include implementing dynamic paint brush checks, optimizing the number of painting points without compromising image quality, expanding the robot's workspace, and incorporating dynamic canvas positioning. These enhancements will amplify the system's creativity and flexibility, enabling it to produce even more intricate and stunning pointillism artwork.


The code can be found at this [github](https://github.com/ME495-EmbeddedSystems/final-project-Group5) link 
