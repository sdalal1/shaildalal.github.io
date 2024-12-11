---
layout: page
title: Reinforcement Learning for Flexible back Quadruped
description: Learning gaits using RL for a self designed quadruped
img: assets/img/segmented_back.gif
importance: 0
category: work
---

### **Project: Designing a Quadruped and Implementing Gaits Using Reinforcement Learning**
<br>

#### **Overview**
This project focused on designing a high-speed quadruped inspired by the biomechanics of agile animals like greyhounds. The primary goal was to develop a modular quadruped capable of efficient locomotion by integrating mechanical innovation with reinforcement learning (RL). The project consisted of three phases:

  1. **Simulation of various robot designs** to optimize locomotion parameters.
  2. **Development of walking gaits using RL** in simulated environments.
  3. **Construction of a modular quadruped** for real-world deployment and evaluation.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include youtube.html  id="1mVXlgvw5_c" %}
    </div>
</div>

---

#### **Simulation and Design Innovations**
The initial phase involved extensive simulations in MuJoCo to explore various robot configurations and their impact on locomotion efficiency, with iterative design improvements focused on reducing complexity while enhancing performance. Starting with variations of the Half-Cheetah template, the simulations incorporated design changes inspired by greyhounds to optimize the quadruped's structure and mechanics for greater efficiency.

Quick Summary of the changes sections:

- **Reducing Degrees of Freedom (DoF)**: 
  - Simplified the standard 12-DoF configuration to 8-DoF by eliminating hip motors.
  - Improved straight-line speed and reduced control complexity while maintaining essential movement.

- **Torque Optimization**: 
  - Modeled motor torques to mimic the force distribution seen in greyhounds, emphasizing stronger upper-leg joints.
  - Enhanced energy efficiency and stability during high-speed motion.

- **Segmented Back Design**: 
  - Introduced a flexible, spine-like back to improve stride length and energy transfer.
  - Simulations demonstrated a significant increase in speed and smoother gaits compared to rigid designs.

- A Picture Collage of all the different styles

<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/multiple_designs.png" title="Collage" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

**2 Dimensional Design**
  - Using the half-cheetah model—a 2D, 6-DOF system—I aimed to simplify the design to a 4-DOF configuration, laying the groundwork for an eventual 8-DOF quadruped system. This approach sought to demonstrate that a hip joint is not essential for linear motion. The modifications were explored as follows:
    - Removing the foot link: The first iteration involved eliminating the foot link to create a 4-DOF system constrained to linear motion, preventing sideways movement.
    - Passive Shin Actuation: The second approach removed the shin motor, allowing the shin to move passively under the influence of the foot and thigh motors while maintaining a 4-DOF setup.
    - Passive Foot Actuation: Lastly, the foot motor was removed, enabling passive actuation of the foot, with the thigh and shin motors controlling movement.
  
  - Despite these efforts, the 2D models struggled to achieve effective training results. The primary challenge stemmed from the critical role of the original 6-DOF configuration in maintaining balance during motion. Without it, the system required the legs to move at high speeds to prevent the torso from tipping forward or backward, leading to instability.
<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/2D_design.png" title="Collage" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

  - This exploration highlighted the limitations of simplifying the 2D design, prompting a shift towards developing an 8-DOF quadruped system in 2D. The insights gained from these experiments guided the transition to a full 3D design with four legs, which I will elaborate on in the next section.

**4 Legged 3D design**

- Following the challenges encountered in achieving stable gaits without tipping in the simplified 2D designs, I transitioned to modeling a complete 4-legged system. This shift introduced new complexities due to the hierarchical parent-child relationship of links in MuJoCo, necessitating several iterations to ensure the legs were both individually articulated and properly connected to the torso link.

- Initial Design and Y-Direction Constrained Training:
    - Once the 4-legged model was successfully constructed, I began training the gaits with a constraint limiting movement to the Y-direction. This approach yielded excellent results, producing smooth forward motion and a natural walking gait for a single-link torso.

    - This stage leveraged motor values from the original MuJoCo half-cheetah model and demonstrated the feasibility of stable motion.

- Scaling for Real-World Implementation:
    - After achieving promising results in simulation, I refined the model to be more realistic for physical implementation.
    - The entire system was scaled down to approximately one-third the size of the original half-cheetah model.
    - Motor torques were adjusted to reflect specifications of off-the-shelf components, aligning the simulated design with practical constraints.

- Training in 2D:
    - Using the scaled-down model, I trained the quadruped in 2D. This phase was highly successful, resulting in stable walking gaits.
  
- Unconstrained Gait Training:
    - Building on the success of 2D training, I removed the Y-direction constraint to enable movement in all directions.
    - This transition opened up new opportunities for refining the model, focusing on achieving balance and developing more versatile and efficient gaits, which will be elaborated on in the reinforcement learning section.

<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/3D_design.png" title="3D Design" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>
---

After the successful training of unconstrained gaits, I focused on designing a real-world quadruped based on the specifications derived from the simulation. The details of this physical design process, including hardware considerations, are discussed in the Quadruped Design section.

This iterative approach bridged the gap between simulation and real-world application, providing a robust foundation for efficient quadruped locomotion.

#### **Reinforcement Learning for Gaits** 

To develop efficient and stable locomotion for the quadruped, a custom MuJoCo environment was designed to integrate the unique features of the robot's 8-DOF configuration. This environment played a critical role in refining gait patterns through reinforcement learning (RL).

##### **Custom Environment Features** 
The environment was built to closely model the physical quadruped, featuring: 
1. **Action Space**: 
   - Tailored to the 8-DOF quadruped, the action space was designed to ensure precise and efficient motor control.
   - Torque outputs for the motors were restricted to a range of [-20, 20], aligning with the capabilities of the selected motors.
   - This configuration not only facilitated effective simulation but also enabled a seamless transition to real-world torque control.
2. **Observation Space**: 
   - The observation space was carefully constructed to provide the RL model with a comprehensive understanding of the quadruped’s state. Key components included:
     - `qpos and qvel`: Representing joint positions and velocities relative to their parent bodies, dynamically adapting to changes in the number of links in the model.
     - `IMU Feedback:` Positioned at the center of the torso, the IMU provided critical data for maintaining balance and posture.
   - As new segments were introduced, the expanding observation space was systematically managed to ensure consistent performance during training.

3. **Energy and Performance Monitoring**: 
   - Metrics such as motor energy consumption, speed, and upright posture were meticulously tracked throughout the training process.
   - These insights were instrumental in optimizing performance, ensuring that the quadruped maintained stability and efficiency during locomotion.

<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/PPO_flowchart.png" title="Flowchart" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

By combining these features, the custom environment offered a realistic simulation platform that effectively bridged the gap between virtual training and real-world implementation. The reinforcement learning process leveraged this robust environment to develop adaptive and reliable gaits for the quadruped.

##### **Training Process** 
The quadruped's locomotion capabilities were developed through a systematic reinforcement learning process using a custom-designed training pipeline. This process included the selection of an appropriate RL algorithm, crafting reward functions tailored to the quadruped's requirements, and deploying a robust training infrastructure.

1. **Algorithm Selection and Implementation**  
  -  The primary algorithm used was PPO from Stable Baselines3, chosen for its reliability and effectiveness in continuous action spaces. Its ability to balance exploration and exploitation while maintaining training stability made it ideal for the quadruped's dynamic and complex control requirements.  
  - PPO's hyperparameters were fine-tuned to optimize training outcomes, with the key parameters being:
     ``` bash
        learning_rate=3e-4,
        n_steps=2048,
        batch_size=64,
        n_epochs=10,
        gamma=0.99,
        gae_lambda=0.95,
        clip_range=0.2,
        clip_range_vf=None,
        ent_coef=0.0,
        vf_coef=0.5,
        max_grad_norm=0.5,
        use_sde=False,
        sde_sample_freq=-1,
        target_kl=None,
        tensorboard_log=log_dir,
        policy_kwargs=policy_kwargs,
        verbose=1,
        normalize_advantage=True,
        device='cpu'
      ```
2. **Exploration of Other Algorithms:**  
   - Alternative algorithms such as Soft Actor Critic (SAC), Advantage Actor Critic (A2C), and Twin Delayed DDPG (TD3) were also tested but did not yield favorable results for this application. These algorithms faced issues in stabilizing the gaits or maintaining efficient learning for the high-dimensional action space of the quadruped.

- **Reward Functions**: 
  - Custom reward functions were critical in driving the quadruped's learning process. These functions were designed to encourage desirable behaviors while penalizing inefficient or unstable actions.
  - **Successful Rewards:**
    - **Speed Reward:**  
      A significant reward multiplier (10) was applied to the forward velocity to encourage faster and smoother locomotion.  
    - **Upright Posture Reward:**  
      To ensure balance, the reward was proportional to the torso's height relative to the desired upright position, calculated as:  
      `upright_reward = -abs(current_z_position - upright_position)` 
  - **Experimental Rewards:**
    - **Joint Position Reward:**  
     Encouraged joint angles to stay within a predefined range, promoting stability.  
     ```python
     reward = 0
     for joint_name, (min_val, max_val) in joint_ranges.items():
         joint_angle = self.sim.data.get_joint_qpos(joint_name)
         if min_val <= joint_angle <= max_val:
             reward += 1.0
         else:
             reward -= 1.0
     ```  
    - **Joint Effort Penalty:**  
     Penalized excessive torque exertion to prevent flailing and promote smooth movement.  
     ```python
     penalty = 0
     for joint_name, max_tor in max_torque.items():
         joint_torque = self.sim.data.actuator_force[self.model.actuator_name2id(joint_name)]
         if abs(joint_torque) > max_tor:
             penalty -= 0.1 * (abs(joint_torque) - max_tor)
     ```  
     While these experimental rewards provided insights, they proved overly complex or less effective compared to simpler strategies.
- **Training Infrastructure**:
  - The training was conducted on a headless server to maximize computational efficiency. A virtual environment was created to ensure seamless integration of the training pipeline, which included MuJoCo and Stable Baselines3. All dependencies and configurations are documented in the GitHub repository linked to this project.

##### **Evaluation** 
Post-training, the model's performance was rigorously evaluated using logged data and visualized insights.

The RL model's performance was evaluated through metrics like: 
1. **Performance Metrics:**  
   - **Energy Expenditure:** Assessed the total energy consumed by the quadruped during locomotion to identify efficient gait patterns.  
   - **Motor Efficiency:** Evaluated the torque-to-energy ratio to ensure optimal motor utilization and reduced power wastage.  
   - **Maximum Velocity:** Measured the peak velocity achieved during simulation without compromising stability or balance.

2. **Training Metrics and Model Selection:**  
   - Training progress was monitored using TensorBoard, particularly focusing on value loss, which served as a key indicator of model stability and convergence.  
   - Comparative analysis of value loss across different timesteps helped identify the best-trained models. Timesteps with consistently lower value loss, indicative of reduced variance and improved learning, were selected for deployment.  
   - Key checkpoints were analyzed to balance performance and efficiency, ensuring the selected models excelled in metrics like reward accumulation and energy optimization.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/val_loss_1.png" title="Value Loss 1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/val_loss_2.png" title="Value Loss 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/val_loss_3.png" title="Value Loss 3" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/val_loss_4.png" title="Value Loss 4" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
</div>

The best-performing models were identified by analyzing the value loss curve during training. Models where the value loss steadily decreased and reached near-zero values at specific timesteps produced the most stable and efficient walking gaits. These curves reflected well-converged policies with minimal variance between predicted and actual rewards.

In contrast, models with value loss curves that plateaued early or fluctuated excessively resulted in suboptimal gaits, marked by instability or inefficient motor control. Successful models displayed a clear downward trend in value loss, aligning with balanced, energy-efficient locomotion. Focusing on these timesteps ensured the selection of optimal gaits and refined overall performance.


- **Visualization Tools:**  
   - TensorBoard provided real-time insights into reward trends, policy gradients, and loss metrics during training.  
   - Custom plots for energy consumption, motor power, and velocity trends offered a comprehensive view of the quadruped's capabilities, aiding in further refinement.  
   - Trends in the value loss curve were used to validate the effectiveness of training strategies and ensure alignment with performance goals.

This structured evaluation approach facilitated the selection of optimal models while providing actionable insights to guide future iterations and enhance the quadruped's real-world performance.


<div class="row" >
<div class = "col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3D-metrics.png" title="3D metrics" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Single link metrics
    </div>
    </div>
    <div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/seg_back.png" title="seg back metrics" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Segmented back metrics
    </div>
</div>
</div>

The left cluster of graphs is for a single link torso and the right is for a multi link torso. We can easily compare that the max speed achieved by a segmented back is 1.47 while the single link is about 0.8. This proves that the segmented back is overall faster than a single link quadruped.

---

#### **Designing and Building the Quadruped** 

The modular quadruped design translated the simulation results into physical hardware:

- **Mechanical Structure**: 
  - The robot's frame was constructed using 20-20 aluminum extrusions, chosen for their lightweight yet durable properties. A belt-pulley system was integrated to ensure smooth and efficient leg movement. The modular nature of the aluminum frame allows for easy adjustments and upgrades, enabling the design to evolve based on user needs.
  - The belt-pulley system was inspired by old versions of MIT cheetah. This design is easy to set up, especially with 20-20 extrusions as they can be easily used to tension the belts. 
  - The motors rotors don't have an axle but a certain pattern which needed to be utilized. To create the entire assembly custom parts had to be designed and made out of aluminum due to high torque to avoid breaking points. 
  - There were constant design iterations which are as below:

<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/mech_iterations.drawio.png" title="Flowchart" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

- **Actuation and Control**:  
  - To meet the torque demands identified in the MuJoCo simulations, where the peak requirement for the larger quadruped model reached 21Nm, high-speed brushless DC motors with a peak torque output of 22Nm were selected. These motors were chosen not only for their torque capacity but also for their high rotational speed and light weight, enabling efficient and dynamic locomotion. Integrated with ODrive controllers, these motors offer seamless integration and precise control, facilitating rapid prototyping and fine-tuned adjustments for optimal performance.
  - After doing some research, the motors selected were Steadywin GIM8108-8 Gear Motor.
  - Some important motor details are as follows:

  ```bash
      - Gear Ratio - 8:1
      - Diameter - 97mm
      - Stall Torque - 22Nm
      - Nominal Torque - 7.5Nm
      - No Load Speed - 320rpm
      - Nominal Current Rating - 7A
      - Peak Current Rating - 25A
      - Voltage Rating - 48V
      - Mass - 375g
  ```

  - The motor information is attached here.
    - [Motor information](/assets/img/33.jpg)
    - [Motor Datasheet](/assets/pdf/SteadyWin_GIM6010-8_Manual_rev1.pdf)

<div class="row" >
<div class = "col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/motor.jpg" title="Motor" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Steadywin GIM 8108-8 Motors
    </div>
    </div>
    <div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dog_leg_design.jpg" title="leg Design" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="caption">
        Complete Leg Design
    </div>
</div>
</div>

- **Feet Design**
  - The feet is 3D design found on thingyverse under squash ball feet which were used as feet for 3D printers [link](https://www.thingiverse.com/thing:5404058). The idea originated from original MIT cheetah design which use squash balls as well. 

- **Electronics**
  - The motors were powered from a 48V-25A power supply. One of these power supply were used to power 4 motors at a time.
  - The [link](https://www.amazon.com/BOSYTRO-Switching-Transformer-Security-Industrial/dp/B0CWTY3G1C?crid=3N6PAJJWM48IX&dib=eyJ2IjoiMSJ9.JwQ5wUAfersA751DQzAN8UCs2y9rV1ASN-KIYq6DzLH-4czIkQQKia4z7YiqPql-94A3OA2g8fjTq4gG3p0A7OuF49hgJqFwrE7v9orUOxMg-4u1sumUQ4fvg4MlbVBJhRwCyIi-bVl3Mfxz9fpqdSuNcBJf3jOWsmpTKtZ-TFtr7JmfrPEbBKZQl1wDG3hB1omnKIK1c5TcahXdz4IRqFskz31rKPqtQOFtr0UmZR0oJOXRSaWr2n2KoHiJj8HLt2ohO3KcfxHkPrpuQMrH65hnHUMLBx8KiuuaABQzB6o.hTOJSAWli0_erfQR_bbYcKEogYxvRgp6WB60dcbRChw&dib_tag=se&keywords=48v%2Bpower%2Bsupply%2B30a&qid=1731359779&s=electronics&sprefix=48v%2Bpower%2Bsupply%2B30a%2Celectronics%2C114&sr=1-1&th=1) to the power supply
  - For communication with the motors, CAN-bus was setup with a raspberry pi4B using a RS485 Can-hat.

- **Sensors**:
  - IMUs and encoders were integrated to provide real-time feedback, replicating the trained RL model's behavior on physical hardware. These sensors ensured accurate motion tracking and contributed to the quadruped's stability and performance during real-world tests.


##### <ins>**Full Quadruped Pictures**</ins>

Here are some pictures of the fully built quadruped
<div class="row" >
<div class = "col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/full_dog_1.jpg" title="Dog Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    </div>
    <div class="col">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/full_dog_2.jpg" title="Dog Setup 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
</div>

The video on the right shows position control test for motors in home position. The video on the left is a small snipped on the quadruped standing under its own weight.

<div class="row" >
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
             {% include youtube.html  id="MRok_12rD68" %}
    </div>
    <div class="caption">
        Dog Standing under its own weight
    </div>
</div>
<div class="col">
    <div class="col-sm mt-3 mt-md-0">
            {% include youtube.html  id="BXxDHsZlvpk" %}
    </div>
    <div class="caption">
        Position Control of the Dog
    </div>
</div>
</div>


---

#### **Sim-to-Real Pipeline**

The transition from simulation to reality was facilitated through a well-defined sim-to-real pipeline:  
1. **Direct Deployment**: 
   - The Rl trained model can be deployed on the real robot directly as the mujoco model is modelled directly around the real quadruped.
   - The action space of the model is torques, so the RL model can be deployed using torque control.
   - Another way it can be deployed is using position control and deploying positions for motors which can be extracted directly from mujoco's physics simulations. 
   - Here is a layout of how this deployment would work:
<div class="row mt-3">
    <div class="col-sm mt-2 mt-md-0">
        <style>
    .center-img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    </style>
        {% include figure.html path="assets/img/sim_real.drawio.png" title="Flowchart" class="img-fluid rounded z-depth-1" class="center-img" %}
    </div>
</div>

1. **Future Works**: 
   - Plans include integrating OptiTrack or camera-based systems to improve positional accuracy during real-world tests.
   - Implementing the segmented back setup on a real quadruped.
   - Completing the sim to real pipeline for the built quadruped.

---

#### **Innovations and Learnings**  
The project introduced several unique features that improved speed and efficiency:  
1. **Reduced Motors**: Achieving high speeds with an 8-DoF configuration reduced cost and complexity while maintaining effective locomotion.  
2. **Segmented Back**: Inspired by animal anatomy, this feature significantly enhanced the quadruped's agility and speed in both simulation and physical tests.  
3. **Custom RL Environment**: Developing a tailored environment allowed efficient training and evaluation of locomotion strategies, with detailed metrics for energy optimization.  

---

<details>
  <summary><b>Working with Stanford Pupper V1</b></summary>

  <p>
    I also worked with a Stanford Pupper before designing my own quadruped. I built and assembled the Pupper V1 kit available on MangDang.com. This particular open-source quadruped was selected for two main reasons: it is completely open-source and easy to fix, and its single-segment back allowed for quick iterations if needed.
  </p>

  <ul>
    <li><b>Reinforcement Learning with the Pupper:</b>
      <ul>
        <li>
          Similar iterations were performed on the Pupper and trained in simulation using MuJoCo. The walking gaits were also converted and implemented on the real robot using an open-loop position control method.
        </li>
      </ul>
    </li>

    <li><b>MuJoCo Models:</b>
      <ul>
        <li>
          Here are the MuJoCo models of the Pupper used for simulation.
        </li>
      </ul>
    </li>

    <li><b>Issues with the Pupper:</b>
      <ul>
        <li>
          After completing the open-loop pipeline, I identified several issues with the Pupper that led to designing my own quadruped. These include:
          <ul>
            <li>
              The goal was to create a very fast quadruped, but this was challenging using hobby servos on the Pupper.
            </li>
            <li>
              The Pupper exhibited poor stability and had difficulty walking on tile floors due to its very thin carbon fiber leg design, which caused it to slip. Adding rubber washers to the feet did not significantly improve stability.
            </li>
            <li>
              Open-loop control proved insufficient for maintaining stability. These limitations suggested that the Pupper V1 hardware was not suitable for the intended tasks.
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <b>Video of the Open-Loop Control on the Pupper:</b>
      {% include youtube.html  id="fUjVGjAbxI8" %}
    </li>
  </ul>
</details>

---

#### **Aknowledgements**  
I want to thank a couple of people for this project:
  - Matthew Elwin - For constant support and guidance throughout this project.
  - Davin landry - Helping me with mechanical Design iterations and keeping me sane


