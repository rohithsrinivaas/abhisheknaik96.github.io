---
layout : post
title : On the surprising challenges that FAD technology faces.
date : 2018-03-22
time : +2245
update : 2018-03-22
permalink : posts/OpEd_FAD_challenges
tags : oped
oped : true
---

With the recent [fatal Uber crash](OpEd_Uber), I was reading [this report](https://onlinelibrary.wiley.com/doi/abs/10.1002/rob.20266) documenting a low-speed crash between two autonomous vehicles in the DARPA challenge, which highlights the surprising but crucial (and arguably blatant) challenges that even the state-of-the-art FAD technology faces today:

### 1. Clusters of Static Objects    

Often, a design choice is made - classification into static objects or moving obstacles. If it is the latter, forward predictions are made for the object. 
> Because the software treated Skynet as a collection of static objects instead of a moving obstacle, no forward prediction was made on Skynet's motion. Talos was driving between a lane constraint on the left and a collection of static objects on the right. If Skynet had not moved, Talos would have negotiated the gap just as it had to avoid K-rails and other static objects adjacent to the lanes throughout the day. Instead, unexpectedly for Talos, Skynet moved forward into the planned path of Talos. Without a forward-motion prediction of Skynet, by the time Skynet was in Talos’s path, Talos was unable to come to a stop before colliding.    

Maybe if a explicit vehicle classification was performed over the deployed moving-obstacle model, this could have been avoided.

### 2. Inability to Track Slow-Moving Objects    

- When the vehicle is moving, the visible portion of other obstacles can change, and the motion of the visible portion of the obstacle is difficult to distinguish from an obstacle that is actually moving. 
- As I realized while researching [the Tesla crash](OpEd_Tesla) earlier, there is already a problem of ignoring static 'moving' objects like traffic signs or billboards relative to the moving car.   

### 3. Inability to Detect Objects that are Too Close     

Remember the black hole around the cars beyond which all the concentric circles of world maps are shown in visualizations of what self-driving cars 'see' in all those videos? Yeah, LiDARs cannot (reliably) detect objects in close range. 

### 4. Negotiation by Intent Recognition

Drivers on the road constantly anticipate the potential  actions  of  fellow  drivers. For close maneuvering in car parks and intersections, for example, eye contact is made to ensure a shared  understanding. DARPA stated that traffic vehicle drivers, unnerved by being unable to make eye contactwith  the robots, had resorted to watching the front wheels of the robots for an  indication of their intent. As intervehicle communication becomes ubiquitous and reliable (Hello 5G!), autonomous vehicles will be able to transmit their intent to neighboring vehicles to implement the level of coordination beyond what human drivers currently achieve using eye contact.

### 5. Re-engaging the driver
Till the last Level-5 of autonomy, self-driving require a human driver to be present at the wheel, to take over in a jiffy when called for. Now we have to take into account that the human in question will lack situational awareness at such points, since they could potentially be engaging in activities like reading or texting. As drivers reengage, they must immediately evaluate their surroundings, determine the vehicle’s place in them, analyze the danger, and decide on a safe course of action.  
As [this insightful article by McKinsey](https://www.mckinsey.com/industries/automotive-and-assembly/our-insights/self-driving-car-technology-when-will-the-robots-hit-the-road) puts it:
> At 65 miles an hour, cars take less than four seconds to travel the length of a football field, and the longer a driver remains disengaged from driving, the longer the reengagement process could take. Automotive companies must develop a better human–machine interface to ensure that the new technologies save lives rather than contributing to more accidents.
