# Perceptron Learning Rule

---

## 1. The Decision Boundary Has Coefficients

From the previous section, the decision boundary is a line of the form:

$$Ax + By + C = 0$$

In perceptron notation these are the **weights and bias**:

$$w_1 x_1 + w_2 x_2 + c = 0$$

For $n$ features, the compact general form is:

$$\sum_{i=0}^{n} w_i x_i = 0 \quad \text{where } x_0 = 1 \text{ (bias input)}$$

The coefficients $[w_1, w_2, \ldots, c]$ **determine the position and angle of the boundary**. Changing them rotates or shifts the line.

---

## 2. When Does the Boundary Need to Change?

The perceptron checks each training point. If the prediction is correct — the boundary stays. If a point is **misclassified**, the boundary is in the wrong position and its coefficients must be corrected.

There are exactly **two ways** a perceptron can be wrong:

---

## 3. The Two Misclassification Cases

### Case 1: True label is 1, predicted 0 — the boundary is too far away

The point belongs to **Class 1** but landed on the wrong side of the line. The boundary needs to move **toward** this point:

$$Coeff_{new} = Coeff_{old} + \eta \cdot x_i$$

### Case 2: True label is 0, predicted 1 — the boundary overreached

The point belongs to **Class 0** but ended up on the wrong side. The boundary needs to move **away** from this point:

$$Coeff_{new} = Coeff_{old} - \eta \cdot x_i$$

![Misclassification Cases](misclassification_cases.png)

---

## 4. Deriving the Unified Update Formula

Looking at the two cases:

| Situation | Error $(y - \hat{y})$ | Update |
|:---:|:---:|:---:|
| $y=1$, $\hat{y}=0$ | $+1$ | $Coeff + \eta \cdot x_i$ |
| $y=0$, $\hat{y}=1$ | $-1$ | $Coeff - \eta \cdot x_i$ |
| Correct prediction | $0$ | No change |

Both cases are captured perfectly by a single formula where the **error term** $(y - \hat{y})$ automatically provides the right sign:

$$\boxed{W_{new} = W_{old} + \eta \cdot (y - \hat{y}) \cdot x_i}$$

Where:
- $\eta$ — learning rate (controls step size)
- $(y - \hat{y})$ — the error: $+1$, $-1$, or $0$
- $x_i$ — the input feature associated with that weight

When the prediction is correct, the error is `0` and the weights do not change.

---

## 5. The Boundary Rotates Over Epochs

With each correction, the boundary rotates slightly toward the right orientation. Over multiple training epochs it converges to the line that correctly separates all classes.

![Boundary Rotation](boundary_rotation.png)

---

## 6. A Worked Example

Suppose the current decision boundary is:

$$2x + 3y + 5 = 0 \quad \Rightarrow \quad \text{Coefficients: } [A=2,\ B=3,\ C=5]$$

A misclassified point with coordinates $(x_1, x_2) = (6, 2)$ and bias $x_0 = 1$ has the full input vector $\mathbf{x} = [6, 2, 1]$.

With learning rate $\eta = 0.1$ and error $(y - \hat{y}) = -1$ (Case 2 — predicted 1, actual 0):

| Coefficient | Old Value | Update $(-0.1 \times x_i)$ | New Value |
|:---:|:---:|:---:|:---:|
| $A$ (for $x_1=6$) | 2 | $-0.6$ | **1.4** |
| $B$ (for $x_2=2$) | 3 | $-0.2$ | **2.8** |
| $C$ (bias, $x_0=1$) | 5 | $-0.1$ | **4.9** |

New boundary equation:

$$1.4x + 2.8y + 4.9 = 0$$

---

## Summary

| Concept | Key Point |
|---|---|
| **Decision Boundary** | Defined by coefficients $[w_1, w_2, \ldots, c]$ |
| **No error** | Weights unchanged |
| **Error = +1** (missed a positive) | $W_{new} = W_{old} + \eta \cdot x_i$ |
| **Error = −1** (misclassified a negative) | $W_{new} = W_{old} - \eta \cdot x_i$ |
| **Unified Rule** | $W_{new} = W_{old} + \eta(y - \hat{y}) \cdot x_i$ |
| **Learning Rate $\eta$** | Controls how large each correction step is |
| **Convergence** | If data is linearly separable, the rule is guaranteed to find the correct boundary |
