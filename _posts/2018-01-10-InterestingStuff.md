---
layout : post
title : Interesting AI tidbits
date : 2018-01-10
time : +2136
update : 2018-01-11
permalink : posts/AItidbits
summary : A collection of interesting AI tidbits.
<!-- published: false -->
---

Hello, world.   

Here are some interesting AI tidbits that I come across every other day.

<hr>
<hr>

<!-- - 29/1/2018    

[This great talk](http://www.iro.umontreal.ca/~bengioy/talks/COGSCI-23july2014.pdf) by Yoshua Bengio offers a great intuition of curriculum learning inspired from humans. He also goes on to propose how new ideas and cultures emerge over time, via the sharing of mid-level abstractions of the brain via the means of language. 
 -->
<hr>
<hr>

- 26/1/2018

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

<hr>
<hr>

- 21/1/2018    

Just realized that platooning, something I first heard in Tesla's semi-truck launch, was first implemented in/by UC Berkeley *[back in 1991](http://people.eecs.berkeley.edu/~varaiya/papers_ps.dir/SmartCars.pdf)*. Yes, autonomous platooning, wherein  vehicles travel at highway speeds with small inter-vehicle spacing to reduce congestion and still achieve high throughput without compromising on safety. This is a really attractive concept as it demonstrates improved efficiencies with relatively little change to the vehicles.    
Now that one thinks of it, it seems obvious that autonomous cars in (the near) future will communicate, cooperate, and form platoons over intersecting lengths of their commutes. 

<hr>
<hr>

- 11/1/2018

From a [2003 article](https://www.theguardian.com/science/2003/jul/03/research.science) in The Guardian: 

> At the Institute for Marine Mammal Studies in Mississippi, Kelly the dolphin has built up quite a reputation. All the dolphins at the institute are trained to hold onto any litter that falls into their pools until they see a trainer, when they can trade the litter for fish. In this way, the dolphins help to keep their pools clean.   
Kelly has taken this task one step further. When people drop paper into the water she hides it under a rock at the bottom of the pool. The next time a trainer passes, she goes down to the rock and tears off a piece of paper to give to the trainer. After a fish reward, she goes back down, tears off another piece of paper, gets another fish, and so on. This behaviour is interesting because it shows that Kelly has a sense of the future and delays gratification. She has realised that a big piece of paper gets the same reward as a small piece and so delivers only small pieces to keep the extra food coming. She has, in effect, trained the humans.   
Her cunning has not stopped there. One day, when a gull flew into her pool, she grabbed it, waited for the trainers and then gave it to them. It was a large bird and so the trainers gave her lots of fish. This seemed to give Kelly a new idea. The next time she was fed, instead of eating the last fish, she took it to the bottom of the pool and hid it under the rock where she had been hiding the paper. When no trainers were present, she brought the fish to the surface and used it to lure the gulls, which she would catch to get even more fish. After mastering this lucrative strategy, she taught her calf, who taught other calves, and so gull-baiting has become a hot game among the dolphins. 

A real-life example of what happens when humans are not able to define reward functions properly (a great example in the RL context is given by OpenAI [in this blog](https://blog.openai.com/faulty-reward-functions/), wherein they show that in a racing game, the agent learns to turn circles to repeatedly pick the mini-rewards instead of the (relatively small) big reward for finishing the race!)    <br>

<hr>
<hr>

- 10/1/18

From a recent NY Times article titled [Leave A.I. Alone](https://www.nytimes.com/2018/01/04/opinion/leave-artificial-intelligence.html?smid=tw-share):

> ... the recent report released by the AI Now Institute ... acknowledges that no commonly accepted definition of A.I. exists, which it describes loosely as "a broad assemblage of technologies ... that have traditionally relied on human capacities."    
> "Artificial intelligence" is all too frequently used as a shorthand for software that simply does what humans used to do. But replacing human activity is precisely what new technologies accomplish — spears replaced clubs, wheels replaced feet, the printing press replaced scribes, and so on. What’s new about A.I. is that this technology isn’t simply replacing human activities, external to our bodies; it’s also replacing human decision-making, inside our minds.

My mindset aligns perfectly with these thoughts. I believe [Elon Musk](https://www.theguardian.com/technology/2017/aug/14/elon-musk-ai-vastly-more-risky-north-korea) and [Stephen Hawking](https://www.usatoday.com/story/tech/talkingtech/2017/11/07/hawking-ai-could-worst-event-history-our-civilization/839298001/) are possibly inadvertently creating a fear of a Hollywood-esque AI-apocalypse in the minds of the general public, who hang on to words of visionaries like those. Yes, I agree there should some regulations on what and where and to what extent AI should or can be incorporated in our daily lives, most notably in critical areas like defense and nuclear safety, but as [Andrew NG puts it brilliantly](https://www.theregister.co.uk/2015/03/19/andrew_ng_baidu_ai/):

> "There's a big difference between intelligence and sentience. There could be a race of killer robots in the far future, but I don’t work on not turning AI evil today for the same reason I don't worry about the problem of overpopulation on the planet Mars.    
If we colonize Mars, there could be too many people there, which would be a serious pressing issue. But there's no point working on it right now, and that's why I can’t productively work on not turning AI evil." 

Go AI!    
