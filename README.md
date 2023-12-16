## Overview
This project can be broken down into two parts, training the model, and [running the ros controller and simulation](#running-the-controller). The controller repo already has a trained model in it so running the training is not needed.
# Training the model
I did all of this on wsl but the steps should be similar for any emulated linux or native linux environment

### Requirements
* python3
* pip
* some flavour of linux, I used ubuntu 22.04 lts in wsl2
* nvidia gpu for cuda enhanced training
* [cuda installed and drivers up to date](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

### Requirements mentioned in submodules
The submodules I forked all have dockerfiles that can be pulled and used.
Just as soon as they're updated to account for the recent wsl change that breaks all the display forwarding and GPU integration with rocker.

## Installing the required modules
```bash
# Clone the repo
git clone --recurse-submodules https://github.com/JoshCu/ppo_ros_racecar.git
cd ppo_ros_racecar/f1tenth_reinforcement_learning
# venv for everything as ROS only uses global python
python -m venv gym_env
source gym_env/bin/activate

# this will take a while
pip install -r requirements.txt
# this will complain a lot about dependency issues 
# ignore the issues, it took days to get everything working like it is

# optionally install tensorflow for more monitoring data
# needs to be a separate venv to the one running training
#pip install tensorflow==2.15.0

```

#### If it can't find f1tenth_gym
```bash
cd ..
cd f1tenth_gym
pip install -e .
cd ../f1tenth_reinforcement_learning
```

### Generating the maps
```bash
cd work/tracks
python random_trackgen.py --num_maps=500
# the number of maps needs to be higher than maps = list(range(1, 450))
# defined #22 in train.py   
# 500 instead of 450 because about 10% fail to generate
```
### Training the model
```bash
cd .. #cd to the work folder
python train.py
```
### Changing train parameters (reccomended)
The model is currently optimised to train on an nvidia 3080 and i9 with 20 cores.
The training paremeters will need to be modified so it doesn't crash or run glacially on different devices.

Iside train.py you will find the following
```python
N_ENVS = 20 # this should not exceed you devices cpu core count 
N_STEPS = 2048 # lower this to 128-512 if you're using cpu
DEVICE = "cuda" # "cpu" for cpu only training

if __name__ == "__main__":
    save_interval = 5e4
    eva_freq = 5e4
    n_eval_episodes = 20
    learn_steps = 1e7 # this controls training itterations
    # with current configuration training beyond 10m offers no improvement
```
#### Training time
This about an hour on my pc, but keep playing with n_envs and steps to maximise your itterations per second.

## Modifying rewards
To change the reward functions, look in wrapper.py at FrenetObsWrapper.observation

## Viewing model in simulation
```bash
python eval_test.py
```
---
---
---
---
---
# Running the Controller 
## Requirements
* [ros humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)
* rviz2
* f1tenth_gym
* f1tenth_gym_ros (ros bridge)
* ppo_controller   

The final three are within this repo

## Installation steps 
__IN A NEW TERMINAL__
```bash
#git clone --recurse-submodules https://github.com/JoshCu/ppo_ros_racecar.git
cd ppo_ros_racecar #this repo
pip install -r requirements.txt
# again, this will comlain a lot but it works
cd f1tenth_gym
pip install -e .
cd ../sim_ws
colcon build
```
# Running the simulation
### Sim + bridge
```bash
ppo_ros_racecar/sim_ws
source install/setup.bash
source /opt/ros/humble/setup.bash
ros2 launch f1tenth_gym_ros gym_bridge_launch.py
```
### Controller
```bash
# in another terminal
cd ppo_ros_racecar/sim_ws
source install/setup.bash && source /opt/ros/humble/setup.bash
ros2 run ros_ppo_controller controller
```
