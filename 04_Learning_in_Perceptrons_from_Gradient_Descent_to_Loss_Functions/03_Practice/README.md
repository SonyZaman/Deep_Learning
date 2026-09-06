# 03 Practice — Perceptron Learning: Labels, Update Rule & Gradient Descent


---

## Q1 — Why ±1 Labels Instead of 0 and 1?

### Short Answer

The **±1 label convention** makes the perceptron update rule elegant and mathematically unified.

### Detailed Explanation

In the perceptron, a prediction is **correct** when the sign of the raw output `w·x` matches the true label `y`.

| Label Convention | Correct prediction condition |
|---|---|
| `y ∈ {0, 1}` | Need separate cases for each class |
| `y ∈ {−1, +1}` | Unified: `y · (w·x) > 0` always means correct |

With **±1 labels**, a single elegant condition captures correctness:

```
y · (w · x) > 0   →  correctly classified  (no update needed)
y · (w · x) ≤ 0   →  misclassified         (update required)
```

### Effect on the Update Rule

The perceptron update is triggered only on a **misclassified** point:

```
w  ←  w + η · y · x
```

Because `y ∈ {−1, +1}`:
- If `y = +1` → we **add** `η·x`, pushing `w` to produce a more positive output.
- If `y = −1` → we **subtract** `η·x` (since `η·y·x = −η·x`), pushing `w` to produce a more negative output.

This would **not work cleanly** with `y = 0`, because the update term `η · 0 · x = 0` would produce **no update at all** for the negative class — the weights would never learn to separate it.

---

## Q2 — Why Does the Perceptron Add `η·y·x` Instead of Subtracting the Gradient?

### Short Answer

The perceptron **does** follow gradient descent — it just happens that the negative gradient of the perceptron loss points in the direction `+η·y·x`.

### Detailed Explanation

Standard gradient descent subtracts the gradient:

```
w  ←  w − η · ∇L(w)
```

For the **perceptron loss** on a misclassified point:

```
L(w) = −y · (w · x)       (positive when misclassified, because y·(w·x) ≤ 0)
```

Taking the gradient with respect to `w`:

```
∇_w L(w) = −y · x
```

Substituting into the gradient descent update:

```
w  ←  w − η · (−y · x)
w  ←  w + η · y · x
```

✅ **So the addition of `η·y·x` IS gradient descent — the minus sign from gradient descent cancels with the minus sign from the gradient of the loss.**

### Intuition

| Situation | What needs to happen |
|---|---|
| Point `x` is `+1` but classified negative | Push `w` toward `+x` → add `η·x` |
| Point `x` is `−1` but classified positive | Push `w` away from `+x` → subtract `η·x` |

The term `η·y·x` automatically handles both cases with the correct sign.

---

## Q3 — Perceptron Learning as Gradient Descent + One Limitation

### Perceptron as Gradient Descent

The perceptron learning rule is **literally a form of stochastic gradient descent (SGD)** applied to the **perceptron loss function**.

**Step-by-step derivation:**

1. **Define the loss** for a single misclassified sample `(x, y)`:

   ```
   L(w) = −y · (w · x)
   ```

   This loss is 0 for correctly classified points and positive for misclassified ones.

2. **Compute the gradient:**

   ```
   ∇_w L(w) = −y · x
   ```

3. **Apply gradient descent:**

   ```
   w  ←  w − η · ∇_w L(w)
      =  w − η · (−y · x)
      =  w + η · y · x
   ```

4. This matches **exactly** the perceptron update rule.

The "stochastic" part means we apply this update **one misclassified sample at a time** rather than averaging over all samples — which is exactly what the classical perceptron algorithm does.

### Summary Table

| Gradient Descent Step | Perceptron Equivalent |
|---|---|
| Define a loss function | `L(w) = −y·(w·x)` for misclassified points |
| Compute gradient | `∇L = −y·x` |
| Subtract gradient | `w ← w + η·y·x` |
| Repeat per sample | Loop over misclassified points |

---

### ⚠️ One Limitation of the Perceptron Loss Function

> **The perceptron loss is non-differentiable and flat for correctly classified points.**

Specifically:

- The loss is defined as `max(0, −y·(w·x))`.
- For **correctly classified** points, the loss = 0 and the **gradient is zero** — the weights receive **no update** and no feedback on how confidently correct they are.
- For **misclassified** points, the gradient is a constant `−y·x` — there is **no smooth gradation** based on how badly wrong the prediction is.

This makes it **different from smooth losses** like logistic (cross-entropy) loss, which:
- Provides gradients even for correct predictions.
- Penalizes overconfident wrong predictions more heavily.
- Enables more reliable convergence in non-linearly separable settings.

Additionally, the perceptron algorithm is **only guaranteed to converge if the data is linearly separable**. If it is not, the algorithm loops forever — a fundamental limitation that motivated the development of SVMs and neural networks.

---

## Key Formulas at a Glance

```
Prediction:      ŷ = sign(w · x)

Perceptron Loss: L(w) = max(0, −y · (w · x))   [for one sample]

Update Rule:     w ← w + η · y · x              [only on misclassified points]

Gradient:        ∇_w L = −y · x                 [for misclassified sample]
```

---

