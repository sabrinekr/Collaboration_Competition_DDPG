# Introduction
For this project, we implemented a MADDPG agent to solve the [Tennis](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#tennis) environment.

![Trained Agent][image1]

In this environment, two agents control rackets to bounce a ball over a net. If an agent hits the ball over the net, it receives a reward of +0.1.  If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01.  Thus, the goal of each agent is to keep the ball in play.

The observation space consists of 8 variables corresponding to the position and velocity of the ball and racket. Each agent receives its own, local observation.  Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. 

The task is episodic, and in order to solve the environment, your agents must get an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents). Specifically,

- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the maximum of these 2 scores.
- This yields a single **score** for each episode.

The environment is considered solved, when the average (over 100 episodes) of those **scores** is at least +0.5.


# Learning Algorithm
We solved our environment using the multi-agent deep deterministic policy gradient. MADDPG  is similar to the single agent DDPG which is a policy-gradient actor-critic algorithm proposed by Google Deep Mind to solve continuous action space problems. The MADDPG involves four neural networks for each agent: the critic neural network, the actor neural network, the critic target neural network and the actor target neural network.


It also uses a replay buffer R to store the expriences of the agent.

* The actor neural network: Each agent has his actor network. The actor neural network maps the states into actions. It will take as an input the current state of each agent and will output one recommended action for each agent from a continuous action space. 

* The critic neural network: The critic neural network takes as input the current state and the action given by the actor of his agent and the other agents and outputs the estimated Q-value of the action-state couple. The goal of the critic neural network is to evaluate the policy given by the  different actors using the temporal difference error. 

* Target networks: In order to avoid the divergence of our learning algorithm the maddpg uses actor target network and critic target network for each agent. The actor target network and the critic target network are initialized with respectively the same weights of the actor network and the critic network. Then, they are soft updated.

* Exploration: In a discrete action space we explore the environment with different methods such as the epsilon-greedy policy and the Boltzman strategy. Without exploration the policy is deterministic and to make the ddpg explore more effectively we add noise to the action returned by the network.

# Model architecture
The actor has a target and local networks of the two agents having the same architecture:
* 1 fully connected layer of size 256 followed by ReLu activation function. 
* Batch normalisation on the first layer output.
* 1 fully connected layer of size 256 followed by ReLu activation function.
* 1 fully connected layer of size 2 (the size of the action space) followed by tanh activation function. 

The critic has a target and local networks having the same architecture:
* 1 fully connected layer of size 256 followed by ReLu activation function. 
* Batch normalisation on the first layer output.
* 1 fully connected layer of size 256 followed by ReLu activation function.
* 1 fully connected layer of size 1.


# Hyperparameters 
Our agent was trained using the follwing hyperparameters: 
* Buffer size: the size of the experience replay buffer is 1 000 000
* Batch size: the batch size of the training is 256
* Gamma: the discount factor 0.99
* TAU learning rate coefficient for soft update of target parameters is 0.001
* The learning rate of the actor is 1e-4
* The learning rate of the critic is 1e-3


# Results

The agent was able to solve the environment after 2076 episodes with the average score of 0.502 during the last 100 episodes.

![Image of Yaktocat](https://github.com/sabrinekr/Continuous-Control-PPO/blob/main/images/ddpg.png?raw=true)

# Ideas for Future Work
We still can improve our results by : 
* Using new hyperparameters (Deeper networks for actor and critic)
* Adding prioritized experience replay.