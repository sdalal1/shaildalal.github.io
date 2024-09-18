---
layout: page
title: Dynamic Obstacles for Powered Wheelchair
description: single and multi actor systems
img: assets/img/IMG_9256.PNG
importance: 1
category: work
---

## Project: 

##### **Description:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The objective of this project was to create dynamic obstacles for a Semi-autonomous wheelchair study. The wheelchair is equipped with a LUCI system which consists of an array of camera's and sensors to enhance the capabilities of the wheelchair. The purpose of this project was to stress test the system by using robot robots as dynamic obstacles for the wheelchair to detect and record the reaction of the users and the system to these moving obstacles. 

There were two major milestones for the project:

1. Single robot interaction
2. Swarm system with 3 robot emulating a crowd

##### **Hardware used:**

**First Hardware iteration:** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The Hardware used was a Mecanum wheel drive robot to emulate human like movement in both x-y directions. The hardware list included raspberry pi 4b as the controller, polulu motors and motor drivers and adafruit PWM board. The BOM is attached as an excel file here. 

<div class = "row mt-3">
    <div class="col-sm mt-2 mt-md-0">
    <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.liquid loading="eager" width="40%" path="assets/img/car_v1.jpg" title="Raw terrain" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>


**Machining HEX Mecanum Wheel Coupler** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The first version of the coupler were 3D printed which were not the most reliable, so decided to machine aluminum hex coupler from a square bar. This was done using a Mill and a CNC lathe. 

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/hex_1.jpg" title="Raw terrain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/hex_2.jpg" title="Multiple Actors Training" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
</div>


**Second Hardware Iteration** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; For the second iteration, a control board from Yahboom was used with a ROS2 controller in-built for mecanum wheel robot. The BOM is attached as the excel sheet. 

<div class = "row mt-3">
    <div class="col-sm mt-2 mt-md-0">
    <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.liquid loading="eager" width="40%" path="assets/img/car_v2.jpg" title="Raw terrain" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

Both these iterations were used for the swarm system created. 

##### **Software Created:**

1. **C++ driver for first iteration of hardware**
   - Created a custom ROS2 and C++ driver for the first Iteration of the robot. The hardware wasn't ROS supported so I created a c++ driver which can supply required PWM to the controller for the robot speed control. I used open source packages to be able to control the pins for the raspberry Pi (Wiringpi) and also a c++ package create for the PWM driver.

2. **Kinematics Library for Mecanum Drive**
   - Created a c++ kinematics library using Modern Robotics to control the robot using a commanded twist. This allows a smooth control of the robot in the desired direction and using the driver allows the speed control of the robot. The library provides Inverse Kinematics and Forward Kinematics provided the radius of the wheel and the length and and width of the robot.

3. **ROS2 C++ package for robot control**
   - Integrated the driver and the library to create a package to control the robot using a commanded twist. The twist can be published on the /cmd_vel topic which will be read by the robot. 
   - For the video here to test all the possible movements, I used the teleop_twist_keyboard to command a twist.
  
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/All motions.mp4" width="50%" class="img-fluid rounded z-depth-1" controls=true autoplay=true style="height: 100%;" %}
    </div>
</div>

1. **Joystick Control of the Robot**
   - Used a PS4 controller and the ros2 joy_node to create a joystick control of the robot. This was done with the use of the controller already created.

2. **Non Reactive Speed Control**
   - Created a package to create a non reactive speed control of the robot. This was done as the first iteration of the obstacle where the robot was completely un-reactive to the environment. This was a more uncontrolled version where the robot would move in a straight line. Multiple services were created to perform different kinds of speed control.
   - The 4 speed versions were as follows:
     - Slow Speed - The speed was set at 1000pwm, the slowest the robot could move.
     - Fast Speed - The speed was set at 4095pwm, the fastest the 12 bit PWM can achieve.
     - Human Speed - For a average human speed (3km/hr), the pwm was set for 2650.
     - Erratic Speed - A set of 4 speed was created where the robot would cycle through them.

3. **Reactive Controller for Single Obstacle**
   - The reactive controller is set up around the car being controlled with a twist. The actors in the scene for a single robot are the robot car and the wheelchair.
   - For the proof of concept a simulation was created in simulation where the robot car was controlled using twist and the wheelchair would be non reactive. The two scenarios which I tested were a static wheelchair and a wheelchair that moves in a straight line. 
   - The robot is commanded a twist towards the goal location which is set, while on its way there if the robot comes in the wheelchair collision radius, it receives a twist tangential twist which makes it avoid the wheelchair until it gets out of the collision radius. Once the obstacle is avoided it follows a path towards the goal pose and stops there. 
   - Here are the two videos side by side

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/obs_avoidance_with_steady_obs.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Static Wheelchair
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/obs_avoidance_with_moving_wc.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Moving Wheelchair
    </div>
</div>
</div>

   - After this proof of concept, I tested the code in real env. To do that, I used a global camera (realsense 455d) which can look at the entire map where the experiment was conducted. To get the locations of the objects in the frame apriltags were used. A world frame apriltag was set up as the connection between the camera and the floor. The other apriltags were considered in relation to the world apriltag on the floor. 
   - To test the robot car reactivity to the movements in apriltags, without setting it up on the wheelchair, kyle moved around the wc apriltag to see how the robot car would react and reach the goal.
   - Some fails and examples are as below:

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/kyle_good.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Good Avoidance
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/kyle_blooper.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Blooper Avoidance
    </div>
</div>
</div>

  - After some iterations and getting the right movements and things, Here are the final videos of the car reacting to the wc movement. This video has changed hardware in terms of the car chassis but not for the electronics. The video test is now with a powered wheelchair and the car. 

<div class="row center-content">
    <div class="col">
        <div class="col">
            {% include video.html path="assets/video/larrisa_good_test.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
        </div>
        <div class="caption text-center">
            Obstacle avoidance with Wheelchair
        </div>
    </div>
</div>

  - To get a better interaction an additional goal pose was added in front of the wheelchair, the controller was changed where the first task for the robot car is to reach in closer to the assigned pose, once it does that, it avoids the wheelchair and then follows a path towards the goal pose. This allows forces interaction with the wheelchair user and get better result.
  - In the test below, the robot car will first path towards the wheelchair, avoid it and then path towards the goal pose. 

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/controlled_1.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Test1
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/controlled_2.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Test2
    </div>
</div>
</div>

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/demiana_full_test_1.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Test3
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include video.html path="assets/video/demiana_full_test_2.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="caption">
        Test4
    </div>
</div>
</div>