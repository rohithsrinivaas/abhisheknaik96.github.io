---
layout : post
title : On Variance
date : 2018-11-24
time : +0015
permalink : posts/SampleVariance
tags : general
summary : Why is the Bessel's correction required?
---


Back when I first studied machine learning, I came across the following formula for computing variance of a sampled distribution :


$$ \sigma^2 = \frac{1}{N-1} \sum_{i=1}^N (x_i - \bar{x})^2 $$ 


where $$x_i$$ is the $$i^{th}$$ sample from the distribution over $$x$$, $$N$$ is the total number of samples, and $$\bar{x}$$ is the sample mean. This was called the _unbiased estimate of the variance_.

<br>
Why? Intuitively, shouldn't the denominator be $$N$$, since $$ \sigma^2 = \mathbb{E}[(x-\bar{x})^2] $$? 

<br>
That's where I was mistaken. And now that I've had time to think about it, it's actually quite elegant.

<br>
Variance is defined as :
> _the expectation of the squared deviation of a random variable from its mean_. 

The 'mean' there, that is the _true_ mean of the distribution of the random variable. $$\bar{x}$$, on the other hand, is the _sampled_ mean. Which means every time we compute $(x_i - \bar{x})$, we're off by a factor of  $$(x_i-\mu)$$, where $$\mu$$ is the _true_ mean of the random variable. 

<br>
Alright, so something is off. But by how much? And how did $N$ get replaced exactly by $N-1$? Time to dive into some math...

Let's call the _unbiased_ variance as $$\sigma_{true}$$ and the _biased_ variance as $$\sigma_{biased}$$. Now,
$$ \begin{aligned}
\sigma_{true}^2 &= \mathbb{E}[(x-\mu)^2] \\
&= \mathbb{E}[(x-\bar{x} + \bar{x} - \mu)^2] \\
&= \mathbb{E}[(x-\bar{x})^2 + (\bar{x} - \mu)^2 + 2(x-\bar{x})(\bar{x}-\mu)] \\
&= \mathbb{E}[(x-\bar{x})^2] + \mathbb{E}[(\bar{x} - \mu)^2] + 2\mathbb{E}[(x-\bar{x})(\bar{x}-\mu)]\\
&= \sigma_{biased}^2 +  \mathbb{E}[(\bar{x} - \mu)^2] + 2(\bar{x}-\mu)\mathbb{E}[(x-\bar{x})]\\
\sigma_{true}^2 &= \sigma_{biased}^2 +  \mathbb{E}[(\bar{x} - \mu)^2] \\
\end{aligned}$$

<br>
Alright now. We have established that our biased estimate is _smaller_ than the true estimate (Why? Because the second term on the RHS is the expectation of a squared quantity). Let us compute that quantity.

$$\begin{aligned} 
\mathbb{E}[(\bar{x} - \mu)^2] &= Var(\bar{x}) \\
&= Var(\frac{1}{N}\sum_{i=1}^N x_i) \\
&= \frac{1}{N^2} Var(\sum_{i=1}^N x_i) \qquad \big( Var(aY) = a^2 Var(Y) \big) \\
&= \frac{1}{N^2} \big( Var(x_1) + Var(x_2) \ldots + Var(x_N) \big) \\
\mathbb{E}[(\bar{x} - \mu)^2] &= \frac{\sigma_{true}^2}{N} \qquad \big(Var(X+Y) = Var(X) + Var(Y) \text{ if X,Y are uncorrelated}\big)
\end{aligned} $$

<br>
In the last step, we could use that property since all the $$x_i$$s are independently sampled, and hence uncorrelated. Thus, we have 

$$ \begin{aligned}
\sigma_{true}^2 &= \sigma_{biased}^2 + \frac{\sigma_{true}^2}{N}\\
\sigma_{true}^2 &= \frac{N}{N-1} \sigma_{biased}^2 \\
\sigma_{true}^2 &= \frac{N}{N-1} \big( \frac{1}{N}\sum_{i=1}^N(x_i - \bar{x})^2 \big) \\
\sigma_{true}^2 &= \frac{1}{N-1}\sum_{i=1}^N(x_i - \bar{x})^2
\end{aligned} $$

There we go, the familiar variance of a sampled distribution.

Doesn't it feel great to prove something to yourself rather than take someone's word for it? :)



### Parting notes
- The use of $$N-1$$ instead of $$N$$ in that formula is called the [Bessel's correction](https://en.wikipedia.org/wiki/Bessel's_correction).
- I'm not sure what or who to attribute the definition of sample variance to, so [here's](https://en.wikipedia.org/wiki/Variance) the Wikipedia link.   
- Check out [this link](https://www.stat.cmu.edu/~cshalizi/uADA/13/reminders/uncorrelated-vs-independent.pdf) for a crisp refresher on independent vs uncorrelated variables (*TL;DR* - independent variables are uncorrelated; uncorrelated variables may not be independent)
