# Gradient Descent: The Mathematical Foundation

## 1. The Goal: Minimizing Loss

In Linear Regression, our model tries to fit a line $y = mx + b$ to the data points. 
To evaluate how good this line is, we calculate the **Loss** (or Error). A common way to measure this is the Sum of Squared Errors:

$$ L = \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 $$

Substituting our line prediction equation $\hat{y}_i = m x_i + b$ into the loss function:

$$ L(m,b) = \sum_{i=1}^{n} (y_i - mx_i - b)^2 $$

If we plot this function, it creates a 3D bowl-shaped (convex) surface, or a 2D parabola if we keep one parameter constant. Our ultimate goal is to find the exact values of $m$ and $b$ that give the lowest possible loss (the global minimum at the bottom of the bowl).

## 2. The Intuition: Taking Downhill Steps

Imagine a person standing on a mountain (representing our loss curve) trying to reach the valley bottom in the dark. They can only feel the slope (gradient) of the ground under their feet to decide which way to step:
- If the slope is **positive** (going up to the right), they need to step left (negative direction).
- If the slope is **negative** (going up to the left), they need to step right (positive direction).

This logical deduction is represented by the update rule:

$$ \text{new\_value} = \text{old\_value} - (\text{step\_size} \times \text{slope}) $$

*   **Slope**: Indicates the direction we should move to increase the error. By subtracting it, we move in the opposite direction (towards the minimum).
*   **Step Size ($\eta$)**: This is the **Learning Rate**. It determines how big of a step we take.

## 3. The Math: Finding the Slopes (Partial Derivatives)

To find the slope of the loss function, we need to take the partial derivative of our loss function $L(m, b)$ with respect to each parameter we want to optimize.

### 3.1. Finding the Gradient for the Intercept ($b$)

Let's find how the loss changes when we tweak $b$, denoted as $\frac{\partial L}{\partial b}$.
Using the chain rule from calculus on $L = \sum (y_i - mx_i - b)^2$:

1.  Bring down the exponent $2$.
2.  Take the derivative of the inside term $(y_i - mx_i - b)$ with respect to $b$, which is $-1$.

$$ \frac{\partial L}{\partial b} = 2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot (-1) $$

Simplifying this gives us the gradient for $b$:

$$ \frac{\partial L}{\partial b} = -2 \sum_{i=1}^{n} (y_i - mx_i - b) $$

The **Update Rule for $b$** then becomes:

$$ b_{new} = b_{old} - \eta \cdot \frac{\partial L}{\partial b} $$

### 3.2. Finding the Gradient for the Slope ($m$)

Now, let's find how the loss changes when we tweak $m$, denoted as $\frac{\partial L}{\partial m}$.
Applying the same chain rule on $L = \sum (y_i - mx_i - b)^2$:

1.  Bring down the exponent $2$.
2.  Take the derivative of the inside term $(y_i - mx_i - b)$ with respect to $m$, which is $-x_i$.

$$ \frac{\partial L}{\partial m} = 2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot (-x_i) $$

Simplifying this gives us the gradient for $m$:

$$ \frac{\partial L}{\partial m} = -2 \sum_{i=1}^{n} (y_i - mx_i - b) \cdot x_i $$

The **Update Rule for $m$** then becomes:

$$ m_{new} = m_{old} - \eta \cdot \frac{\partial L}{\partial m} $$

---

## 4. Putting it Together: The Gradient Descent Algorithm

To train a model from scratch, we use the formulas derived above in a loop:

1.  **Initialize**: Start with random guesses for $m$ and $b$ (e.g., $m=1, b=0$).
2.  **Calculate Gradients**: Use the formulas above to calculate $\frac{\partial L}{\partial m}$ and $\frac{\partial L}{\partial b}$ based on the entire training dataset.
3.  **Update Parameters**: Calculate $m_{new}$ and $b_{new}$ by taking a step dictated by the learning rate $\eta$.
4.  **Iterate**: Repeat steps 2 and 3 for a set number of **epochs** (loops), slowly walking down the mountain until the values converge at the minimum loss.
