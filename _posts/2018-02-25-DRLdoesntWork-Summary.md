---
layout : post
title : Summary - DRL doesn't work, yet.
date : 2018-02-25
time : +1617
permalink : posts/DRLisHard
tags : summary
---

This post is a summary of the extremely insightful blog - [Deep Reinforcement Learning Doesn't Work Yet](https://www.alexirpan.com/2018/02/14/rl-hard.html) - that is making the rounding on twitter since it came out a couple of weeks ago.

---

## Training times are mind-numbingly large. 
- ATARI : Average >20 million frames (~100 hours of continuous play, for which humans take a few minutes to master.
- MuJoCo : 10<sup>5</sup>-10<sup>7</sup> steps. DeepMind's parkour suite results, though super-cool, took 6400 CPU hours.
- **Takeaway** : DRL is still orders of magnitude above a practical level of sample efficiency.\s\s 

## Classical control-theory methods obliterate DRL in many tasks in terms of final performance
- [Here is a video](https://www.youtube.com/watch?v=uRVAX_sFT24) of the MuJoCo robots, controlled with online trajectory optimization. Correct actions are computed in near real-time, online, with no offline training. Oh, and it's running on 2012 hardware. :P :P
	- Does that mean all our lives have been lies? Not really. [Tassa et al, IROS 2012](https://homes.cs.washington.edu/~todorov/papers/TassaIROS12.pdf) use model predictive control, which gets to perform planning against a ground-truth world model (the physics simulator). Model-free RL doesn't do this planning, and therefore has a much harder job. But one could argue, if planning against a model helps this much, why even bother with the bells and whistles of training an RL policy? 
- In the same vein, off-the-shelf Monte Carlo Tree Search outperforms the standard DQN by a large margin. Again, this is not a fair comparison MCTS gets to perform search against a ground truth model (the Atari emulator). But then again, sometimes you just want things to work.
- The rule-of-thumb is that except in rare cases, domain-specific algorithms work faster and better than reinforcement learning. Which is why AlphaGo was such a big win for DRL, because unambiguous wins like these don't happen very often.
- Similarly, [Boston Dynamics](https://www.youtube.com/channel/UC7vVhkEfw4nOGp8TyDk7RcQ) **does not use any RL**. Yeah. Just classical robotics techniques.

## RL requires reward functions, which are (1) difficult to design, (2) hard to get to work
- For RL to do the right thing, your reward function must capture exactly what you want. 
- If it doesn't, the agent might end up learning behaviors [completely](https://youtu.be/8QnD8ZM0YCo?t=25) [different](https://www.youtube.com/watch?v=tlOIHko8ySg) than what was expected, because the agent blindly optimizes to maximize the obtained reward.
- One way of addressing this is to make the reward sparse, giving a positive reward *only* when the final goal is achieved. This works well for certain (rather simple) tasks, but in tasks like navigating a maze, or dribbling to score a goal, this sparse 
- The other way is reward-shaping (aka reward-hacking), wherein we put all our domain knowledge to use to come up with these Byzantine reward functions from hell which need an eternity to tune in order to get the 'expected' behavior.    
- Even so, one could end up in local minimas that are hard to escape from (i.e. unintended behavior). This is a result of the classic exploration-exploitation dilemma - if your current policy explores too much you get junk data and learn nothing; exploit too much and you 'burn-in' behaviors that aren't optimal.
- **Takeaway**:
> I’ve taken to imagining deep RL as a demon that’s deliberately misinterpreting your reward and actively searching for the laziest possible local optima. It’s a bit ridiculous, but I’ve found it’s actually a productive mindset to have.    

## When DRL works, it has generally overfit.
- As [this tweet](https://twitter.com/jacobandreas/status/924356906344267776) rightly points out:
> Deep RL is popular because it’s the only area in ML where it’s socially acceptable to train on the test set.
- The generalization capabilities of deep RL aren't strong enough to handle a diverse set of tasks yet.
- Very few target generalisability (aka transfer). Most results with superb performance (superhuman or otherwise) can do so because they've overfit like crazy
- This seems to be a running theme in multiagent RL. When agents are trained against one another, a kind of co-evolution happens. The agents get really good at beating each other, but when they get deployed against an unseen player, performance drops. 
- The author goes on to say - _if your agents are learning at the same pace, they can continually challenge each other and speed up each other’s learning, but if one of them learns much faster, it exploits the weaker player too much and overfits. As you relax from symmetric self-play to general multiagent settings, it gets harder to ensure learning happens at the same speed._

## Okay, suppose it works. But now it's unstable and irreproducible.
- Supervised learning is stable. If you change the hyperparameters a little bit, your performance won’t change that much. In DRL, on the other hand, you might be happy with a model which works 70% of the time. 
- While a 30% failure rate implies a bug in supervised learning, it DRL, you have no idea whether it’s a bug, if your hyperparameters are bad, or if you simply got unlucky...
- Random chance is a big player in DRL. And the only way to address it is by throwing enough experiments at the problem to drown out the noise.
- _When your training algorithm is both sample inefficient and unstable, it heavily slows down your rate of productive research._
- **Takeaway** : RL is very sensitive to both initialization and to the dynamics of the training process, because your data is always collected online and the only supervision you get is a single scalar for reward. A policy that randomly stumbles onto good training examples will bootstrap itself much faster than a policy that doesn’t. A policy that fails to discover good training examples in time will collapse towards learning nothing at all, as it becomes more confident that any deviation it tries will fail.

## Where do we actually see DRL in real life?
- DQN, learning from raw pixels to play a large suite of ATARI games, was a breakthrough. AlphaGo and AlphaZero are brilliant achievements as well. 
- But what about deployment in real life (that we know of for sure)?
	- Recommendation systems still use Contextual Bandits. [[source](https://research.yahoo.com/publications/5863/contextual-bandit-approach-personalized-news-article-recommendation)]
	- Google uses it for reducing data center power usage. [[source](https://deepmind.com/blog/deepmind-ai-reduces-google-data-centre-cooling-bill-40/)]
	- OpenAI uses it to play DOTA 2, in a limited 1v1 setting, with only the _Shadow Fiend_. Many would not call this a real-world application, but game-playing is something close to my heart so this ends up here.
- Other rumored works include Audi's self-driving RC cars, Facebook's chatbots, financial models (trading, etc), but there's no definite proof.
- [Libratus (Brown et al, IJCAI 2017)](https://www.ijcai.org/proceedings/2017/0772.pdf) and [DeepStack (Moravčík et al, 2017)](https://arxiv.org/abs/1701.01724) **do not use RL**, but counterfactual regret minimization and clever iterative solving of subgames... #Mythbuster
- **Takeaway** : 
> _... either deep RL is still a research topic that isn’t robust enough for widespread use, or it’s usable and the people who’ve gotten it to work aren’t publicizing it. The former is more likely._

## Ohkay... So will DRL ever work?
- Yes, when some (hopefully, more) of the following conditions are satisfied:
	- When we have great simulators for generating humongous amounts of experience.
	- When we can break the problem down into (hopefully easier) sub-tasks that can be solved with RL.
	- When we have a clean way to define a learnable, ungameable reward.

## Concluding remarks

**Takeaway**: DRL has a long way to go before becoming a plug-and-play technology. So while one could be annoyed with the current state of affairs in DRL (wherein everything is seemingly in shambles), one should also believe in where it could be. As Andrew Ng says, 
> [there is] a lot of short-term pessimism, balanced by even more long-term optimism.   

 Go RL!