[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif "Trained Agent"
[image2]: https://miro.medium.com/max/561/1*n8UyR2HxQPudoBbZ6z4MjA.png "Dueling DDQN"

# Project 1: Navigation

### Introduction

For this project, I will train an agent to navigate (and collect bananas!) in a large, square world.  

![Trained Agent][image1]

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana.  Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas.  

The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around agent's forward direction.  Given this information, the agent has to learn how to best select actions.  Four discrete actions are available, corresponding to:
- **`0`** - move forward.
- **`1`** - move backward.
- **`2`** - turn left.
- **`3`** - turn right.

The task is episodic, and in order to solve the environment, your agent must get an average score of +13 over 100 consecutive episodes.

### Getting Started

1. Download the environment from one of the links below.  You need only select the environment that matches your operating system:
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Windows_x86_64.zip)
    
    (_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

    (_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Linux_NoVis.zip) to obtain the environment.

2. Place the file in the DRLND GitHub repository, in the `p1_navigation/` folder, and unzip (or decompress) the file. 

### Implementations

1. Define the Deep Q-Network:
    - with Dueling DDQN, the performance improved
    - As we had observed the Vector Observation space size in this environment is 37, so I have defined First hidden layer units as 128 and 2nd hidden layer as 32, small enough to run in CPU
    - Then 2 liner hidden layers for state value and advantage values, same as 2nd hidden layer, 32, respectively
    - Relu is a better choice in this senario than tanh in this senario, as we need the values diverse enough to make the choice of actions.
    
    - ![Dueling DDQN][image2]

2. Define the Replay Buffer:
    - Make a deque for memorizing episode
    - Each step of episode will be saved in the buffer
    - Sample randomly from the buffer once it was filled more than the batch size
    - TODO Next, implement prioritized DDQN
    
3. Define the Agent:
    - Since DDQN is off-policy learning, I need to define 2 Deep Q-Networks: local and target, the local network will used for generating policy, and the target network will be used as updating the better policy
    - As Double DQN, in each learning step, we will find the max value actions in local network, and then use this action to get Q value from the target network.

4. Train the DQN:
    - Epsilon starts from 1 and times 0.97 as decay, this will make it down to 0.002 around 200 episode
    - The navigations goes 300 - 1000 steps, so make the minimum epsilon as 0.0005 gets very good result at first 300 episodes
    - The model reach average score as 13 around 300 episodes, and 15 in 500 episodes
    
    
