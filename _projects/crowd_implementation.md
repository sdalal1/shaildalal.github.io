---
layout: page
title: Swarm implementation to emulate a crowd
description: Multi Actor Systems
img: assets/img/crowd_3_cars.png
importance: 1
category: work
---

## Project: 

##### **Description:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The objective of this project was to create a swarm of dynamic obstacles for a Semi-autonomous wheelchair study. The swarm consisted of 3 cars and reacted to moving wheelchair in a region.

##### **Hardware used:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The Hardware was developed for the first part of this project and can be looked at under the Dynamic Obstacles for Powered Wheelchair  project. The Hardware consisted of three mecanum wheel cars which would react to the moving wheelchair in the environment.

##### **Software Created:**

1. **Social Forces Algorithm**
   - Implemented the social forces algorithm which is modelled around crowd of humans. The basis of this algorithm revolves around the attraction and repulsion forces humans feel from their environment. The basic example of this is that someone walking in the crowd has a positive attractive force towards the goal location and would move in that general direction. In an opposite case, a human would feel a repulsive force from other humans, cars or anything else in the way towards the goal. My implementation revolves around this where each cars feels an attraction goal towards the goal location and a repulsion force between the other things in the environment. This includes other cars as well as the wheelchair. The wheelchair for my consideration doesn't change its motion in respect to anything in the environment but is controlled in whatever way the human riding it controls it.

    The algorithm revolves around these forces which lead to change in the commanded twist. The functions are explained below:
      - **Repulsion Forces Calculation between two cars:**
        - Parameters --> 
          - A - higher A leads to higher magnitude of repulsion forces
          - B - For distance error metric
          - sigma - leads to smoothness in force change
          - dist - Euclidean dist between two elements
        - Force is calculated using $$Force = Ae^{(B-dist/sigma)}$$, where $$f_x = Force * (dx / dist)$$ and $$f_y = Force * (dy / dist)$$ where dx and dy are x and y differences between the two car positions.
        - This force is then used to decrease or increase the speed of the car as necessary, the way I have it set up is that the first car in the equation receives a negative force while the second one receives a positive force. This is basically done by $$v_{x1} -= fx$$ and $$v_{y1} -= fy$$ while $$v_{x2} += fx$$ and $$v_{y2} += fy$$
      - **Repulsion Forces Calculation between the car and wc:**
        - Here the calculations are done the same way but only a negative force is implemented on the car velocities while the wc velocity is left unchanged.
        - This force implementation can also be done with a static obstacle.
      - **Attraction Force towards Goal:**
        - Parameters -->
          - K - Leads to a higher attraction towards the goal
          - dx - x difference between goal and car location
          - dy - y difference between goal and car location
          - dist - Euclidean dist between goal and car
        - Here the velocity in x and y is calculated using $$v_x+=K*(dx/dist)$$ and $$v_y+=K*(dy/dist)$$
      - **Entire Social Forces Pipeline:**
        - For every iteration, the repulsion forces are calculated between each car as well as between each car and the wheelchair as well as the attraction forces towards the goal. The overall velocity is then published to the cars. The parameters need to be set for a smooth interaction between all actors. 

2. **Sim implementation**

- A matplotlib simulation 
<div class="row">
<div class="col-sm mt-3 mt-md-0">
    {% include video.html path="assets/video/matplotlib_sim.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false muted=true style="height: 100%;" %}
</div>
</div>

- Between three cars starting in three different directions without any wheelchair (Using RVIZ and C++)
<div class="row">
<div class="col-sm mt-3 mt-md-0">
    {% include video.html path="assets/video/crowd_with_3_cars.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false muted=true style="height: 100%;" %}
</div>
</div>

- Between three cars starting in three different directions with the wheelchair (Using RVIZ and C++)
<div class="row">
<div class="col-sm mt-3 mt-md-0">
    {% include video.html path="assets/video/Crowd_simulation_3_dir_moving_wc.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false muted=true style="height: 100%;" %}
</div>
</div>

- Between three cars and the wheelchair where the cars approach the wheelchair head-on (Using RVIZ and C++)
<div class="row">
<div class="col-sm mt-3 mt-md-0">
    {% include video.html path="assets/video/crowd_approaching_wc.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false muted=true style="height: 100%;" %}
</div>
</div>
    
- Between three cars and the wheelchair where the cars approach the wheelchair tangentially (Using RVIZ and C++)
<div class="row">
<div class="col-sm mt-3 mt-md-0">
    {% include video.html path="assets/video/crowd_tangential_wc.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=false muted=true style="height: 100%;" %}
</div>
</div>
    

##### **Ongoing Work**

- Implementation on Real Robots

##### **Acknowledgements**

- Larisa Loke
- Joel Goh
- Dr. Brenna Argall