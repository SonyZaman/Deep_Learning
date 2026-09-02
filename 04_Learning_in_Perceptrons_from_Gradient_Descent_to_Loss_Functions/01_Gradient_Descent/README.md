# Gradient Descent: Step-by-Step Mathematical Derivation

## 1. The Setup: Linear Regression and Loss

Imagine we are trying to predict a value (like Attendance $y$) based on an input (like Study Hours $x$). We want to fit a line to our data points:
$$ \hat{y} = mx + b $$

To measure how wrong our line is, we use the Sum of Squared Errors (Loss function):
$$ L = \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 $$
$$ L = \sum_{i=1}^{n} (y_i - mx_i - b)^2 $$

## 2. A Simpler Problem: Fixing $m$ to find $b$

To understand how Gradient Descent works gradually, let's temporarily pretend we already know the perfect slope is $m = 10$. Now our only job is to find the best intercept, $b$.

Our loss function simplifies to a 2D problem:
$$ L(b) = \sum_{i=1}^{n} (y_i - 10x_i - b)^2 $$

If we plot Loss $L$ against $b$, we get a U-shaped curve (a parabola). The lowest point on this curve is the **global minimum** where our error is smallest.

### The Intuition of Movement

If we pick a random starting point for $b$ on this curve, we can look at the slope (the gradient) at that exact point to decide which way to step:
*   **Positive Slope (+)**: We are on the right side of the minimum. We need to move left (decrease $b$). As noted in the diagrams: *"+ve slope, (-)ve দিকে যাবে"* (Goes in the negative direction).
*   **Negative Slope (-)**: We are on the left side of the minimum. We need to move right (increase $b$). As noted: *"(-)ve slope, (+) দিকে যাবে"* (Goes in the positive direction).

This gives us our logical update rule:
$$ b_{\text{new}} = b_{\text{old}} - (\text{step\_size} \times \text{slope}) $$

By subtracting the slope, we guarantee we always move *towards* the minimum. In calculus terms, the slope is the derivative $\frac{dL}{db}$, and the step size is the learning rate $\eta$:
$$ b_{\text{new}} = b_{\text{old}} - \eta \times \frac{dL}{db} $$

### Calculating the Derivative for $b$

Let's find the slope formula ($\frac{dL}{db}$) using the chain rule from calculus on our simplified loss function (assuming $n=4$ data points for a small dataset):
$$ L = \sum_{i=1}^{4} (y_i - 10x_i - b)^2 $$

Bring down the power of 2, and multiply by the derivative of the inside with respect to $b$ (which is $-1$):
$$ \frac{dL}{db} = 2 \sum_{i=1}^{4} (y_i - 10x_i - b) \cdot (-1) $$
$$ \frac{dL}{db} = -2 \sum_{i=1}^{4} (y_i - 10x_i - b) $$

**The First Step Example:** If we start with a completely random initial guess of $b=0$, the very first slope we calculate would be:
$$ \text{slope} = -2 \sum_{i=1}^{4} (y_i - 10x_i - 0) $$
We then plug this specific slope into our formula $b_{\text{new}} = b_{\text{old}} - \eta \times \text{slope}$ to take our first step down the parabola!

## 3. The Real Problem: Finding both $m$ and $b$

In reality, we don't know $m$. We have to find both $m$ and $b$ at the exact same time.
Our loss is no longer a 2D line, but a 3D bowl shape:
$$ L(m,b) = \sum_{i=1}^{n} (y_i - mx_i - b)^2 $$

Because we have two unknown variables, we can't just use a standard derivative. We must use **Partial Derivatives**. This means we find the slope with respect to $b$ while pretending $m$ is constant, and then we find the slope with respect to $m$ while pretending $b$ is constant.

### 3.1. Partial Derivative with respect to $b$

Just like we did in the simple example, we differentiate with respect to $b$:
$$ \frac{\partial L}{\partial b} = 2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot (-1) $$
$$ \frac{\partial L}{\partial b} = -2 \sum_{i=1}^{n} (y_i - mx_i - b) $$

### 3.2. Partial Derivative with respect to $m$

Now we differentiate with respect to $m$. The derivative of the inside term $(-mx_i)$ with respect to $m$ is $(-x_i)$:
$$ \frac{\partial L}{\partial m} = 2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot (-x_i) $$
$$ \frac{\partial L}{\partial m} = -2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot x_i $$

## 4. The Final Gradient Descent Update Rules

We apply the exact same intuition (walking downhill) as the simple 1D case, but we apply it to both variables simultaneously using their respective partial derivatives:

$$ b_{\text{new}} = b_{\text{old}} - \eta \times \frac{\partial L}{\partial b} $$
$$ m_{\text{new}} = m_{\text{old}} - \eta \times \frac{\partial L}{\partial m} $$

By repeatedly calculating these partial derivatives across the whole dataset and updating $m$ and $b$ step-by-step (epoch by epoch), we slowly "walk down the mountain" until we reach the global minimum where the loss is lowest.
