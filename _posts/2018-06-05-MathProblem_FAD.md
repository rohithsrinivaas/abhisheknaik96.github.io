---
layout : post
title : A Classical Math Problem Gets Pulled Into Self-Driving Cars
date : 2018-06-05
time : +1520
permalink : posts/MathProblem_FAD
tags : summary
---

- [Wired Article](https://www.wired.com/story/a-classical-math-problem-gets-pulled-into-self-driving-cars/)
- DSOS and SDSOS Optimization: More Tractable Alternatives to Sum of Squares and Semidefinite Optimization [[arXiv](https://arxiv.org/pdf/1805.08966v1.pdf)] 


---

## TL;DR version
---

### Motivation

- Assume you know at every time-step where exactly each obstacle is around you.
- Suppose I assign a positive cost to each obstacle and a negative cost to the 'free space'. 
- The idea is to come up with a polynomial cost map that exactly matches these aforementioned costs. 
- If this is possible, the vehicle now moves only in region that is deemed safe mathematically by the polynomial, giving us a formal guarantee of safety.

### Methodology

- Find polynomials whose maxima are at minimum distance from the obstacles.
- This is done (among other things) with the help of the sum-of-square test for nonnegativity for polynomials.
- The authors have come up with a way to perform this test efficiently, which could be useful when this needs to be done in real-time for FAD.

---

## Longer version
---


### Motivation

- "To get a complete 100-percent-provable guarantee that your system will be collision-avoidant"
- The idea is to provable/reliably find free space for a vehicle to drive into.
- This can be done by coming up with elaborate polynomials to find the free space in the surrounding and 'fence off' detected obstacles, but this has to be done _in real-time_...


### Methodology

#### The Math Problem

- If a polynomial can be expressed as a sum of squares, then it is non-negative.
- Some optimization problems involve finding the minimum of a polynomial. Finding the minimum value of a polynomial with many variables is hard.
- One of the alternate methods to do this is the test of non-negativity, wherein one tries to find the maximum value one can subtract from the polynomial-in-question such that it is still non-negative (a sum of squares). That value is the minimum value that the polynomial can take, and the value of the variable that leads to that minimum value can be found out with other methods.
- People used to use semidefinite programming (SP) to check the sum-of-squares condition, but it slow on large problems and can’t handle many of the most complicated polynomials researchers really care about - for instance, making a humanoid walk. SP can be used to find a sum of squares decomposition for polynomials with upto a dozen variables raised to powers no higher than about 6.
- [Ahmadi and Majumdar](https://arxiv.org/abs/1706.02586) found a way to solve many linked problems instead of the single slow semidefinite program, and then combine the results together to test for nonnegativity _quickly_.

#### Application to FAD

- A polynomial can serve as a kind of mathematical barrier around obstacles you don't want to hit — if you can find it fast enough.
- The idea : 
    * Make a polynomial such that its value is positive at obstacles and negative at 'free space'. 
    * At some points between the free space and the obstacle(s), the polynomial will change sign.  Since your car is allowed to be on points only where the polynomial is negative, these points form something like a wall. 
    * The objective is to craft the polynomial such that this distance between the point of inflection and the obstacle is minimal. 
- This intuitively cordons off the obstacles, and since the car is only supposed to stay in the negative region of the polynomial and not cross the boundary (polynomial), we have a formal guarantee of safety.
> In realistic driving scenarios, a car's sensors continually identify new and shifting obstacles - cars, bikes, kids. Every time a new obstacle appears, or an existing one moves, the car has to come up with elaborate new polynomials to fence them off. That's a lot of sum of squares checks to do on the fly.

---

### Notes

1. Concretely, the two-fold approach is as follows : 
    1. come up with a polynomial that hugs the obstacles tightly,  
    2. if this is possible, the vehicle now moves only in region that is deemed safe mathematically by the polynomial, giving us a formal guarantee of safety.     
The first step is performed by leveraging the SOS property and the authors' efficient way of computing it _in real-time_.

2. How do you efficiently search in the entire subspace of polynomials?
    * Need more digging into optimization and its gory mathematics to answer this one...   

3. Is the sum-of-squares condition the only way a polynomial can be non-negative?
    * Turns out, the sum-of-squares (SOS) decomposition is sufficient for non-negativity (ob), but the reverse is not true. That is, in general, SOS is not equivalent to nonnegativity.
    * Haha, look what I dug up -    
    Hilbert was one of the examiners of a PhD defense, wherein the student claimed that there exist nonnegative polynomials which aren't sum of squares. Hilbert published a paper on the categories of polynomials for which this could be, but it was only about 70 years later that the first explicit example was found.

4. Since this is not the case, you might actually be missing out on feasible solutions in your search space, which increases the wall-clock time of computation. 

