---
layout: page
title: Kuka Youbot Simulation
description: Coppeliasim using MATLAB and feedforward + PI control
img: assets/img/mobile_manipulation.png
importance: 3
category: work
---

## Project: Mobile Manipulation with YouBot Robot

**Introduction:**

The project focused on enhancing mobile manipulator control, successfully culminating in achieving our primary objective. We aimed to empower the YouBot mobile manipulator with specialized software, enabling it to execute complex tasks involving block manipulation efficiently.

**Development Overview:**

1. **Simulation:**
   - Validation using CoppeliaSim ensured precise testing and validation of the YouBot's movements and task accuracy.
   - CSV output capturing essential robot data facilitated accurate simulation in CoppeliaSim for performance evaluation.
   - Emulating realistic physics within CoppeliaSim ensured lifelike interactions between the robot and the block.

2. **Trajectory Planning and Control:**
   - Eight segments guided the robot through various movements, ensuring accuracy in approach, grasping, release, and block positioning.
   - The specialized controller ensured error-free execution, guiding the arm precisely along the planned trajectory.

**Mobile Manipulator Control Using MATLAB:**

The software, developed in MATLAB, effectively controlled and simulated the mobile manipulator. It comprised three key functions:
- **NextState:** Predicting the robot's configuration after a specific timestamp.
- **TrajectoryGenerator:** Generating the desired end effector trajectory.
- **FeedbackControl:** Correcting the robot's configuration to align with the desired path.

**Goals:**

- Tuning parameters, like proportional and integral gain, proved pivotal in steering the robot accurately.
- Surprising insights, such as the impact of specific parameter settings on the robot's behavior, highlighted the project's depth.
- Three distinct runs showcased different scenarios, illustrating successful outcomes.
- The three significant runs are as follows:

- **Run 1 (Error Minimization):**
  - Focuses on minimizing errors in the robot's movements.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/best_edited.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>

- **Run 2 (Overshoot Demonstration):**
  - Demonstrates a scenario where the robot exhibits overshoot behavior.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/overshoot_edited.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>

- **Run 3 (Box Location Variation):**
  - Involves changing the box's location, testing the robot's adaptability.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/new_edited.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>

**Conclusion:**

The successful completion of this project signifies a significant leap in mobile manipulator control. By combining meticulous software development, realistic simulation, and insightful control strategies, I achieved the final goal. The lessons learned pave the way for advanced applications in robotics, underlining the project's success in enhancing mobile manipulator capabilities.

The code can be found at this [github](https://github.com/sdalal1/Mobile-Manipulator) link 