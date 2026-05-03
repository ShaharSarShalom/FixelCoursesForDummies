# Convex Optimization – Algorithms & Solvers
### The "For Dummies" Summary

**Course:** Optimization Methods
**Date:** February 2026
**Author:** Royi Avital

---

Welcome back to class! In previous lectures, we figured out what the "valley" (objective function) and the "fences" (constraints) look like. Now, we are looking at the **Algorithms**—the actual step-by-step recipes a computer uses to find the absolute bottom of that valley. 

Sometimes, standard methods are just too slow, or the math is too ugly to solve all at once. This lecture gives us three major mathematical "cheat codes" to solve these problems faster and smarter.


---

## 1. Acceleration Methods (FISTA): Building Momentum

Imagine you are walking down a mountain blindfolded to find the lowest point. 
* **Standard Gradient Descent** is like taking a single step, stopping, feeling the slope with your foot, and then taking another step. It is safe, but very slow.
* **Acceleration Methods (like FISTA)** add **momentum**. If you have been walking steadily downhill for a while, you start to jog! 

Instead of just looking at where you are *right now*, acceleration algorithms (invented by Nesterov) use your **past steps** to predict a better, faster direction. 
* **FISTA** (Fast Iterative Shrinkage-Thresholding Algorithm) is a famous version of this. 
* *Fun fact:* Because of this momentum, the algorithm might accidentally run slightly uphill for a brief moment, but overall, it gets to the bottom much faster than standard methods!

side note: in optimization problem FISTA is better then adam optimizer (in dnn it's different) 

---

## 2. Augmented Lagrangian: The Smart Bouncer

Remember how we deal with constraints (rules you can't break, like $h(x) = 0$)? 

* **The Penalty Method (The Dumb Wall):** One way to keep you in bounds is to build a massive, infinitely steep mathematical wall. If you step out of bounds, your score gets hit with a massive penalty ($p 	o \infty$). The problem? Computers *hate* dealing with massive numbers—it causes calculations to explode or freeze.
* **The Augmented Lagrangian Method (The Smart Bouncer):** To fix this, we combine a smaller, manageable penalty wall with a "smart bouncer" (our old friend, the Lagrange Multiplier, $\lambda$). 
    * Instead of making the wall infinitely tall, the algorithm slowly adjusts the bouncer's "push" over time based on your past mistakes. It converges smoothly and beautifully without crashing your computer.

---

## 3. ADMM: Divide, Conquer, and Compromise

**ADMM** stands for *Alternating Direction Method of Multipliers*. This is the heavy lifter of the optimization world.

Sometimes, an optimization problem is a nightmare because it consists of two very different, conflicting mathematical rules crashing together (e.g., trying to minimize $f(x) + g(Px)$). 

**How ADMM works (The Teamwork Trick):**
1. **Split them up:** ADMM introduces a "dummy" clone variable, $z$. Now, instead of one giant problem, you have two smaller problems. 
2. **Take turns:** You solve the smooth, easy part for $x$. Then, you solve the ugly, non-smooth part for $z$. 
3. **Compromise:** Finally, you force $x$ and $z$ to negotiate. A multiplier variable (like a referee) updates itself to force $x$ and $z$ to eventually equal the exact same thing.

It is exactly like two separate departments in a company working on their own parts of a project, and then meeting in the middle to sync up their work.

---

## 4. Why Do We Care? (Real-World Applications)

The lecture wraps up by showing where these algorithms are used in the real world:

* **1D Total Variation (TV) Denoising:** Imagine a heartbeat monitor graph that is full of static (noise). Standard smoothing tools will remove the noise but blur the sharp heartbeat spikes into soft lumps. Algorithms like FISTA and ADMM can strip away the noise while keeping the "stair-step" jumps perfectly sharp. 
* **Consensus (Intersection of Convex Sets):** When you have a massive problem with thousands of constraints, ADMM allows you to send the math to 1,000 different computers. Each computer solves its tiny piece, and ADMM mathematically averages their answers together until they all agree on the perfect spot.
* **Auto Knot Selection:** Fitting complex lines to data points where the algorithm automatically figures out the best places for the line to bend or change direction.

---
*Summary generated for easy reading and conceptual understanding.*

