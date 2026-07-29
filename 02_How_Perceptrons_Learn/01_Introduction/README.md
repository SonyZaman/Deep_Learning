# How a Perceptron Learns: Introduction

> **Module:** 02 — How Perceptrons Learn | **Topic:** 01 — Introduction  
> This note covers how a Perceptron is set up as a **Supervised Learning** model, how it processes a forward pass, and how it decides its output through a numerical worked example.

---

## 1. The Perceptron as a Supervised Algorithm

A Perceptron is a **Supervised Learning** algorithm. This means it learns from **labeled data** — data where we already know the correct answer ($y$).

The typical workflow is:

```
Input Data  →  Weighted Sum + Bias  →  Activation Function  →  Predicted Output (ŷ)
                                                              ↕ Compare with y
                                                         Update Weights if wrong
```

The perceptron sees many training examples, checks its prediction against the true label, and adjusts its weights every time it makes a mistake. Over time, it learns the correct decision boundary.

---

## 2. How a Perceptron Processes an Input (The Forward Pass)

Given inputs $x_1, x_2, \ldots, x_n$ with corresponding weights $w_1, w_2, \ldots, w_n$ and a bias $b$, the perceptron first computes a **weighted sum**:

$$z = x_1 w_1 + x_2 w_2 + \cdots + x_n w_n + b$$

Or compactly:

$$z = \sum_{i=1}^{m} (x_i \cdot w_i) + b$$

This value $z$ is then passed through the **Activation Function** (Step Function) to produce the final output:

$$\hat{y} = \begin{cases} 1, & \text{if } z \ge 0 \\ 0, & \text{if } z < 0 \end{cases}$$

---

## 3. A Numerical Worked Example

### Problem: Study Hours, Attendance → Pass or Fail?

Let's say we have a dataset where a student either **passes (1)** or **fails (0)** based on:
- $x_1$ = Study Hours
- $x_2$ = Attendance

| Study Hours ($x_1$) | Attendance ($x_2$) | Pass/Fail ($y$) |
|---|---|---|
| 7  | 1  | 1 (Pass) |
| 12 | 7  | 1 (Pass) |

**Initial Weight Initialization (randomly chosen):**
$$w_1 = 0.3, \quad w_2 = 0.5, \quad b = 0.1$$

---

### Step-by-Step Forward Pass (First Example: $x_1=7, x_2=1$)

**① Compute the weighted sum:**

$$z = (7 \times w_1) + (1 \times w_2) + b$$
$$z = (7 \times 0.3) + (1 \times 0.5) + 0.1$$
$$z = 2.1 + 0.5 + 0.1 = 2.7$$

**② Apply the Step Activation Function:**

$$z = 2.7 \ge 0 \implies \hat{y} = 1 \quad ✓ \text{ (Correct! Predicted Pass)}$$

---

### Step-by-Step Forward Pass (Second Example: $x_1=3, x_2=0$)

**① Compute the weighted sum:**

$$z = (3 \times w_1) + (0 \times w_2) + b$$

**② Check the decision boundary with $w_1=1, w_2=1.5$:**

$$7w_1 + 3.5w_2 = 7(1) + 3.5(1.5) = 7 + 5.25 = 12.25 \ge 0 \implies \hat{y} = 1$$
$$7 + 3(1.5) = 7 + 4.5 = 11.5 \ge 0 \implies \hat{y} = 1$$

> **Key Insight:** The sign of $z$ tells us which "side" of the decision boundary a data point falls on. Positive → Class 1 (Pass), Negative → Class 0 (Fail).

---

## 4. What Happens When the Perceptron is Wrong?

If the perceptron predicts $\hat{y} = 0$ but the true label is $y = 1$, it has made an error. It will then update its weights using the **Weight Updating Formula** (covered in detail in the next section):

$$W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i$$

$$b_{new} = b_{old} + \eta \cdot (y - \hat{y})$$

Where $\eta$ is the **learning rate** — a small value (e.g., $0.1$) that controls how big a correction step the model takes.

> 💡 **Quick Intuition:** If $y - \hat{y} = 0$, the prediction was correct and the weights don't change. If $y - \hat{y} \ne 0$, the weights are nudged in the direction that would have made the correct prediction.

---

## Summary

| Concept | Key Point |
|---|---|
| **Supervised Learning** | Perceptron learns from labeled examples $(x, y)$ |
| **Forward Pass** | $z = \sum w_i x_i + b$, then apply Step Function |
| **Step Function** | Output $1$ if $z \ge 0$, else $0$ |
| **Weight Init** | Weights can be initialized randomly (e.g., $w_1=0.3, w_2=0.5, b=0.1$) |
| **Weight Update** | Only happens when a prediction is **wrong** |
| **Learning Rate $\eta$** | Controls the size of each weight update step |
