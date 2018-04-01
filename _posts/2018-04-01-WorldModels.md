---
layout : post
title : World Models - Schmidhuber, 1803
date : 2018-04-01
time : +2216
update : 2018-04-01
permalink : posts/Summary_WorldModels
tags : summary
mathjax: true
---

David Ha, Jürgen Schmidhuber [[PDF]](https://arxiv.org/abs/1803.10122)    

Check out [this brilliant blog](http://worldmodels.github.io/) accompanying the paper (IMO, this is how every paper should be presented from now on). 

---

## Motivation

- They train a large neural network to tackle RL tasks by dividing the agent into a large world model and a small controller model. 
    1. A large neural network to learn a model of the agent’s world in an unsupervised manner, 
    2. and then a smaller controller model to learn to perform a task using this world model. 
- A small controller lets the training algorithm focus on the credit assignment problem on a small search space, while not sacrificing capacity and expressiveness via the larger world model. By training the agent through the lens of its world model, the authors show that it can learn a highly compact policy to perform its task.

## Methodology

- The model in a nutshell
    + a visual sensory component that compresses what it sees into a small representative code
    + a memory component that makes predictions about future codes based on historical information
    + a decision-making component that decides what actions to take based only on the representations created by its vision and memory components.    
![The Model](/images/worldModel.svg)
- In some detail
    + C is deliberately made as simple and small as possible, and trained separately from V and M, so that most of the agent's complexity resides in the world model (V and M).
    + The authors choose the Covariance-Matrix Adaptation Evolution Strategy (CMA-ES) for training C.
    + the prediction of $$z_{t+1}$$​​ is not fed into the controller C directly — just the hidden state $$h_t​$$ and $$z_t$$.

## Experiments

### Car Racing (Car-v0)

- Model
    + The VAE is trained with a set of 10000 random roll-outs.
        * _No attention_!
    + The MDN-RNN is trained to model $$P(z_{t+1}\|a_t,z_t,h_t)$$
    + A linear controller is trained with CMA-ES to maximize the expected cumulative reward. 
- Note : The world model (V and M) has no knowledge about the actual reward signals in the environment. Only the Controller (C) Model has access to the reward information from the environment

- Takeaways

    + It is seen that the world model learns to reproduce the frames really well (remember, no attention)
    + Using both the V and M, they obtain SoTA results in this (simplistic) car domain! 
        * Traditional Deep RL methods often require pre-processing of each frame, such as employing edge-detection, in addition to stacking a few recent frames into the input.
        * Their world model takes in a stream of raw RGB pixel images and directly learns a spatial-temporal representation.  
    + The world model enables hallucinations, which means **the agent could possibly learn via its dreams...** Can we then transfer this policy back to the actual environment?

### VizDoom (Take Cover)

- Model
    + The only difference is that the model M is also supposed to predict $$done_t$$, i.e. whether the agent dies before the next action is taken (in the next time-step)
    + This enables M to be used as a Gym environment!
    + They add more uncertainty to the world by increasing the temperature parameter $$\tau$$ when sampling $$z_{t+1}$$.<sup>1</sup> This makes the environment more 'difficult', and helps the agent ameliorate the imperfections in the real world. 

- It works.
> ... our RNN-based world model is trained to mimic a complete game environment designed by human programmers. By learning only from raw image data collected from random episodes, it learns how to simulate the essential aspects of the game — such as the game logic, enemy behaviour, physics, and the 3D graphics rendering.

- Takeaways
    + The virtual model has an identical interface to the real one, so transfer is not an issue
    + The VAE is trained in a latent space...? _Noooooo_
 
## Thoughts

- While dreaming, the world model takes as input the action form the controller, and the controller itself has the hidden states of the model as input. Thus,  
    + the agent can essentially 'explore' ways to directly manipulate the hidden states of the game engine in its quest to maximize its expected cumulative reward.
    + it can also find a policy that looks good under our dynamics model, but will fail in the actual environment, usually because it visits states where the model is wrong because they are away from the training distribution.  
- Practical benefits of the approach
    + running computationally intensive game engines require using heavy compute resources for rendering the game states into image frames, or calculating physics not immediately relevant to the game. We may not want to waste cycles training an agent in the actual environment, but instead train the agent as many times as we want inside its simulated environment. 
    + Training agents in the real world is even more expensive, so world models that are trained incrementally to simulate reality will make it easier to experiment with different approaches for training our agents.
- Future work
    + VAEs trained in isolation don't have attention.    
    In their experiments, it reproduced unimportant detailed brick tile patterns on the side walls in VizDoom, but failed to reproduce task-relevant tiles on the road in the Car-v0.
        * _One could predict the reward as well to get over this, but then it makes the world model task-specific._      
        Personally, I don't mind the trade-off. It is okay to not have one model for every task out there in the world. The best way I see is to have a generic enough world model that can be finetuned for every task at hand by leveraging the domain knowledge available. There doesn't have to be one solution that fits all.
    + Explore the use of an unsupervised segmentation layer?    
    To extract better feature representations that might be more useful and interpretable compared to the representations learned using a VAE. _(check this out...)_
    + We may require higher capacity models or memory modules to prevent suffering from catastrophic forgetting in more complicated worlds.
    + Hierarchical breakup of the tasks. 

<sub>
    1 the MDN-RNN models a distribution of the possible outcomes in the actual environment. Even if the actual environment is deterministic, the MDN-RNN would in effect approximate it as a stochastic environment. This has the advantage of enabling training C inside a more stochastic version of any environment, with the parameter $$\tau$$ controlling the tradeoff between realism and exploitability.
</sub>