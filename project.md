# Control Robot Race Cars with Machine Learning
**Author:** Josh  
**Project:** Control Robot Race Cars with Machine Learning

## Abstract
This project focuses on controlling autonomous robot race cars using machine learning techniques. By utilizing the F110th open-source hardware framework, this initiative bridges the gap between hardware control and artificial intelligence in the realm of autonomous racing.

## Introduction
F110th provides an ideal platform for autonomous racing, complete with detailed hardware specifications. It facilitates command integration and feedback mechanisms like scan odometry data, making it an excellent candidate for sophisticated control algorithms.

## Related Works
The project builds upon existing works in autonomous racing and machine learning. Key influences include the F110th open-source hardware framework, similar to the TurtleBot but tailored for racing scenarios, and the use of ROS (Robot Operating System) for effective machine-human interactions. Comparative studies with other autonomous vehicle projects and machine learning applications have shaped the project's development.

## Approach
1. Get a functional simulator.
2. Implement a machine learning trainer using stable_baselines3.
3. Train a model on the car in simulation on randomly generated race tracks.
4. Modify the model where needed to improve performance in simulation. 
5. Create a ros controller that subscribes to scan data and uses the model output movement commands. 

## ROS controller
Key Implementation Steps:
1. **Data Acquisition:** Collecting velocity and LiDAR data.
2. **Model Processing:** Inputting data into the reinforcement learning model.
3. **Action Decisions:** Generating and interpreting actions from the model.
4. **Command Publication:** Converting actions to ROS commands.

## Assumptions
The project assumes a certain level of consistency and reliability in the hardware's performance and sensor readings. It also presumes that the training environment closely mirrors real-world conditions, ensuring that the learned behaviors are transferrable to actual racing tracks.

## Results
The model showed promising results in controlling the race car, with noticeable improvements in path optimization and obstacle avoidance. Challenges encountered included balancing reward mechanisms to prevent overly aggressive maneuvers and ensuring stable performance in varied racing conditions. The application of adjusted reward strategies led to significant improvements in the model's racing behavior.

## Conclusion
The project successfully demonstrates the application of machine learning in controlling autonomous robot race cars. It highlights the potential of AI in enhancing the capabilities of autonomous vehicles, especially in high-stakes, dynamic environments like racing. Future enhancements could include multi-agent scenarios and further refinement of the reward system for more nuanced control strategies.

## Link to Presentation Video
[Watch the Project Presentation](https://www.youtube.com/watch?v=OMefsw3mV8Y)

## Project Repositories
- [ROS PPO Controller Code](https://github.com/JoshCu/ros_ppo_controller/tree/main)
- [F1Tenth Gym Code](https://github.com/f1tenth/f1tenth_gym)
- [RL PPO Trainer Code](https://github.com/JoshCu/f1tenth_reinforcement_learning)
- [ROS Gym Bridge Code](https://github.com/JoshCu/f1tenth_gym_ros/commits/main/)

## Forked Repositories
- [ROS Gym Bridge Code](https://github.com/f1tenth/f1tenth_gym_ros)
- [Source for baseline RL model](https://github.com/meraccos/f1tenth_reinforcement_learning)
