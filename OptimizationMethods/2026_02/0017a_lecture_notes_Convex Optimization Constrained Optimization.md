# Convex Optimization – Constrained Optimization

**Course:** Optimization Methods
**Date:** February 2026
**Author:** Royi Avital (fixelalgorithms.gitlab.io)

---

## 1. Introduction: The Primal Problem

In constrained optimization, we aim to minimize an objective function while adhering to specific constraints. This initial setup is known as the **Primal Problem**:

$$
begin{cases} 
\min_{x \in \mathbb{R}^2} f(x) \ 
	ext{subject to } g(x) \le 0 
\end{cases}
$$

* **$f(x)$**: The objective function (e.g., altitude, cost) we want to minimize.
* **$g(x) \le 0$**: The constraint. Any point $x$ that satisfies this condition is considered **feasible**. If $g(x) > 0$, the point is not feasible (it breaks the rules).
* **Optimal Solution ($x^*$):** The point inside the feasible region that yields the lowest possible value for $f(x)$.

---

## 2. The Lagrangian Relaxation

Instead of dealing with a "hard" constraint boundary, we can translate the constraint into a penalty added to the objective function. This is known as the **Lagrangian Relaxation**.

The **Lagrangian** function is defined as:

$$\mathcal{L}(x, \lambda) := f(x) + \lambda g(x), \quad \lambda \ge 0$$

* **$\lambda$ (Lagrange Multiplier):** This acts as a penalty weight. If $g(x) > 0$ (a violation), a large $\lambda$ imposes a heavy penalty on the total score.
* **Lower Bound Property:** For the optimal solution $x^*$ of the primal problem, the Lagrangian provides a lower bound:
    $$orall \lambda \ge 0: \min_x \mathcal{L}(x, \lambda) \le f(x^*)$$

---

## 3. The Dual Problem and Strong Duality

Optimization can be viewed as a "MinMax" game between the Primal and Dual formulations:

* **The Primal Problem ($P$):** $$P = \min_x \max_{\lambda \ge 0} \mathcal{L}(x, \lambda)$$
* **The Dual Problem ($D$):** $$D = \max_{\lambda \ge 0} \min_x \mathcal{L}(x, \lambda)$$

Generally, $D \le P$ (Weak Duality). However, under **Strong Duality** (e.g., when $f(x)$ and $g(x)$ are convex functions and there is a strictly feasible point), the optimal values are equal:

$$D = P = \mathcal{L}(x^*, \lambda^*)$$

This means solving the Dual problem gives us the exact same optimal value as solving the Primal problem.

---

## 4. Karush-Kuhn-Tucker (KKT) Conditions

The KKT conditions are the ultimate test for optimality. A pair $(x^*, \lambda^*)$ is an optimal solution if and only if it satisfies the following three conditions:

### 1. Feasibility
The solution must respect the original rules and penalty requirements:
$$g(x^*) \le 0$$
$$\lambda^* \ge 0$$

### 2. Complementary Slackness
You cannot simultaneously have an active penalty and be strictly inside the feasible region:
$$\lambda^* g(x^*) = 0$$
* If $g(x^*) < 0$ (strictly feasible/inactive constraint), then $\lambda^* = 0$ (no penalty).
* If $\lambda^* > 0$ (active penalty), then $g(x^*) = 0$ (the solution is exactly on the boundary).

### 3. Stationarity
The gradient of the Lagrangian with respect to $x$ must be zero at the optimum. The "push" of the objective is perfectly balanced by the "push" of the constraint boundary:
$$
abla_x \mathcal{L}(x^*, \lambda^*) = 0$$
$$
abla f(x^*) + \lambda^* 
abla g(x^*) = 0$$

---

## 5. Other Topics Covered in Lecture 008
* **Disciplined Convex Programming (DCP)**
* **Steepest Descent Methods** (For $L_1$, $L_2$, and $L_\infty$ Norms)
* **Orthogonal Projections & Projected Gradient Descent**
* **Linear Programming (LP) and Quadratic Programming (QP)**
* **Linear Fit with $L_1$ and $L_\infty$ Norms**
* **Class Balancing in Machine Learning Training**
