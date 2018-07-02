---
layout : post
title : Procedural Level Generation Improves Generality of DRL
date : 2018-07-01
time : +1911
permalink : posts/PPCG_DRLgeneralization
tags : summary
---

[[arXiv;1806.10729](https://arxiv.org/abs/1806.10729)]
---

### What?

![Socially Acceptable](/images/sociallyAcceptable.png)

This paper does a great job at calling out this questionable practice in RL.
> When it is reported that an agent has learned to play a game, it may simply mean that the policy model is memorizing what action to take for a large number of observations that it has seen over and over again. This boils down to the network compressing a large number of image and action pairs without the network ever learning the general concepts of the game. 

- It is seen that RL agents perform decently in the environment that they are tested on, and generally show unbelievably low performance on even related tasks, let alone significantly different ones. This is the motivation for transfer learning, and one of the goals of AGI.
- The authors attempt to learn generalized behaviour by training the agent to learn on a newly-generated level of the game after every episode!
- Leveraging existing methods for generating levels with varying difficulty, their approach learned policies that generalize well to other procedurally generated levels, even with some variation in difficulties.

### How?

- They use the GVG-AI platform, which uses a high-level video game description language (VGDL) to allow for rapid development of new games!
- They design game-specific level generators. For instance, for the Boulderdash initializes a new layout for a level, add the player at a random location, adds the exit door (a location that is further away from the player has a higher probability of being selected), adds gems at random locations in the map, etc.
- The agent controls the level of difficulty of the levels with a parameter $$\alpha \in [0,1]$$. With $$\alpha$$ close to 1, the levels are bigger and more 'difficult'. _The main idea is that the difficulty is increased by 0.01 for a win and decreased by the same amount for a loss_.
- They train A2C (from OpenAI baselines) and compare against:
    1. An agent trained on a single human-generated level.
    2. Agents trained on levels randomly sampled from 0 to 3.
    3. PCG agent trained on levels on constant difficulty.


### Results

- Agents trained on a single level overfit - getting a high score on the same level, and extremely poor scores on other levels.
- Agents trained on randomly sampled levels of different difficulties perform decently on the test (unseen) levels, but there is a lot of room for improvement.
- PPCG achieves respectable scores on all sets of levels.
- Notably, PPCG agents do not generalize as well on human-generated levels
> In Zelda, the agent may never learn to properly navigate around walls and kill enemies at certain positions. If a truly general understanding of the game mechanics was learned, the agent would be able to recognize when two frames were semantically similar and take proper actions even on new types of level layouts. It is clear that our approach, in contrast, still only generalizes to a subset of levels.
- Furthermore, PPCG agents are seen to show the behaviour of catastrophic forgetting on levels of lower difficulties as training progresses.

As future work, it needs to be ensured that the level generator covers a sufficiently large space of levels in order to learn proper general understanding of the game.
    
## Notes

- While GVG-AI originally provides a forward model that allows agents to use search algorithms, the GVG-AI Gym only provides the pixels of each frame, the incremental reward, and whether the game is won or lost
