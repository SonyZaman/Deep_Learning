# Module 03 — Practice Questions & Answers

---

## Part 1: Conceptual Questions

---

**Q1. What is a decision boundary, and how does it relate to classification tasks?**

**Answer:**

A **decision boundary** is a line (in 2D), a plane (in 3D), or a hyperplane (in $n$ dimensions) that separates the feature space into two distinct regions — one for each class.

In a classification task, every data point lives somewhere in the feature space. The decision boundary defines the **threshold** the model uses to decide which class a new point belongs to:

- Points on one side → predicted **Class 1**
- Points on the other side → predicted **Class 0**

The model's entire goal during training is to **find the decision boundary** that makes the fewest classification errors.

---

**Q2. How does the bias term in a neuron affect the position of the decision boundary?**

**Answer:**

Without bias, the decision boundary is always forced to pass through the **origin** (0, 0). This severely limits the model because many real-world datasets do not have their class boundary running through the origin.

The bias $b$ acts as an **offset** — it shifts the entire decision boundary to an arbitrary position in the feature space without changing its orientation (slope).

Consider the boundary equation:

$$w_1 x_1 + w_2 x_2 + b = 0$$

- **Weights control the angle** (slope/rotation) of the boundary.
- **Bias controls the position** (translation/offset) of the boundary.

| Bias Value | Effect |
|:---:|:---|
| $b = 0$ | Boundary forced through origin |
| $b > 0$ | Boundary shifts in one direction |
| $b < 0$ | Boundary shifts in the other direction |

Without bias, the perceptron cannot correctly separate classes whose boundary does not pass through the origin.

---

**Q3. Explain with an example how changing weights affects the orientation of the decision boundary.**

**Answer:**

The boundary equation is $w_1 x_1 + w_2 x_2 + b = 0$, which we can rewrite in slope-intercept form:

$$x_2 = -\frac{w_1}{w_2} x_1 - \frac{b}{w_2}$$

The **slope** of the boundary is $-\dfrac{w_1}{w_2}$.

**Example:**

| $w_1$ | $w_2$ | Slope $(-w_1/w_2)$ | Orientation |
|:---:|:---:|:---:|:---|
| 1 | 1 | $-1$ | 45° diagonal |
| 2 | 1 | $-2$ | Steeper diagonal |
| 1 | 3 | $-0.33$ | Nearly flat |
| $-1$ | 1 | $+1$ | Opposite tilt |

So increasing $w_1$ relative to $w_2$ **rotates** the boundary to be steeper, while decreasing it makes the boundary flatter. This is exactly what happens during perceptron training — the weight update rule **rotates the boundary** with each correction until it separates the classes correctly.

---

**Q4. Why is it important to visualize the decision boundary when training a model?**

**Answer:**

Visualizing the decision boundary helps in several ways:

1. **Diagnosing underfitting** — if the boundary is a straight line but the data is clearly non-linear, you can immediately see that a single perceptron is insufficient.

2. **Monitoring convergence** — you can watch the boundary rotate epoch by epoch and confirm it is moving in the right direction.

3. **Detecting misclassification** — outliers or overlap between classes become visible, helping you decide if more data or a more complex model is needed.

4. **Verifying the model** — after training, plotting the boundary against the data confirms whether the learned boundary actually separates the classes as expected.

> A model that looks correct numerically can still have a visually wrong boundary — visualization catches what metrics sometimes miss.

---

## Part 2: Math — Weight Update (Perceptron Learning Rule)

---

**Given:**

| Parameter | Value |
|:---|:---|
| Initial weights | $\mathbf{w} = [0.2,\ -0.1]$ |
| Bias | $b = 0.1$ |
| Learning rate | $\eta = 0.1$ |
| Input | $\mathbf{x} = [1,\ 1]$ |
| Target | $t = 1$ |
| Perceptron output | $y = 0$ |

---

**Step 1: Compute the error**

$$\text{error} = t - y = 1 - 0 = 1$$

The perceptron predicted `0` but the true label is `1`. A correction is needed.

---

**Step 2: Apply the weight update rule**

$$\mathbf{w}_{new} = \mathbf{w}_{old} + \eta \cdot (t - y) \cdot \mathbf{x}$$

For $w_1$:

$$w_1^{new} = 0.2 + 0.1 \times 1 \times 1 = 0.2 + 0.1 = \mathbf{0.3}$$

For $w_2$:

$$w_2^{new} = -0.1 + 0.1 \times 1 \times 1 = -0.1 + 0.1 = \mathbf{0.0}$$

---

**Step 3: Apply the bias update rule**

$$b_{new} = b_{old} + \eta \cdot (t - y)$$

$$b_{new} = 0.1 + 0.1 \times 1 = \mathbf{0.2}$$

---

**Result:**

| Parameter | Before Update | After Update |
|:---|:---:|:---:|
| $w_1$ | $0.2$ | $\mathbf{0.3}$ |
| $w_2$ | $-0.1$ | $\mathbf{0.0}$ |
| $b$ | $0.1$ | $\mathbf{0.2}$ |

**New weights:** $\mathbf{w} = [0.3,\ 0.0]$, **new bias:** $b = 0.2$

---

**Verification — Re-run with updated weights:**

$$z = w_1 x_1 + w_2 x_2 + b = 0.3(1) + 0.0(1) + 0.2 = 0.5$$

$$z = 0.5 \ge 0 \implies \hat{y} = 1 \quad \checkmark \text{ (Now correctly predicts Class 1)}$$

The boundary has been corrected after a single update step.
