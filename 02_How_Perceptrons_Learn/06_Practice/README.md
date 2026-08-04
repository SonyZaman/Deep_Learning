#  Practice Questions & Answers

---

## Part 1: Perceptron Basics

**Q1. In one sentence, what problem does a perceptron solve?**  
**Answer:** A perceptron solves binary classification problems that are linearly separable.

**Q2. What are the three main components of a perceptron?**  
**Answer:** 
1. **Inputs & Weights:** The features multiplied by their respective importance.
2. **Summation Function:** Adds up the weighted inputs and the bias ($z = \sum w_i x_i + b$).
3. **Activation Function:** A step function that converts the sum into a discrete output (e.g., `0` or `1`).

**Q3. Is a perceptron a classifier or a regressor? Explain briefly.**  
**Answer:** It is a **classifier** (specifically a binary classifier). Because it uses a step activation function, its final output is restricted to discrete categorical values (like `0` or `1`), rather than a continuous number.

---

## Part 2: Weight Updating & Learning

**Q4. Why do we need to update weights in a perceptron?**  
**Answer:** We update weights to minimize prediction errors. By tweaking the weights, the perceptron incrementally adjusts its decision boundary until it correctly separates the classes.

**Q5. What happens if we never update the weights during training?**  
**Answer:** The perceptron will never learn. It will continue making predictions based on its initial random guess and will likely always be wrong.

**Q6. Which two quantities decide how much the weight changes?**  
**Answer:** The **learning rate ($\eta$)** and the **input value ($x_i$)**. (The magnitude of the error also dictates direction and scale, but for binary classification, the error is usually just `1`, `-1`, or `0`).

**Q7. What does the term $(y - \hat{y})$ represent in perceptron learning?**  
**Answer:** It represents the **prediction error**. It is the difference between the Actual Output ($y$) and the Predicted Output ($\hat{y}$).

**Q8. If the prediction is correct, will weights change? Why?**  
**Answer:** **No.** If the prediction is correct, $y = \hat{y}$, making the error $(y - \hat{y}) = 0$. Since the weight update formula multiplies by the error, the entire update term becomes `0`.

**Q9. What role does the learning rate ($\eta$) play intuitively?**  
**Answer:** It controls the **step size** of learning. A high $\eta$ makes the model adjust weights drastically (learning fast but risking overshooting the solution), while a low $\eta$ adjusts weights slightly (learning slowly but steadily).

---

## Part 3: AND Gate & Geometry

**Q10. Write the AND gate truth table.**  
**Answer:**  

| $x_1$ | $x_2$ | $y$ (AND) |
|:---:|:---:|:---:|
| 0 | 0 | **0** |
| 0 | 1 | **0** |
| 1 | 0 | **0** |
| 1 | 1 | **1** |

**Q11. Why is the AND gate linearly separable?**  
**Answer:** Because if you plot its points on a 2D graph, you can draw a single straight line to perfectly separate the single `1` output from the three `0` outputs.

**Q12. Draw a rough decision boundary that separates AND gate outputs.**  
**Answer:**  
Imagine a 2D graph. A straight diagonal line cutting off the top right corner perfectly separates $(1,1)$ from $(0,0), (0,1),$ and $(1,0)$.
```text
 y │
 1 │   0       1 (Target)
   │     \
 0 │   0  \    0
 ──┼───────\───────── x
   0       1
```

---

## Part 4: Bias & Implementation Details

**Q13. Why do we add a bias term in the perceptron?**  
**Answer:** The bias shifts the decision boundary away from the origin $(0,0)$. Without a bias, the decision line is forced to pass exactly through the origin, which limits the perceptron's ability to solve many problems.

**Q14. Why is bias often added after summation?**  
**Answer:** Bias acts as a mathematical threshold shift. Instead of checking if the weighted sum is greater than some arbitrary threshold ($w \cdot x \ge \text{threshold}$), we subtract/add the bias so the activation function can simply check if the result is greater than zero ($w \cdot x + b \ge 0$).

**Q15. Why do we initialize weights with small random values?**  
**Answer:** To break symmetry and give the model a starting point to learn from. Initializing with large values can cause huge initial errors and make the decision boundary jump around too erratically.

---

## Part 5: Python / Math Implementation

**Q16. What is the purpose of `np.dot(X, w)` in perceptron code?**  
**Answer:** It efficiently calculates the weighted sum of all inputs simultaneously using vectorization (matrix multiplication), avoiding slow `for` loops.

**Q17. Why is matrix transpose used in weight updating?**  
**Answer:** When dealing with multiple samples (a batch) at once, we use the transpose of the input matrix $X^T$ to correctly align the dimensions with the error vector. This allows us to calculate the updates for all weights across all samples in one single mathematical operation.

**Q18. What will happen if we remove the activation function?**  
**Answer:** The perceptron becomes a simple linear regression model. Instead of predicting discrete classes like `0` or `1`, it will output a continuous number.

---

## Part 6: Limitations & XOR Gate

**Q19. Why can the OR gate be solved using a single perceptron?**  
**Answer:** Because, like the AND gate, its outputs (one `0` and three `1`s) are linearly separable.

**Q20. What is common between AND and OR gates from a geometry view?**  
**Answer:** Both are **linearly separable** problems. You can perfectly split their classes using one straight line.

**Q21. Does XOR need more features or more layers? Why?**  
**Answer:** It needs **more layers**. The problem is inherently non-linear, so we need a Multi-Layer Perceptron (MLP) to combine multiple straight lines and form a complex decision boundary.

**Q22. What does linearly separable mean?**  
**Answer:** It means you can draw a single straight line (or hyperplane in higher dimensions) to perfectly separate two different classes of data points.

**Q23. Why can’t a single straight line separate XOR data?**  
**Answer:** Because the outputs of the same class lie on opposite diagonal corners (a criss-cross pattern). Any single straight line will accidentally mix the classes together.

**Q24. What change is required to solve XOR successfully?**  
**Answer:** We must move away from a single perceptron and use a **Multi-Layer Perceptron (MLP)** by adding hidden layers.

---

## Part 7: Strengths & Weaknesses

**Q25. List two strengths of a perceptron.**  
**Answer:** 
1. Extremely fast and computationally cheap.
2. Guaranteed to converge (find the optimal solution) if the dataset is linearly separable.

**Q26. List two limitations of a perceptron.**  
**Answer:** 
1. Completely fails on non-linearly separable problems (like XOR).
2. It outputs hard boundaries (`0` or `1`), not probabilities, so it cannot express "how confident" it is in a prediction.

**Q27. When should we avoid using a single-layer perceptron?**  
**Answer:** Whenever the data is complex and non-linear (e.g., image recognition, natural language processing, or any problem with non-linearly separable data).

---

##  Part 8: Mini Challenge (Very Easy)

**Q28. Given:**
- $x_1 = 1,\ x_2 = 1$
- $\text{weights} = [1, 1]$
- $\text{bias} = -1.5$

**Will the perceptron output be 1 or 0? (Show reasoning in one line)**

**Answer: 1**  
**Reasoning:** $z = (w_1 \cdot x_1) + (w_2 \cdot x_2) + \text{bias} = (1 \cdot 1) + (1 \cdot 1) - 1.5 = 2 - 1.5 = 0.5$. Since $0.5 \ge 0$, the step function outputs **1**.
