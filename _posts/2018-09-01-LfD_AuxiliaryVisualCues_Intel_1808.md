---
layout : post
title : Learning End-to-End Autonomous Driving using Guided Auxiliary Supervision
date : 2018-09-01
time : +1248
permalink : posts/LfDAuxiliaryTasks
tags : summary
---

[[arXiv](https://arxiv.org/abs/1808.10393); Intel]

---

The authors propose a LfD framework (pure DL; no RL) to learn expert demonstrations with auxiliary predictions of visual affordances (like distances to cars) and action primitives.
They claim that such joint learning and supervised guidance facilitates hierarchical task decomposition, assisting the agent to learn faster, achieve better driving performance and increases transparency of the otherwise black-box end-to-end network. The simulator used is CARLA.

### Motivation

Humans apply a rich hierarchical task decomposition pipeline while carrying out visuomotor tasks like driving. Rather than arriving at the exact throttle, brake and steering percentages directly from high-dimensional pixel values, our brains first decompose the image into recognizable objects and their intuitive states in the scene, then use visual abstractions along with past experience to come up with approximately optimal high level decisions or action primitives for the given state, and lastly use these action primitives to arrive at the exact motor commands to be executed.    

Such kind of hierarchical structure allows us to tackle hard problems like driving by decomposing them into multiple sub-tasks allowing for better generalization. 

The authors propose, as auxiliary sub-tasks : 
1. visual affordances - to form abstract description of the visual scene in front of the vehicle (distances to other cars, pedestrians, turns, etc)
2. action primitives - to provide an abstract description of the possible high-level actions required for driving (speed up, cut into lane, turn left) 

Benefits:
1. Infusion of a rich prior that assists the final prediction task instead of expecting the network to learn all relevant knowledge from scratch. ~~Catch : It's a humongous job to expect human demonstrators to sit and annotate every frame with ALL of primitives.~~ They don't do that. Check 'Data Collection' below.
2. Since the network is forced to predict these affordances and primitives, it makes black-box DL a little more transparent in terms of what decisions were made... 

### Notes
- To overcome the distribution-mismatch problem, and hence covariate shift, they employ Laskey et al. (2017)'s noise injection method, by inducing noise in the agent during demonstration to force it to visit novel states and improve the demonstration distribution.
- Instead of appending the speed and planner input (more on that shortly) to the ResNet output of the visual input, they use them to (separately) learn a soft attention on the high-dimension image feature vector, with the union of the attended features being used to predict the auxiliary tasks. Finally, a double-attention unit using these visual cues and action primitives.
- Data collection:
    - Done by human driver who follows the sequence of commands given by an A* planner, amounting to 7 hours of human driving. 
    - A couple of the Visual Cues are obtained from CARLA, all others need to be computed. 
    - The Action Primitives are derived using fixed rules to classify the demonstrated steering, brake and throttle control values.
 
### Takeaways 
Good promising ideas (auxiliary sub-tasks, attention, A*-usage) but unrealistic to implement this end-to-end DL solution _to drive faithfully in highly stochastic urban settings_, as they call it. 
- Using speed and high-level intent seems too brittle alone to decide for attention on the visual features. Since they also claim transparency, I would like to see what visual cues were attended to when making a turn on an intersection, say. 
- Needless to say, truckloads of will be required for this to go anywhere. While the noise injection is a start, among other things, they haven't even touched diferent types of day and weather conditions, something CARLA can be used for.  
