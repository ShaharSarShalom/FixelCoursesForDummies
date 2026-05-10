# Piecewise Linear Model with Auto Knot Selection

---

# 1. The Problem Setup

We are given noisy observations

$$
\{(x_i,y_i)\}_{i=1}^n
$$

where:

- \(x_i\): input/sample location
- \(y_i\): observed value

The data is assumed to come from an underlying function that is:

- linear over intervals,
- but changes slope at a few unknown locations.

These slope-change locations are called **knots**.

---

# 2. Why Piecewise Linear Regression is Difficult

Suppose we knew the knot locations beforehand.

Then the problem is easy:

- split the domain into intervals,
- fit one line per interval.

Similarly, splines also require knot locations.

## The Difficulty

The knot locations are **unknown**.

So we must answer simultaneously:

1. How many knots are there?
2. Where are they?
3. What are the slopes in each segment?

This becomes a **combinatorial model selection problem** if done directly.

---

# 3. Key Optimization Idea

The slide proposes:

> “Model the knots as a sparse phenomenon.”

This is the central idea.

Instead of explicitly parameterizing knot locations, we detect them indirectly.

---

# 4. Discrete Piecewise Linear Structure

Assume samples lie on a grid:

$$
x_1, x_2, \dots, x_n
$$

and let

$$
x = [x_1,\dots,x_n]^T
$$

represent the fitted signal/function values.

A perfectly linear sequence has:

$$
x_i - 2x_{i+1} + x_{i+2} = 0
$$

Why?

Because this is the **second finite difference**.

---

# 5. Second Difference Interpretation

Recall:

For a continuous function,

$$
f''(t)=0
$$

means the function is linear.

In the discrete setting,

$$
x_i - 2x_{i+1} + x_{i+2}
$$

is the discrete analogue of the second derivative.

Thus:

- If it equals zero → local linearity.
- If nonzero → slope changes → a knot.

So knots correspond to locations where the second difference is nonzero.

---

# 6. Sparse Knots

Most regions are assumed linear.

Therefore:

$$
x_i - 2x_{i+1} + x_{i+2}
$$

should be zero for most \(i\).

Only a few indices should be nonzero.

That means:

> the second difference vector is sparse.

This transforms knot detection into a sparse estimation problem.

---

# 7. The Optimization Problem

The slide gives:

$$
\arg\min_x
\frac12\|x-y\|_2^2
+
\lambda
\sum_{i=1}^{n-2}
|x_i - 2x_{i+1} + x_{i+2}|
$$

Let’s dissect it carefully.

---

# 8. First Term: Data Fidelity

$$
\frac12\|x-y\|_2^2
$$

This ensures the fitted signal \(x\) stays close to the observations \(y\).

Equivalent to least squares.

Without regularization:

$$
x=y
$$

which overfits noise.

---

# 9. Second Term: Sparse Curvature Penalty

$$
\lambda
\sum_{i=1}^{n-2}
|x_i - 2x_{i+1} + x_{i+2}|
$$

This penalizes total absolute second differences.

Important:

- It is an \(L^1\)-norm penalty.
- \(L^1\) promotes sparsity.

So the optimizer prefers:

- many second differences exactly zero,
- only a few nonzero.

Hence:

- mostly linear behavior,
- few slope changes,
- automatic knot selection.

---

# 10. Why \(L^1\) Produces Sparsity

If we used squared penalties:

$$
\sum (\Delta^2 x_i)^2
$$

we would obtain smooth curvature everywhere.

That would produce smooth splines.

But \(L^1\) behaves differently:

- it encourages exact zeros,
- leading to sharp transitions.

This is analogous to:

- LASSO in statistics,
- compressed sensing,
- sparse recovery.

---

# 11. Role of \(\lambda\)

$$
\lambda > 0
$$

controls the tradeoff.

## Small \(\lambda\)

- weak regularization,
- many knots,
- closer fit to noise.

## Large \(\lambda\)

- stronger sparsity,
- fewer knots,
- simpler model.

---

# 12. Matrix Form

Define the second-difference operator:

$$
D=
\begin{bmatrix}
1 & -2 & 1 & 0 & \cdots \\
0 & 1 & -2 & 1 & \cdots \\
\vdots & & & & \ddots
\end{bmatrix}
$$

Then:

$$
Dx
$$

contains all second differences.

The optimization becomes:

$$
\min_x
\frac12\|x-y\|_2^2
+
\lambda \|Dx\|_1
$$

This is a convex optimization problem.

---

# 13. Why This is Powerful

This formulation gives:

## Automatic Knot Selection

No need to specify knot positions.

## Convexity

Unlike combinatorial knot search, this problem is convex.

Therefore:

- globally solvable,
- numerically stable,
- scalable.

## Interpretability

Nonzero entries of \(Dx\) indicate knots directly.

---

# 14. Relation to Trend Filtering

This method is known as:

- **\(L^1\) trend filtering**
- or **fused spline estimation**

First-order fused lasso gives piecewise constant signals.

Second-order fused lasso gives piecewise linear signals.

Higher-order differences produce higher-order piecewise polynomials.

---

# 15. Geometric Interpretation

The optimization seeks the simplest explanation of the data:

- fit observations reasonably well,
- but use as few slope changes as possible.

So the solution becomes:

- long straight segments,
- connected at sparse breakpoints.

Exactly the orange curve shown on the right side of the slide.

The purple points are detected knots.

---

# 16. Statistical Interpretation

This is also a form of:

$$
\text{MAP estimation}
$$

with a Laplace prior on second differences:

$$
p(Dx)\propto e^{-\lambda \|Dx\|_1}
$$

which statistically encodes:

> “Most curvatures should be zero.”

---

# 17. Optimization Algorithms

Because the objective is convex but nonsmooth, common solvers include:

- proximal gradient,
- ADMM,
- coordinate descent,
- interior-point methods,
- primal-dual algorithms.

The nonsmoothness comes from the absolute values.

---

# 18. Important Insight

The deepest idea in this slide is:

> We transformed a difficult structural/model-selection problem into a convex sparse optimization problem.

Instead of explicitly searching for knots:

- represent knots implicitly through second derivatives,
- enforce sparsity,
- let optimization discover the structure automatically.

This philosophy appears everywhere in modern optimization:

- compressed sensing,
- sparse coding,
- graphical models,
- change-point detection,
- image denoising,
- total variation regularization.

---

# 19. Summary

The slide proposes:

$$
\min_x
\frac12\|x-y\|_2^2
+
\lambda\|Dx\|_1
$$

where:

- \(x\) = fitted signal,
- \(Dx\) = discrete second derivative,
- sparsity of \(Dx\) = few knots,
- \(L^1\) penalty automatically selects knot locations.

Thus we obtain:

- piecewise linear fitting,
- automatic knot detection,
- convex optimization formulation,
- sparse adaptive model complexity.