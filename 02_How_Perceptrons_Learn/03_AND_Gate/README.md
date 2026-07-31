# Solving the AND Gate with a Perceptron

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
- **Activation:** Step Function → output $\hat{y} \in \{0, 1\}$

```
x1 ──(w1)──┐
            ├──[ Σ ]──[ Step Function ]──► ŷ
x2 ──(w2)──┘
```

---

## 3. Initial Weight Setup

We begin with **randomly initialized weights** and a threshold:

$$w_1 = 1.2, \quad w_2 = 0.6, \quad \eta = 0.5, \quad \text{threshold} = 1$$

> **Note on Threshold:** The decision rule used here is:  
> $w_1 x_1 + w_2 x_2 \ge \text{threshold} \implies \hat{y} = 1$, otherwise $\hat{y} = 0$

---

## 4. The Weight Update Rule

Every time the perceptron makes a **wrong prediction**, weights are updated using:

$$W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i$$

When the error is $-1$ (predicted 1, should be 0), this simplifies to:

$$W_{new} = W_{old} - \eta \cdot x_i$$

---

## 5. Epoch 1 — Step-by-Step Training

We iterate through all 4 training cases. For each case:
1. Compute $z = w_1 x_1 + w_2 x_2$
2. Compare $z$ to the threshold → get $\hat{y}$
3. Compute error: $\text{error} = y - \hat{y}$
4. Update weights only if error ≠ 0

---

### ▶ Case 1: $x_1 = 0,\ x_2 = 0,\ y = 0$

**Forward Pass:**
$$z = 1.2(0) + 0.6(0) = 0$$
$$z = 0 < 1 \ (\text{threshold}) \implies \hat{y} = 0$$

**Error:** $y - \hat{y} = 0 - 0 = 0$

**No update needed.** ✅ Prediction correct.

---

### ▶ Case 2: $x_1 = 0,\ x_2 = 1,\ y = 0$

**Forward Pass:**
$$z = 1.2(0) + 0.6(1) = 0 + 0.6 = 0.6$$
$$z = 0.6 < 1 \implies \hat{y} = 0$$

**Error:** $y - \hat{y} = 0 - 0 = 0$

**No update needed.** ✅ Prediction correct.

---

### ▶ Case 3: $x_1 = 1,\ x_2 = 0,\ y = 0$

**Forward Pass:**
$$z = 1.2(1) + 0.6(0) = 1.2$$
$$z = 1.2 \ge 1 \implies \hat{y} = 1$$

**Error:** $y - \hat{y} = 0 - 1 = -1$ ❌ Wrong prediction!

**Detailed Weight Update:**

For $w_1$:
$$w_1^{new} = w_1^{old} + \eta(y - \hat{y}) \cdot x_1$$
$$w_1^{new} = 1.2 + 0.5 \times (-1) \times 1 = 1.2 - 0.5 = \mathbf{0.7}$$

For $w_2$:
$$w_2^{new} = w_2^{old} + \eta(y - \hat{y}) \cdot x_2$$
$$w_2^{new} = 0.6 + 0.5 \times (-1) \times 0 = \mathbf{0.6}$$

**Updated weights after Case 3:** $w_1 = 0.7,\ w_2 = 0.6$

**Verification with new weights:**
$$z = 0.7 \times 1 + 0.6 \times 0 = 0.7 < 1 \implies \hat{y} = 0 \quad ✓$$

---

### ▶ Case 4: $x_1 = 1,\ x_2 = 1,\ y = 1$

**Forward Pass (with updated weights $w_1=0.7,\ w_2=0.6$):**
$$z = 0.7(1) + 0.6(1) = 1.3$$
$$z = 1.3 \ge 1 \implies \hat{y} = 1$$

**Error:** $y - \hat{y} = 1 - 1 = 0$

**No update needed.** ✅ Prediction correct.

---

## 6. Epoch 1 — Summary

| Case | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | Error | Update? | $w_1$ after | $w_2$ after |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | 0 | 0 | 0 | 0 | 0 | ❌ No | 1.2 | 0.6 |
| 2 | 0 | 1 | 0 | 0 | 0 | ❌ No | 1.2 | 0.6 |
| 3 | 1 | 0 | 0 | 1 | −1 | ✅ Yes | **0.7** | **0.6** |
| 4 | 1 | 1 | 1 | 1 | 0 | ❌ No | 0.7 | 0.6 |

**Weights after Epoch 1:** $w_1 = 0.7,\ w_2 = 0.6$

---

## 7. Epoch 2 — Verifying Convergence

We run through all 4 cases again with the new weights $w_1 = 0.7,\ w_2 = 0.6$, threshold = 1:

### ▶ Case 1: $x_1 = 0,\ x_2 = 0$
$$z = 0.7(0) + 0.6(0) = 0 < 1 \implies \hat{y} = 0, \quad y = 0 \quad \text{✅ No update}$$

### ▶ Case 2: $x_1 = 0,\ x_2 = 1$
$$z = 0.7(0) + 0.6(1) = 0.6 < 1 \implies \hat{y} = 0, \quad y = 0 \quad \text{✅ No update}$$

### ▶ Case 3: $x_1 = 1,\ x_2 = 0$
$$z = 0.7(1) + 0.6(0) = 0.7 < 1 \implies \hat{y} = 0, \quad y = 0 \quad \text{✅ No update}$$

### ▶ Case 4: $x_1 = 1,\ x_2 = 1$
$$z = 0.7(1) + 0.6(1) = 1.3 \ge 1 \implies \hat{y} = 1, \quad y = 1 \quad \text{✅ No update}$$

**All 4 cases are correct! Zero errors → Training Converged! 🎉**

---

## 8. Summary

| Concept | Key Point |
|---|---|
| **AND Gate** | Output is `1` only when both inputs are `1` |
| **Initial weights** | $w_1 = 1.2,\ w_2 = 0.6,\ \eta = 0.5,\ \text{threshold} = 1$ |
| **Update rule** | $W_{new} = W_{old} + \eta(y - \hat{y}) \cdot x_i$ |
| **Epoch 1** | Only Case 3 caused an update ($w_1: 1.2 \to 0.7$) |
| **Epoch 2** | All predictions correct — perceptron **converged** |
| **Key insight** | Weights only change when the perceptron makes a mistake |
