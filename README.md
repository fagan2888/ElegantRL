## Lightweight, Efficient and Stable DRL Implementation Using PyTorch 

[![Downloads](https://pepy.tech/badge/elegantrl)](https://pepy.tech/project/elegantrl)
[![Downloads](https://pepy.tech/badge/elegantrl/week)](https://pepy.tech/project/elegantrl)
[![Python 3.6](https://img.shields.io/badge/python-3.6-blue.svg)](https://www.python.org/downloads/release/python-360/)
[![PyPI](https://img.shields.io/pypi/v/elegantrl.svg)](https://pypi.org/project/elegantrl/)

<br/>
<a href="https://github.com/AI4Finance-LLC/ElegantRL" target="\_blank">
	<div align="center">
		<img src="figs/icon.jpg" width="40%"/>
	</div>
<!-- 	<div align="center"><caption>Slack Invitation Link</caption></div> -->
</a>
<br/>

ElegantRL is featured with **lightweight, efficient and stable**, for researchers and practitioners.
  
  + **Lightweight**: The core codes  <1,000 lines (check elegantrl/tutorial), using PyTorch (train), OpenAI Gym (env), NumPy, Matplotlib (plot).
  
  + **Efficient**: performance is comparable with [Ray RLlib](https://github.com/ray-project/ray).
  
  + **Stable**: as stable as [Stable Baseline 3](https://github.com/DLR-RM/stable-baselines3).

Currently, model-free deep reinforcement learning (DRL) algorithms: 
+ **DDPG, TD3, SAC, A2C, PPO, PPO(GAE)** for continuous actions
+ **DQN, DoubleDQN, D3QN** for discrete actions

For DRL algorithms, please check out the educational webpage [OpenAI Spinning Up](https://spinningup.openai.com/en/latest/). 

Check out the [ElegantRL documentation](https://elegantrl.readthedocs.io/en/latest/index.html).

# Table of Contents

+ [News](#News)
+ [File Structure](#File-Structure)
+ [Training Pipeline](#Training-Pipeline)
+ [Experimental Results](#Experimental-Results)
+ [Requirements](#Requirements)
+ [Model-free DRL Algorithms](#Model-free-DRL-Algorithms)

# News

+ [Towardsdatascience] [ElegantRL: A Lightweight and Stable Deep Reinforcement Learning Library](https://towardsdatascience.com/elegantrl-a-lightweight-and-stable-deep-reinforcement-learning-library-95cef5f3460b)
+ [ElegantRL: Mastering PPO Algorithms (Part I)](https://medium.com/@elegantrl/elegantrl-mastering-the-ppo-algorithm-part-i-9f36bc47b791)
+ [MLearning.ai] [ElegantRL Demo: Stock Trading Using DDPG (Part I)](https://elegantrl.medium.com/elegantrl-demo-stock-trading-using-ddpg-part-i-e77d7dc9d208)
+ [MLearning.ai] [ElegantRL Demo: Stock Trading Using DDPG (Part II)](https://medium.com/mlearning-ai/elegantrl-demo-stock-trading-using-ddpg-part-ii-d3d97e01999f)
# File Structure
![File_structure](https://github.com/Yonv1943/ElegantRL/blob/master/figs/File_structure.png)

   An agent in **agent.py** uses networks in **net.py** and is trained in **run.py** by interacting with an environment in **env.py**.

-----kernel file----
+ **elegantrl/net.py**    # Neural networks.
   + Q-Net,
   + Actor Network,
   + Critic Network, 
+ **elegantrl/agent.py**  # RL algorithms. 
   + AgentBase 
+ **elegantrl/run.py**    # run DEMO 1 ~ 4
   + Parameter initialization,
   + Training loop,
   + Evaluator.

-----utils file----
+ **elegantrl/envs/**    # gym env or custom env, including FinanceStockEnv.
   + **gym_utils.py**: A PreprocessEnv class for gym-environment modification.
   + **Stock_Trading_Env**: A self-created stock trading environment as an example for user customization.
+ **eRL_demo_BipedalWalker.ipynb**      # BipedalWalker-v2 in jupyter notebooks
+ **eRL_demos.ipynb**       # Demo 1~4 in jupyter notebooks. Tell you how to use tutorial version and advanced version.
+ **eRL_demo_SingleFilePPO.py**  # Use single file to train PPO, more simple than tutorial version
+ **eRL_demo_StockTrading.py**  # Stock Trading Application in jupyter notebooks

As a high-level overview, the relations among the files are as follows. Initialize an environment in **Env.py** and an agent in **Agent.py**. The agent is constructed with Actor and Critic networks in **Net.py**. In each training step in **Run.py**, the agent interacts with the environment, generating transitions that are stored into a Replay Buffer. Then, the agent fetches transitions from the Replay Buffer to train its networks. After each update, an evaluator evaluates the agent's performance and saves the agent if the performance is good.

# Training Pipeline

### Initialization:
+ hyper-parameters `args`.
+ `env = PreprocessEnv()` : creates an environment (in the OpenAI gym format).
+ `agent = agent.XXX()` : creates an agent for a DRL algorithm.
+ `evaluator = Evaluator()` : evaluates and stores the trained model.
+ `buffer = ReplayBuffer()` : stores the transitions.
### Then, the training process is controlled by a while-loop:
+ `agent.explore_env(…)`: the agent explores the environment within target steps, generates transitions, and stores them into the ReplayBuffer.
+ `agent.update_net(…)`: the agent uses a batch from the ReplayBuffer to update the network parameters.
+ `evaluator.evaluate_save(…)`: evaluates the agent's performance and keeps the trained model with the highest score.

The while-loop will terminate when the conditions are met, e.g., achieving a target score, maximum steps, or manually breaks.

# Experimental Results

Results using ElegantRL 

[LunarLanderContinuous-v2](https://gym.openai.com/envs/LunarLanderContinuous-v2/)

![LunarLanderTwinDelay3](https://github.com/Yonv1943/ElegantRL/blob/master/figs/LunarLanderTwinDelay3.gif)

[BipedalWalkerHardcore-v2](https://gym.openai.com/envs/BipedalWalkerHardcore-v2/)


<img src="https://github.com/Yonv1943/ElegantRL/blob/master/figs/BipedalWalkerHardcore-v2-total-668kb.gif" width="150" height="100"/>


BipedalWalkerHardcore is a difficult task in continuous action space. There are only a few RL implementations can reach the target reward.

Check out a video on bilibili: [Crack the BipedalWalkerHardcore-v2 with total reward 310 using IntelAC](https://www.bilibili.com/video/BV1wi4y187tC).

# Requirements

    Necessary:
    | Python 3.6+     |           
    | PyTorch 1.6+    |    

    Not necessary:
    | Numpy 1.18+     | For ReplayBuffer. Numpy will be installed along with PyTorch.
    | gym 0.17.0      | For env. Gym provides tutorial env for DRL training. (env.render() bug in gym==1.18 pyglet==1.6. Change to gym==1.17.0, pyglet==1.5)
    | pybullet 2.7+   | For env. We use PyBullet (free) as an alternative of MuJoCo (not free).
    | box2d-py 2.3.8  | For gym. Use pip install Box2D (instead of box2d-py)
    | matplotlib 3.2  | For plots. Evaluate the agent performance.
    
    pip3 install gym==1.17.0 pybullet Box2D matplotlib
