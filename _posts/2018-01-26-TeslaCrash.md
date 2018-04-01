---
layout : post
title : On the latest Tesla crash and an interesting revelation.
date : 2018-01-26
time : +1445
permalink : posts/OpEd_Tesla
tags : oped
---

So there was [another Tesla-crash](https://twitter.com/CC_Firefighters/status/955529991319560192) this week. Into a big, red, stationary fire-truck. What?! How can state-of-the-art autonomous driving technology not avoid something as obviously blatant as that?!    
Someone dug into Tesla's manual to find this:

> "Traffic-Aware Cruise Control cannot detect all objects and may not brake/decelerate for stationary vehicles, especially in situations when you are driving over 50 mph (80 km/h) and a vehicle you are following moves out of your driving path and a stationary vehicle or object is in front of you instead."    

Now this clearly sounds like a glaring flaw, the kind of an obvious, horrible mistake that engineers should be racing to eliminate. No, how are we even allowing such systems to be deployed on our roads?     
The answer is - without this 'ignorance', RADARs wouldn't work at all. According to [this WIRED article](https://www.wired.com/story/tesla-autopilot-why-crash-radar/):
> ...it also detects lots of things a car rolling down the highway needn't worry about, like overhead highway signs, loose hubcaps, or speed limit signs. So engineers make a choice, telling the car to ignore these things and keep its eyes on the other cars on the road: They program the system to focus on the stuff that's moving."     

If you ask me, I don't understand why sensor-fusion isn't working here - why aren't the RGB and depth cameras able to see an approaching (stationary or otherwise) big, red firetruck and veto the RADAR inputs into applying brakes?   
Oh well, while we fix the sensor-fusion (and their weightages<sup>1</sup>), we're stuck with a flawed system, the result of a compromise made to navigate the world at speed. And when even the best systems available can't see a big red big firetruck, it's a stark reminder of how long and winding the path to autonomy actually is.

Source : [WIRED](https://www.wired.com/story/tesla-autopilot-why-crash-radar/).

<sub>
1 Are those learnt as well?
</sub>