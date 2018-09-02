---
layout : post
title : Beyond Reward - The Problem of Knowledge and Data
date : 2018-09-02
time : +1619
permalink : posts/Knowledge_BeyondRewards
tags : summary
---

Richard Sutton [[PDF](http://ilp11.doc.ic.ac.uk/short_papers/Sutton_ILP11_abstract.pdf)]

---

### Motivation / Research Question

> Intelligence can be defined, informally, as knowing a lot and being able to use that knowledge flexibly to achieve one's goals. In this sense it is clear that knowledge is central to intelligence.

Hence one of the questions that the paper explores is how this knowledge can be learned and maintained autonomously by intelligent systems.     

### Idea

Traditional methods rely on people to maintain correctness of said knowledge, supplemented by checks for internal consistency. This leverages existing human expertise, but cannot scale to large knowledge bases due to reliance on humans. An additional feature/limitation(?) of this _public-knowledge approach_ is that it has to be universal, objective, abstract, and public for humans to validate.     
On the other hand, if a system could correct and learn its knowledge base from the routine ordinary data available to it, one could scale to enormous knowledge bases. 'Knowledge' in this _sensorimotor approach_ need not be accessible to other observers in a useful way, and hence it is private, personal, and subjective. 

1. To address this mismatch in types of data, 
    - one could explicitly introduce _spacial abstractions_ (clumps of these states represent a 'room' - a higher level concept). 
    - or represent knowledge through _temporal abstractions_ - some predictions or outcomes from visual inputs of temporally extended procedures (much like the initiation set of options).
2. Now to learn knowledge of this procedure-prediction form, algorithms like SARSA, Q-learning fail when experience comes in incomplete trajectories (not played to completion).

### Evidence

- Gradient-TD methods appear to make learning temporally-abstract knowledge from sensorimotor data practical.
- A robot learning thousands of non-trivial, temporally-extended facts about its sensorimotor interface in real time without human training or supervision has also been demonstrated (Sutton's Horde - [my summary](Horde)).

### Conclusions / Thoughts / Limitations

Read up on:
1. Why convergence guarantees of TD-learning algorithms go not hold with incomplete experiences.
2. Horde, in context of the above.
3. Options, in context of the above. 