---
layout : post
title : On the Bellman Equation
date : 2019-07-24
time : 1855
permalink : posts/BellmanEquation
tags : general
summary : The derivation of the Bellman equation is ... subtle.
---


Back when I first read [the RL textbook](http://www.incompleteideas.net/book/the-book.html), the Bellman Equation for the _discounted_ value function was introduced as:

![Eq 3.14 from Pg 46](/images/BellmanEquation_book.png)

Now, I felt there was a big jump from the third equation to the fourth, so recently I came back to it. Turns out, the derivation from that third equation the the fourth was quite significant! So let's break it down:

$$ \begin{aligned}
v_{\pi}(s) &= \mathbb{E}[G_t | S_t = s]\\
&= \mathbb{E}[R_{t+1} + \gamma G_{t+1}| S_t = s]\\
&= \mathbb{E}[R_{t+1}|S_t=s] + \gamma \mathbb{E}[G_{t+1}| S_t = s] \qquad (1)
\end{aligned} $$

Before we expand these two terms, let us revisit some basic probability.

$$ \begin{aligned}
\mathbb{E}[A] = \mathbb{E}\big[\mathbb{E}[A|B]\big]
\end{aligned} $$

Why is this true? Let's expand the RHS.

$$ \begin{aligned} \mathbb{E}\big[\mathbb{E}[A|B]\big] &= \sum_{b\in\mathcal{B}} p(B=b)\;\mathbb{E}[A|B=b] \\
&= \sum_{b\in\mathcal{B}} p(B=b) \sum_{a\in\mathcal{A}}a\;p(A=a|B=b)\\
&= \sum_{b\in\mathcal{B}}  \sum_{a\in\mathcal{A}}a\;p(B=b)\;p(A=a|B=b)\\
&= \sum_{b\in\mathcal{B}} \sum_{a\in\mathcal{A}}a\;p(A=a,B=b)\\
&= \sum_{a\in\mathcal{A}}a\;\sum_{b\in\mathcal{B}}p(A=a,B=b)\\
&= \sum_{a\in\mathcal{A}}a\;p(A=a)\\
&=\mathbb{E}[A]
\end{aligned} $$

What we did here is condition over another random variable and marginalize over it. Keeping this in mind, let us go back to the first term on the RHS in equation 1.

$$ \begin{aligned}
\mathbb{E}[R_{t+1}|S_t=s] &= \mathbb{E}\big[\mathbb{E}[R_{t+1}|S_t=s,A_t=a]\big]\\
&= \sum_{a}p(A_t=a|S_t=s)\;\mathbb{E}[R_{t+1}|S_t=s,A_t=a]\\
&= \sum_{a}\pi(a|s)\;\mathbb{E}[R_{t+1}|S_t=s,A_t=a]\\
&= \sum_{a}\pi(a|s)\;\mathbb{E}\big[\mathbb{E}[R_{t+1}|S_t=s,A_t=a,S_{t+1}=s']\big]\\
&= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\mathbb{E}[R_{t+1}|S_t=s,A_t=a,S_{t+1}=s']\\
&= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;r(s,a,s') \qquad (2)
\end{aligned} $$

Here, $$r(s,a,s')$$ is the _expected_ reward when action $$a$$ is state $$s$$ leads to state $$s'$$.

Next, the second term (without $$\gamma$$) in the RHS of equation 1 can be similarly expanded as:

$$ \begin{aligned}
\mathbb{E}[G_{t+1}| S_t = s] &= \mathbb{E}\big[\mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s']\big] \\
&= \sum_{s'} \mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s']\; p(s'|s) \\
&= \sum_{s'}  \mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s'] \sum_a p(a|s)\;p(s'|s,a)\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \mathbb{E}[G_{t+1}| S_{t+1}=s'] \qquad (3)
\end{aligned} $$

Substituting equations 2 and 3 back in 1, we get:

$$ \begin{aligned}
v_{\pi}(s) &= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;r(s,a,s') \\
&\quad + \gamma \sum_a \pi(a|s) \sum_{s'} p(s'|s,a)\;\mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \big[r(s,a,s') + \gamma \mathbb{E}[G_{t+1}| S_t = s, S_{t+1}=s']\big]\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \big[r(s,a,s') + \gamma  \mathbb{E}[G_{t+1}|S_{t+1}=s']\big]\\
v_{\pi}(s) &= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \big[r(s,a,s') + \gamma v_\pi(s')\big] \qquad (4)
\end{aligned} $$

<p align="center">
Tadaa!
</p>

Wait, you say, the Bellman equation in the book looks different from this...

![Eq 3.14 from Pg 46](/images/BellmanEquation_book_small.png)

Hmm... Well, it turns out that that requires another assumption: the reward obtained per timestep depends only on the current state $$s$$ and the action $$a$$ taken in it, and not the next state $$s'$$.

If that is the case, then equation 2 can be re-written as:

$$ \begin{aligned}
v_{\pi}(s) &= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;r(s,a,s')\\
&= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;\mathbb{E}[R_{t+1}|S_t=s,A_t=a,S_{t+1}=s']\\
&= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;\mathbb{E}[R_{t+1}|S_t=s,A_t=a]\\
&= \sum_{a}\pi(a|s)\;\sum_{s'}p(s'|s,a)\;\sum_{s'}p(r|s,a)\;r\\
&= \sum_{a}\pi(a|s)\;\sum_{s',r}p(s',r|s,a)\;r \qquad (5)
\end{aligned} $$

Starting to look more familiar, eh?

Similarly, we can re-write equation 3. Here, the RHS does not have any reward term, and so:

$$ \begin{aligned}
\mathbb{E}[G_{t+1}| S_t = s]
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \;\mathbb{E}[G_{t+1}| S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a)\cdot 1\cdot \mathbb{E}[G_{t+1}| S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s'} p(s'|s,a) \sum_{r} p(r|s,a)\;\mathbb{E}[G_{t+1}| S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a)\;\mathbb{E}[G_{t+1}| S_{t+1}=s'] \qquad(6)\\
\end{aligned} $$

Finally, substituting equations 5 and 6 back in 1:

$$ \begin{aligned}
v_{\pi}(s) &= \sum_{a}\pi(a|s)\;\sum_{s',r}p(s',r|s,a)\;r \\
&\quad + \gamma \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a)\;\mathbb{E}[G_{t+1}|S_{t+1}=s']\\
&= \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a) \big[r + \gamma \mathbb{E}[G_{t+1}|S_{t+1}=s']\big]\\
v_{\pi}(s) &= \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a) \big[r + \gamma v_\pi(s')\big] \qquad (7)
\end{aligned} $$

Ladies and gentlemen, the Bellman equation.

It was more nuanced that I expected.
