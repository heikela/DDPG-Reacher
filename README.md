# Reacher Project with DDPG and Population Based Training

This repository contains a project for Udacity deep reinforcement learning nanodegree. A deep deterministic policy gradient based agent is trained to control an arm with a joint in order to track a moving target.

## The environment

### Summary

The environment consists of a double-jointed arm, and a target location that the agent is trying to make the arm reach.

### Dynamics

The target moves around in the 3D space near the arm in an unpredictable way. The agent decides what kind of torques to apply to the joints of the arm to reach the target.

### Rewards

The agent receives a +0.1 reward during each time step during which it manages to keep the arm reaching to the target volume.

### Observations

Although a first person view of the world is provided for a visualisation of the environment for humans, the agent's observation space is not this image. The agent observes a vector of 33 floating point numbers. The description of the environment makes it clear that these include position, rotation, velocity and angular velocity of the arm. Presumably they also contain information about the position of the target. In this project I will attempt to train an agent that uses an artificial neural network (ANN) to process the observations and select actions without any a priori knowledge of finer details of the observation space.

### Actions

At each time step, the agent has to choose an action that consists of 4 continuous parameters. These are the torques applied at the joints of the arm the agent moves. Each individual component of each action should take on a value between -1 and 1.

### Solving the environment

The environment is considered solved when the agent achieves an average score of +30 over 100 consecutive episodes. By convention we say the agent solved the environment on the timestep N if timesteps N + 1 to N + 100 are the ones whose average first reaches this goal.

The environment was provided in two different versions. One for training an agent in parallel in several environments, and the other one for training a single agent in a single environment. In this project, we train multiple agents using approaches from the [population based training method](https://arxiv.org/abs/1711.09846), but still use the single-agent environments because we are not really training one agent in multiple environments - each agent in the population is potentially very different from each other.

The project instructions give two different ways to evaluate when the environment is solved, corresponding to the cases where an agent is trained using one environment or multiple parallel ones. Becauase the approach taken here, multiple independent agents training effectively in parallel, isn't an exact match to either case, we evaluate it both ways: 1) determining after how many episodes any one of the agents reaches the solution threshold, and 2) evaluating when the population of agents, taken together (which makes sense if we view the population as a meta-agent) reach an average performance over 100 episodes (for each agent in the population) that reaches the solution threshold.

## Dependencies

This project has four groups of dependencies: IPython Jupyter notebooks, Python libraries used by the agent and the learning algorithm, the Unity Environment described above, and the Unity agents python interface.

### Unity Environment

This project is based on a pre-built Unity Environment for the agent. This is supplied by Udacity at the following locations depending on operating system:

- [Linux](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Linux.zip)
- [Mac OSX](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher.app.zip)
- [Windows (32-bit)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Windows_x86.zip)
- [Windows (64-bit)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Windows_x86_64.zip)

Extract this package into the project directory.

The above links are the to the one-agent environment used in this solution. The 20 agent parallel environment for
further experiments can be found at:

- [Linux (20 agent)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux.zip)
- [Mac OSX (20 agent)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher.app.zip)
- [Windows (32-bit) (20 agent)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86.zip)
- [Windows (64-bit) (20 agent)](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86_64.zip)

### Jupyter Environment

In order to use the code in this repository locally, you need a Jupyter installation. Follow installation instructions at [https://jupyter.org/install](https://jupyter.org/install) or get Jupyter as part of a larger environment such as [Anaconda](https://www.anaconda.com/)

Alternatively, if you have access to the Udacity environment set up for this project in the deep reinforcement learning nanodegree, this environment can be used. It also contains the Unity Environment preinstalled.

### Python dependencies

The solution depends on the following packages:
 - numpy
 - scipy
 - pytorch (0.4.0)
 - matplotlib

These can be installed with pip / conda.

Additionally, the unity agents package and its dependencies are required for connecting the python-based agent to the unity environment. The pre-built Unity environment is built with Unity-Python API level 0.4, which means we need to find a compatible version of the unity agents to install.

This can be achieved e.g. by choosing a suitable release commit from git, as follows:

```
git clone git@github.com:Unity-Technologies/ml-agents.git
cd ml-agents
git checkout 1ead1ccc2c842bd00a372eee5c4a47e429432712 
cd python
pip install -e .
```

The chosen commit is the one tagged as version 0.4.0b in [the repository's releases page](https://github.com/Unity-Technologies/ml-agents/releases). The final command installs the python dependencies of the ml agents connector.

## How to run this code

The code to define and train the agent is included in the interactive Python notebook [Report.ipynb](Report.ipynb). To train the model from scratch, execute each code cell in the notebook in order. To evaluate a pre-trained model, create an agent and replace network weights with ones loaded from a checkpoint included in the repository, as shown in section 5. If you read the report outside of an actual Jupyter environment, e.g. in the Github UI, some of the output from simulation runs is verbose - inside Jupyter this becomes a scrollable area with limited height, leading to better readability.
