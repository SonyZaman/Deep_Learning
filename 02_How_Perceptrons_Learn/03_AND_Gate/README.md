# Solving the AND Gate with a Perceptron

> **Module:** 02 — How Perceptrons Learn | **Topic:** 03 — AND Gate  
> This note walks through a complete, step-by-step example of training a perceptron to learn the **AND logic gate** from scratch — including the truth table, architecture setup, initial weights, and case-by-case forward pass with weight updates.

---

## 1. What is an AND Gate?

Before training, let's understand what the perceptron needs to learn. The **AND gate** is a binary logic function. Its output is `1` only when **both** inputs are `1`.

**AND Gate Truth Table:**

| Case | $x_1$ | $x_2$ | $y$ (AND) |
|:---:|:---:|:---:|:---:|
| 1 | 0 | 0 | **0** |
| 2 | 0 | 1 | **0** |
| 3 | 1 | 0 | **0** |
| 4 | 1 | 1 | **1** |

> 💡 This is a **linearly separable** problem — a straight line can separate the `0` outputs from the `1` output, which means a single perceptron can solve it.

---

## 2. Perceptron Architecture for AND Gate

The perceptron for this problem has:
- **2 Inputs:** $x_1$ and $x_2$
- **2 Weights:** $w_1$ and $w_2$
- **1 Bias:** $b$
- **Activation:** Step Function → output $\hat{y} \in \{0, 1\}$

```
x1 ──(w1)──┐
            ├──[ Σ + bias ]──[ Step Function ]──► ŷ
x2 ──(w2)──┘
```

---

## 3. Initial Weight Setup

We begin with **randomly initialized weights** and a threshold/bias:

$$w_1 = 1.2, \quad w_2 = 0.6, \quad \eta = 0.5, \quad \text{threshold} = 1$$

> **Note on Threshold vs. Bias:** In this formulation, the decision rule is:  
> $w_1 x_1 + w_2 x_2 \ge \text{threshold} \implies \hat{y} = 1$, otherwise $\hat{y} = 0$

---

## 4. Step-by-Step Training (Forward Pass + Weight Update)

We iterate through all 4 training cases. For each case, we:
1. Compute $z = w_1 x_1 + w_2 x_2$
2. Compare $z$ to the threshold → get $\hat{y}$
3. Compute the error: $\text{error} = y - \hat{y}$
4. Update weights if error ≠ 0: $W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i$

---

### ▶ Case 1: $x_1 = 0,\ x_2 = 0,\ y = 0$

**Forward Pass:**
$$z = w_1(0) + w_2(0) = 0$$
$$z = 0 < 1 \ (\text{threshold}) \implies \hat{y} = 0$$

**Error:** $y - \hat{y} = 0 - 0 = 0$

**No update needed.** ✅ Prediction correct.

---

### ▶ Case 2: $x_1 = 0,\ x_2 = 1,\ y = 0$

**Forward Pass:**
$$z = w_1(0) + w_2(1) = 0 + 0.6 = 0.6$$
$$z = 0.6 < 1 \implies \hat{y} = 0$$

**Error:** $y - \hat{y} = 0 - 0 = 0$

**No update needed.** ✅ Prediction correct.

---

### ▶ Case 3: $x_1 = 1,\ x_2 = 0,\ y = 0$

**Forward Pass:**
$$z = w_1(1) + w_2(0) = 1.2 + 0 = 1.2$$
$$z = 1.2 \ge 1 \implies \hat{y} = 1$$

**Error:** $y - \hat{y} = 0 - 1 = -1$ ❌ Wrong prediction!

**Weight Update:**
$$w_1^{new} = w_1^{old} + \eta(y - \hat{y}) \cdot x_1 = 1.2 + 0.5(-1)(1) = 1.2 - 0.5 = \mathbf{0.7}$$
$$w_2^{new} = w_2^{old} + \eta(y - \hat{y}) \cdot x_2 = 0.6 + 0.5(-1)(0) = \mathbf{0.6}$$

**Updated weights:** $w_1 = 0.7,\ w_2 = 0.6$

---

### ▶ Case 4: $x_1 = 1,\ x_2 = 1,\ y = 1$

**Forward Pass (with updated weights $w_1=0.7, w_2=0.6$):**
$$z = w_1(1) + w_2(1) = 0.7(1) + 0.6(1) = 1.3$$

**But wait — let's re-verify with original weights $w_1=1.2, w_2=0.6$:**
$$z = 1.2(1) + 0.6(1) = 1.8$$
$$z = 1.8 \ge 1 \implies \hat{y} = 1$$

**Error:** $y - \hat{y} = 1 - 1 = 0$

**No update needed.** ✅ Prediction correct.

---

## 5. Summary of Training Pass

| Case | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | Error | Update? |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | 0 | 0 | 0 | 0 | 0 | ❌ No |
| 2 | 0 | 1 | 0 | 0 | 0 | ❌ No |
| 3 | 1 | 0 | 0 | 1 | −1 | ✅ Yes |
| 4 | 1 | 1 | 1 | 1 | 0 | ❌ No |

> The perceptron only updates when it makes a **mistake**. After seeing all 4 cases (one epoch), it repeats until all predictions are correct.

---

## 6. Key Takeaway

The perceptron successfully **learns the AND gate** by iteratively adjusting weights based on errors. This is the core of how perceptron training works:

> **Prediction correct → do nothing. Prediction wrong → adjust weights using the learning rate and the error.**

More screenshots and cases will be added in the next update! 🔄
