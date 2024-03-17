---
layout: page
title: Reinforcement Learning for Quadruped
description: Weaving poles using a Unitree Go1
img: assets/img/unitree1.png
importance: 2
category: work
---

## Project: Quadruped Reinforcement Learning for Dynamic Weaving on Unitree Go 1

**Description:**

The objective of this project is to perform weaving motion around the poles with a quadruped using Reinforcement learning. The project environment is set up in Nvidia IsaacGym and uses a Unitree Go1 quadruped.

**Training:**

The training is a two step process and they are as follows:

- **Base-Policy**
<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        <!-- {% include youtube.html id="PLyBZDvgqVs" %} -->
        {% include video.html path="assets/video/202403060006.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>
<div class="caption">
    Base policy setup with known scandots location and heading
</div>

The Base policy is trained in the simulation using all the known data specially the goal position and the heading. The scandot positions are set as goals and a reward is offered if the quadruped reaches the scandot. The simulation will penalize collision and joint/torque limit extension. This leads to the joint states being trained for the task.

- **Distillation Policy**
<div class="row" >
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/Unitree_distillation.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>

<div class="caption">
    Distillation video of Unitree performing weaving
</div>

The second step is the training of the Distillation policy. The training is build up as a layer on top of the base policy where the joint state trained are saved in history and used for further training. Here, the camera acts as the main source of input and the scandots and heading knowledge (privileged_obs) is not known. Here using the joint states and the camera input, the policy find the right heading with respect to the obstacle.

The quadruped gets the same rewards and penalty to get the same robust output. This policy once trained is saved as a pytorch-jit for deployment on the real robot.

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

While training, the policy is kept track of by keeping a close eye on the loss graphs. Once the loss converges, The training is stopped, played to make sure that the joints work and then resumed again for distillation or saved to be ran on the real robot. 

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

**Terrain Manipulation**

The gym terrain needed to be set up to represent poles to weave around. Mutiple environments were set up for training and training was done on a remote machine using docker. The terrain is a vital part of this training as the interaction with the terrain is what leads to the actor training.

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

**SIM TO REAL**

The conversion of simulation into the real life setup is a work in progress. The deployment will be done using ROS2. 
The simulation is divided into two big portions, observations and the depth space. The sim-to-real pipeline for a closed loop policy needs to take a sensor data into account (RealSense 435di camera) and use the data to complete the loop. All image processing is done through multiple Neural networks compressing to be represented by a latent space.
To use the camera raw image, it is first converted to the same quality as seen in simulation. This image is a very compressed version of the raw image. This image also doesnt have multiple color channels. 
To the modified image, another compression is applied which changes the picture into just a latent representation of the space. This representation goes through another model to convert it to a 1x32 size depth latent representation of the raw image. 

This latent representation is then used as an input for the trained model with the observation space which gives actions as joint states.

A ROS2 package was created to use the camera input and use the compression models on those. Below is a video of depth policy working on the quadruped while being teleoperated.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/Unitree_full.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
</div>

**SIM TO REAL PIPELINE TEST**

A sim to real pipeline test of [quadruped_locomotion_project
](https://github.com/muye1202/quadruped_locomotion_project) package:

<div class="row">
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/pipeline_test.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false %}
    </div>
    <div class="caption">
    Pipeline testing for sim to real
    </div>
</div>
</div>

This project is highly inspired from the [Extreme-Parkour](https://github.com/chengxuxin/extreme-parkour) package created by CMU and is built on top of [RSL-RL](https://github.com/leggedrobotics/rsl_rl) Nvidia Package.


The code can be found at this [github](https://github.com/sdalal1/) link 
