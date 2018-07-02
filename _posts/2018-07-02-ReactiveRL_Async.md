---
layout : post
title : Reactive RL in Asynchronous Environments
date : 2018-07-02
time : +1302
permalink : posts/ReactiveRL_Async
tags : summary
---

Jaden Travnik, Kory W. Mathewson, **Richard S. Sutton**, and Patrick M.
Pilarski [[arXiv;1802.06139](https://arxiv.org/abs/1802.06139)] 

---

### What?

- In asynchronous environments, the state of the environment may change during the agents' computation (acting, observing, choosing an action, learning). If the agent's reaction time is too long, its chosen action may become inappropriate in the now changed environment.  (Read Section 2 of the paper for a detailed Hallway World example)
- The authors propose a class of reactive reinforcement learning algorithms that address this problem by immediately acting after observing new state information.


### Before this?

- This problem was observed earlier and resulted in the introduction of online and parallel architectures to reduce the reaction time. 
- But such methods are computationally demanding. The authors' proposal works even in resource-constrained systems.


### Okay, how?

- Simple : _take actions immediately after choosing them given the most recent observation_.
![Reactive SARSA](/images/reactiveSARSA.png)
- The learning step contributes most significantly to the time-delay/reaction-time. 


### Results

- Check out the robot-arm setup in Section 4 of the paper.
- It was observed that as the time-delay increases, both the algorithms' performances suffered - standard SARSA performed worse with large variability. 
- Reactive SARSA had approximately the same number of failed stops as the optimal control strategy whereas the standard SARSA performed significantly worse with more than four times as many failed stops.

### What next?

- One alternate means of addressing delay-induced performance concerns may be to create a dedicated thread for each of the RL algorithm components.
    - Then again, multi-threading is difficult on single-core embedded systems, in which case, the proposed strategy is enough.
- A bolder claim is of allowing the agent to learn an optimal ordering of its learning protocol or to interrupt learning components for more pressing computations.
    - The motivation for this is expert video-game players or musicians, who perform without actually observing the consequences of their actions (as if in a trance), since they know the task and their abilities so well.
    - **In the FAD context,  an agent may be able to find an optimal ordering of its learning protocol or to interrupt long lasting computations (e.g., analyzing an image) for more pressing computations (e.g., avoiding a pedestrian)**

## Notes

- Notably, in discrete-time synchronous tasks, Reactive SARSA learns the same optimal policy as SARSA, in the same manner. This is because in both algorithms, the first 2 actions are selected using the initial policy. In each subsequent step $$t$$, actions are chosen using the policy learned on the last step, and the policy updates happen with identical experiences.    

- Read :
    1. Parallel On-Line Temporal Difference Learning for Motor Control. Caarls, W., and Schuitema, E. (2015)
    2. RTMBA: A Real-Time Model-Based Reinforcement Learning Architecture for Robot Control. Hester, T., M. Quinlan, and Stone, P. (2012)

