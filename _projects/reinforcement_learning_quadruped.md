---
layout: page
title: Reinforcement Learning for Quadruped
description: Weaving poles using a Unitree Go1
img: assets/img/unitree1.png
importance: 2
category: work
---

## Project: Quadruped Reinforcement Learning for Dynamic Weaving on Unitree Go 1

#### **Description:**

The objective of this project is to replicate the intricate weaving motion performed by animals around poles using reinforcement learning with a quadruped. Inspired by one of the most common tricks exhibited by animals, I aim to emulate this movement pattern with precision and agility. To achieve this, I've chosen to utilize reinforcement learning within the Nvidia IsaacGym environment, paired with the versatile Unitree Go1 quadruped. Through this setup, I seek to unravel the complexities of animal-like locomotion and push the boundaries of what's achievable in robotic motion control.

#### **Training:**
The training is a two step process and they are as follows:

- ##### **Base-Policy**
<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        <!-- {% include youtube.html id="PLyBZDvgqVs" %} -->
        {% include video.html path="assets/video/202403060006.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>
<div class="caption">
    Base policy setup with known scandots location and heading
</div>

The base policy is trained within the simulation environment using all available data, particularly focusing on goal positions and headings. Scan-dot positions serve as primary objectives, with rewards granted upon successful attainment by the quadruped. However, collisions and exceeding joint/torque limits result in penalties within the simulation, driving the training process to refine joint states for optimal task performance.

- ##### **Distillation Policy**
<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/Unitree_distillation.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>

<div class="caption">
    Distillation video of Unitree performing weaving
</div>

The second step involves training the Distillation policy, which is constructed as a layer atop the base policy. In this phase, joint states trained by the base policy are stored in history and utilized for further refinement. Unlike the base policy, the Distillation policy relies solely on camera input, without access to privileged knowledge such as scan-dots and heading. Through this setup, the policy aims to determine the optimal heading relative to obstacles using joint states and camera data.

The quadruped receives consistent rewards and penalties to ensure the attainment of a robust output. Once trained, this policy is saved as a pytorch-jit model for seamless deployment onto the real robot.

<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mean_reward_base.png" title="Base Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/training_reward_dist.png" title="Distillation Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Left is the Base policy reward curve and the right is Distillation policy reward curve
</div>

The training flowchart is as follows:

<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/flowchart.png" title="policy flowchart" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

During training, meticulous attention is paid to the loss graphs to monitor the policy's progress. When the loss converges, indicating satisfactory training, several steps follow. Firstly, the trained policy is tested to ensure the proper functionality of the joints. After successful validation, training may resume for distillation or the policy may be saved for deployment on the real robot. This careful process ensures that the policy undergoes thorough validation before advancing, ensuring reliability and performance in real-world applications.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/estimator_loss_base.png" title="Base Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/latent_loss_png_base.png" title="Distillation Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Base policy loss graphs
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/depth_actor_loss.png" title="Base Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/depth_loss_yaw.png" title="Distillation Policy Rewards" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Distillation policy loss graphs
    </div>
</div>
</div>

#### **Terrain Manipulation**

Creating the gym terrain to simulate the poles for weaving was a crucial step in this project. Multiple environments were meticulously set up to facilitate training, with each environment playing a unique role in the learning process. Training was conducted on a remote machine using Docker, ensuring scalability and efficient resource utilization.

The terrain design is of paramount importance as it directly influences the interaction between the quadruped and its surroundings, thereby shaping the actor training process. By accurately representing the obstacles and challenges that the quadruped encounters, the terrain enables the actor to learn and adapt its behavior accordingly. This emphasis on realistic terrain simulation enhances the training process, leading to more robust and effective policies.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/raw_terrain.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Raw Terrain
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/multiple_actors.png" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="caption">
        Multiple actors training
    </div>
</div>
</div>

#### **SIM TO REAL**


The ongoing effort to transition from simulation to real-life deployment is a critical aspect of this project. Leveraging ROS2 for deployment underscores the commitment to robust and scalable solutions in robotics.

In the simulation phase, two key components are at play: observations and depth space. These elements form the backbone of the sim-to-real pipeline for closed-loop policy deployment. Central to this process is the integration of sensor data, specifically from the RealSense 435di camera.

The journey from raw camera input to actionable data involves several stages of processing. Initially, the raw image undergoes transformation to match the quality observed in simulation, often resulting in a compressed version devoid of multiple color channels.

Subsequently, this modified image is subjected to further compression, ultimately yielding a latent representation of the space captured by the camera. This latent representation is then fed into a neural network model to generate a compact, 1x32 size depth latent representation of the original raw image.

Through these intricate stages of processing and neural network compression, the project aims to bridge the gap between simulation and real-world application, ensuring seamless integration and robust performance in varied environments.

<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/latent.png" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


The integration of the latent representation into the trained model, alongside the observation space, serves as a critical input for generating actions, manifested as joint states. This comprehensive approach ensures that the policy developed in simulation can effectively adapt and perform in real-world scenarios.

To facilitate this integration, a dedicated ROS2 package was developed. This package leverages the camera input and applies the compression models to generate the latent representations. By seamlessly interfacing with ROS2, the package enables efficient communication and integration with the broader robotics ecosystem.

For a visual demonstration of the depth policy in action on the quadruped during teleoperation, please find the attached video below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/Unitree_full.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>


#### **SIM TO REAL PIPELINE TEST**

A sim to real pipeline test of [quadruped_locomotion_project
](https://github.com/muye1202/quadruped_locomotion_project) package:

<div class="row">
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/pipeline_test.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>
</div>

This project is highly inspired from the [Extreme-Parkour](https://github.com/chengxuxin/extreme-parkour) package created by CMU and is built on top of [RSL-RL](https://github.com/leggedrobotics/rsl_rl) Nvidia Package.


The code can be found at this [github](https://github.com/sdalal1/extreme-parkour-barkour) link 
