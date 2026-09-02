# Gradient Descent

---

## 1. What is Gradient Descent?

Gradient Descent is an optimization algorithm used to find the lowest point (minimum) of a curve. In machine learning, we use it to find the best parameters (like the slope and intercept of a line) that result in the lowest possible error for our model.

- It starts with a random guess.
- It calculates the direction of the steepest decrease in error.
- It takes a small step in that downhill direction.
- It repeats this process until it reaches the bottom (minimum error).

---

## 2. A Concrete Example — Predicting Attendance

Consider a scenario where we want to predict a student's **Attendance** based on their **Study Hours** using a simple straight line.

| Study Hours (Input) | Attendance (Output) |
|:---:|:---:|
| 2 | 30% |
| 5 | 75% |

We want to find a line with a specific **Slope** and **Intercept** that fits this data as closely as possible.

---

## 3. The Concept of Loss (Error)

To know if our guessed line is good or bad, we measure the **Loss**.

The Loss is calculated by taking the difference between the student's **Actual Attendance** and our line's **Predicted Attendance**, squaring that difference so it's always positive, and adding it up for every student.

The entire goal of Gradient Descent is to find the exact Slope and Intercept that make this Loss as close to zero as possible.

---

## 4. A Simpler Scenario — Fixing the Slope

To understand how the algorithm works, let's pretend we already know the perfect Slope is exactly ten. Now we only need to find the best Intercept.

If we plot our Loss against different guesses for the Intercept, it forms a U-shaped curve. The bottom of the U is our target.

> If you drop a physical ball on the side of this U-shaped curve, it naturally rolls downhill to the bottom. Gradient Descent does the exact same thing mathematically.

---

## 5. The Update Rule (Walking Downhill)

To move our guess closer to the bottom, we look at the **steepness** of the curve at our current guess. 

- If the steepness goes UP to the right (a positive steepness), we must move LEFT (the negative direction) to go down.
- If the steepness goes UP to the left (a negative steepness), we must move RIGHT (the positive direction) to go down.

This logical deduction gives us our core rule:

**New Intercept = Old Intercept - (Step Size * Steepness)**

By always subtracting the steepness, we guarantee that we are moving in the opposite, downhill direction.

---

## 6. The Step Size (Learning Rate)

The **Step Size** controls how big of a jump we make in each update.

- If it is **too big**, we might jump completely over the minimum and land higher up on the other side of the U-curve.
- If it is **too small**, it will take a very long time and too many steps to finally reach the bottom.

---

## 7. The Real Problem — Two Unknowns

In reality, we don't know the Slope either. We have to find both the Slope and the Intercept simultaneously. Our simple U-shape becomes a 3D bowl shape.

To solve this, we use a technique that separates the two:
1. We calculate the downhill steepness for the **Intercept** while pretending the Slope is frozen.
2. We calculate the downhill steepness for the **Slope** while pretending the Intercept is frozen.

---

## 8. The Final Algorithm

We apply our downhill update rule to both variables at the exact same time:

1. **New Intercept = Old Intercept - (Step Size * Steepness for Intercept)**
2. **New Slope = Old Slope - (Step Size * Steepness for Slope)**

We repeat these updates over and over (these loops are called **Epochs**) until our values stop changing. When they stop changing, it means we have successfully reached the bottom of the bowl and found our perfect line!

---

## Summary

| Concept | Key Point |
|---|---|
| **Gradient Descent** | An algorithm that walks downhill to find the lowest point of an error curve. |
| **Loss** | The total measure of how wrong our current predictions are. |
| **Steepness**| Tells us which direction is downhill at any given point. |
| **Step Size (Learning Rate)**| Determines how big of a jump we take downhill in each step. |
| **Epoch** | One full cycle of updating all our guesses. |
| **Goal** | Find the Slope and Intercept that produce the absolute minimum Loss. |
