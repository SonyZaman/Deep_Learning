# Practice: Perceptron Concepts 

Covering the core building blocks of a Perceptron: **Bias**, **Weights**, **Output Calculation**, the **Step Function**, and **Activation Functions**.

---

## 1. Bias

### What is a bias in a neural network?

**Bias** is an extra learnable parameter added to a neuron that acts as a constant offset — similar to the **intercept** ($b$) in a linear equation $y = mx + b$.

- It is **not connected to any input** — it always contributes a fixed value to the weighted sum.
- It shifts the activation function **left or right**, giving the model more flexibility to fit the data.

### Why do we add bias to a perceptron?

Without bias, the **decision boundary is always forced through the origin (0, 0)**. This severely limits what patterns the perceptron can learn.

| Without Bias | With Bias |
|---|---|
| Decision boundary must pass through the origin | Decision boundary can be anywhere |
| Risk of "dead neurons" if all inputs are 0 | Neuron can still fire even when all $x_i = 0$ |
| Less flexible model | Much more flexible and expressive model |

> **Key Insight:** If all inputs $x_i = 0$, the weighted sum $\sum(w_i \cdot x_i) = 0$. Without a bias, the neuron would always output the same value regardless of the task. Bias prevents this.

---

## 2. Weights

### What are weights in a perceptron?

**Weights** ($w_1, w_2, \ldots, w_n$) are learnable parameters — one for **each input feature**. They determine the **importance (strength)** of each input's contribution to the final output.

- Each input $x_i$ is multiplied by its corresponding weight $w_i$.
- Weights are initialized randomly and then **updated iteratively during training** using the weight update rule.

### How do weights affect the output of a perceptron?

The weight controls how much influence a specific input has on the output:

| Weight Value | Effect on Output |
|---|---|
| **Large positive** $w_i$ | Input $x_i$ strongly pushes the output higher |
| **Large negative** $w_i$ | Input $x_i$ strongly pushes the output lower |
| **Zero** ($w_i = 0$) | Input $x_i$ is completely ignored |
| **Small value** | Input $x_i$ has a weak influence |

> The perceptron learns by adjusting weights using:
> $$W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i$$

---

## 3. Perceptron Output — Worked Example

### Problem

A perceptron has:
- Inputs: $x_1 = 2$, $x_2 = 3$
- Weights: $w_1 = 0.5$, $w_2 = 1$
- Bias: $b = 1$

**Calculate the weighted sum (before activation).**

### Solution

The weighted sum formula is:

$$z = (w_1 \cdot x_1) + (w_2 \cdot x_2) + b$$

Substituting the values:

$$z = (0.5 \times 2) + (1 \times 3) + 1$$

$$z = 1 + 3 + 1$$

$$\boxed{z = 5}$$

> This value $z = 5$ is then passed into the **activation function** to produce the final output.

---

## 4. Step Function

### What is the step function and how does it work?

The **Step Function** (also called the **Heaviside function**) is the simplest activation function. It converts the continuous weighted sum $z$ into a **binary output** — either $0$ or $1$.

$$f(z) = \begin{cases} 1, & \text{if } z \ge 0 \\ 0, & \text{if } z < 0 \end{cases}$$

**How it works:**

1. Compute the weighted sum: $z = \sum(w_i \cdot x_i) + b$
2. If $z \ge 0$ → neuron **fires** → output = **1**
3. If $z < 0$ → neuron stays **silent** → output = **0**

**Applying to our example above:**

Since $z = 5 \ge 0$:
$$f(5) = 1 \quad \text{(Neuron fires)}$$

> **Limitation:** The step function is not differentiable at $z = 0$, so it cannot be used with gradient-based learning algorithms (like backpropagation). This is why modern networks use smoother activation functions.

---

## 5. Activation Function

### Why do we need an activation function in a perceptron?

Without an activation function, the perceptron is just performing a **linear transformation** — no matter how many layers you stack, the output is still a linear combination of the inputs. This means it can only learn **linearly separable** problems.

An activation function introduces **non-linearity**, which allows the network to:
- Learn complex, non-linear patterns in data.
- Model real-world relationships (e.g., image recognition, NLP).
- Stack multiple layers meaningfully (deep networks).

> Without a non-linear activation: $f(Ax + b)$ is still linear. With it: the network can approximate **any function** (Universal Approximation Theorem).

### Name three common activation functions

| # | Activation Function | Formula | Use Case |
|---|---|---|---|
| 1 | **Step Function** | $f(z) = 1$ if $z \ge 0$ else $0$ | Basic binary perceptron |
| 2 | **Sigmoid** | $f(z) = \dfrac{1}{1 + e^{-z}}$ | Binary classification (output layer) |
| 3 | **ReLU** (Rectified Linear Unit) | $f(z) = \max(0, z)$ | Hidden layers in deep networks |

> **ReLU** is the most widely used activation function in modern deep learning due to its simplicity and effectiveness in avoiding the vanishing gradient problem.

---

## Quick Recap

| Concept | One-Line Summary |
|---|---|
| **Bias** | A constant offset that shifts the activation function; prevents dead neurons |
| **Weights** | Learnable values that control each input's importance |
| **Weighted Sum** | $z = \sum(w_i \cdot x_i) + b$ — the raw output before activation |
| **Step Function** | Maps $z$ to binary: $1$ if $z \ge 0$, else $0$ |
| **Activation Function** | Adds non-linearity, enabling the network to learn complex patterns |
