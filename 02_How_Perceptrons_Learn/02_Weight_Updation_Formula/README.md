# Weight Updation Formula

---

## 1. The Core Formula

When a perceptron makes an incorrect prediction, it adjusts its weights using the **Weight Updation Formula**:

$$W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i$$

Where:

| Symbol | Meaning |
|---|---|
| $W_{new}$ | The updated weight after correction |
| $W_{old}$ | The current weight before correction |
| $\eta$ | **Learning Rate** — a small value between $0$ and $1$ that controls the step size |
| $y$ | The **actual / true** label |
| $\hat{y}$ | The **predicted** output from the perceptron |
| $x_i$ | The **input** feature corresponding to that weight |

The term $(y - \hat{y})$ is the **error**. If the prediction is correct, the error is $0$ and the weight does not change.

---

## 2. Connection to Gradient Descent

The weight updation formula is a simplified, special case of the general **Gradient Descent** update rule:

$$W_{new} = W_{old} - \eta \cdot \frac{\partial L}{\partial W}$$

In the perceptron's case, the error term $(y - \hat{y}) \cdot x_i$ directly approximates the negative gradient of the loss with respect to the weight. This is why the perceptron learning algorithm works — it is implicitly performing gradient descent on the prediction error.

---

## 3. Case-by-Case Analysis

To understand when and how weights change, consider all possible combinations of $y$ (actual) and $\hat{y}$ (predicted):

---

### Case 01 — Prediction is Too Low: $y = 1$, $\hat{y} = 0$

The perceptron predicted **0** but the true label is **1**. This is a **miss** — the neuron should have fired.

**Error:**
$$y - \hat{y} = 1 - 0 = 1$$

**Applying the formula:**
$$W_{new} = W_{old} + \eta \cdot (1) \cdot x_i$$

$$\boxed{W_{new} = W_{old} + \eta \cdot x_i}$$

The weight **increases**. This makes the weighted sum larger on the next pass, making the neuron more likely to fire.

---

### Case 02 — Prediction is Too High: $y = 0$, $\hat{y} = 1$

The perceptron predicted **1** but the true label is **0**. This is a **false alarm** — the neuron fired when it should not have.

**Error:**
$$y - \hat{y} = 0 - 1 = -1$$

**Applying the formula:**
$$W_{new} = W_{old} + \eta \cdot (-1) \cdot x_i$$

$$\boxed{W_{new} = W_{old} - \eta \cdot x_i}$$

The weight **decreases**. This reduces the weighted sum, making the neuron less likely to fire incorrectly.

---

### Case 03 — Prediction is Correct: $y = \hat{y}$

This covers two sub-cases: $y = 0, \hat{y} = 0$ and $y = 1, \hat{y} = 1$.

**Error:**
$$y - \hat{y} = 0$$

**Applying the formula:**
$$W_{new} = W_{old} + \eta \cdot (0) \cdot x_i$$

$$\boxed{W_{new} = W_{old}}$$

The weight **does not change**. When the prediction is correct, no correction is needed — the perceptron leaves the weights as they are.

---

## Summary

| Case | Actual ($y$) | Predicted ($\hat{y}$) | Error | Weight Change |
|---|---|---|---|---|
| **Case 01** | $1$ | $0$ | $+1$ | Weight **increases** by $\eta \cdot x_i$ |
| **Case 02** | $0$ | $1$ | $-1$ | Weight **decreases** by $\eta \cdot x_i$ |
| **Case 03** | $0$ | $0$ | $0$ | **No change** |
| **Case 03** | $1$ | $1$ | $0$ | **No change** |

The learning rate $\eta$ controls the magnitude of each correction. A value too large causes the model to overshoot; a value too small causes it to learn very slowly. Typical values range from $0.01$ to $0.1$.
