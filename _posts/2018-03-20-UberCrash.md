---
layout : post
title : On the fatal Uber crash :/
date : 2018-03-20
time : +0142
update : 2018-03-28
permalink : posts/OpEd_Uber
tags : oped
oped : true
---

So an Uber in the autonomous mode [killed a woman](https://www.nytimes.com/2018/03/19/technology/uber-driverless-fatality.html) crossing a street a few hours ago, _while there was a human safety driver at the wheel_.     
The first things that come to mind:
- Assuming the woman wasn't jaywalking, at what speed was the Uber crossing the intersection anyway to kill her.
- Secondly, what was the 'safety human driver' doing? Isn't his/her job description to precisely do this one thing of not bumping the car into anyone or anything?!    

Oh well, let's wait for Uber to make a statement about the technical details. It might take a while because they'll want to be 101% sure of what they're reporting, since their, and pretty much all the autonomous driving players' necks hang in balance now...    

<br/>
_Update (22/3/18)_    

So the interior and exterior camera footage [was released](http://www.foxnews.com/us/2018/03/22/dashcam-video-deadly-self-driving-uber-crash-released.html) by the Tempe Police Department. Thoughts:
- IMHO, it would have been extremely difficult for a human driver to avoid that accident, with the low visibility compounded by the jaywalking. But aren't such kinds of situations exactly where autonomous vehicles supposed to perform better than humans, with their armory of sensors for a richer-than-human sensory input? :/
- Secondly, why release such disturbing footage to the general public during an ongoing investigation? It would have been better had they released this a few days later with a juxtaposedcommentary or annotation on what exactly was going on and what went wrong...   

Anyway, I'm really interested to know how the LiDAR/RADAR input was being handled by the Uber inm the seconds leading to the crash...   

<br/>
_Update (28/3/18)_   

So Intel/Mobileye just took a pot-shot at Uber. CEO Amnon Shashua and his team ran their advanced driver assistance systems (ADAS) _on a video of the dashcam video_ and reported clear detection _approximately one second before impact_, using "pattern recognition" and a "free-space" detection module. In his words:

> Recent developments in artificial intelligence, like deep neural networks, have led many to believe that it is now easy to develop a highly accurate object detection system and that the decade-plus experience of incumbent computer vision experts should be discounted. This dynamic has led to many new entrants in the field. While these techniques are helpful, the legacy of identifying and closing hundreds of corner cases, annotating data sets of tens of millions of miles, and going through challenging preproduction validation tests on dozens of production ADAS programs, cannot be skipped. **Experience counts, particularly in safety-critical areas**.    

Professor Amnon also reports that at Mobileye, they obtain true redundancy by building a separate end-to-end camera-only system and a separate LIDAR and radar-only system.

