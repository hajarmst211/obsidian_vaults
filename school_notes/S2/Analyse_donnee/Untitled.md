
### 1. Simple Linear Regression (SLR)
**The Math:** $maxO3 = \beta_0 + \beta_1 \times T12 + \epsilon$
*   **Correlation ($r$):** Know that $r$ ranges from -1 to 1. 
    *   Close to 1: Strong positive linear relationship.
    *   Close to 0: No linear relationship.
*   **The Equation:** From an R summary, look at the "Estimate" column.
    *   `(Intercept)` is $\hat{\beta}_0$.
    *   `T12` is $\hat{\beta}_1$.
    *   *Exam tip:* Always write the full equation: $\widehat{maxO3} = \text{Value}_0 + \text{Value}_1 \times T12$.

### 2. Interpreting the `summary(lm(...))` Table
This is the most likely exam question. You must know these four columns:
1.  **Estimate:** The coefficients of the line.
2.  **Std. Error:** The standard deviation of the estimate.
3.  **t value:** (Estimate / Std. Error).
4.  **Pr(>|t|):** The **p-value**.
    *   If $p < 0.05$: The variable is **significant** (Reject $H_0: \beta_i = 0$).
    *   If $p > 0.05$: The variable does not significantly help predict $maxO3$.

### 3. Global Model Quality
*   **R-squared ($R^2$):** "Coefficient of determination." It tells you the percentage of the variance of $maxO3$ explained by $T12$. (e.g., $0.75 = 75\%$).
*   **F-statistic:** Tests if the *whole model* is useful. 
    *   $H_0$: All coefficients are zero (Model is useless).
    *   $H_1$: At least one coefficient is non-zero.
    *   Look at the "p-value" at the very bottom of the R summary.

### 4. Analysis of Variance (ANOVA Table)
You should know the relationship: **SST = SSR + SSE**
*   **SST (Total):** Total variation in data.
*   **SSR (Regression):** Variation explained by the model.
*   **SSE (Error):** Residual/unexplained variation.
*   **Mean Square (MS):** Sum of Squares divided by Degrees of Freedom ($df$).

### 5. Model Validation (Assumptions)
On paper, they might show you diagnostic plots. You need to recognize:
*   **Normality of Residuals:** Look at the **QQ-Plot**. Points should follow the diagonal line. Statistical tests: **Shapiro-Wilk** or **Kolmogorov-Smirnov** ($H_0$: Residuals are normal).
*   **Homoscedasticity (Constant Variance):** Look at "Residuals vs Fitted" plot. The spread should be random/constant. If it looks like a "fan" or "funnel," it’s heteroscedastic. Test: **Breusch-Pagan**.
*   **Independence:** Test: **Durbin-Watson**. If the value is near 2, there is no autocorrelation.
*   **Influential Points:** Look at **Cook’s Distance**. If a point has a distance > 1 (or is far from the others), it is an "outlier" that pulls the regression line too much toward it.

### 6. Multiple Linear Regression (MLR)
*   **Adjusted $R^2$:** In MLR, always look at **Adjusted $R^2$**, not the Multiple $R^2$. The adjusted version penalizes you for adding useless variables.
*   **Variable Selection:**
    *   **RegBest / Stepwise:** This is the process of removing non-significant variables (where $p > 0.05$) one by one to find the "parsimonious" (simplest best) model.
*   **Multicollinearity:** If variables are too correlated with each other (e.g., $T9$ and $T12$), it ruins the model.

### 7. Prediction
You will likely be asked to calculate a value manually.
*   *Example:* If the model is $maxO3 = 20 + 1.5 \times T12$.
*   *Question:* Predict $maxO3$ for $T12 = 30$.
*   *Calculation:* $20 + (1.5 \times 30) = 65$.

---

### Summary of Key Tests for the Exam:

| Test Name | Purpose | Null Hypothesis ($H_0$) | Desired result for a "good" model |
| :--- | :--- | :--- | :--- |
| **Student t-test** | Check individual variable | Coefficient = 0 | **p < 0.05** (Significant) |
| **Fisher F-test** | Check global model | Model is useless | **p < 0.05** (Useful) |
| **Shapiro-Wilk** | Normality of residuals | Residuals are normal | **p > 0.05** (Normal) |
| **Breusch-Pagan** | Variance of residuals | Variance is constant | **p > 0.05** (Homoscedastic) |
| **Durbin-Watson** | Autocorrelation | No correlation | **Value $\approx$ 2** |

**How to study:** Go through the TP instructions and for every "Test" mentioned (e.g., Shapiro, Cook, ANOVA), write down on a cheat sheet: **"What does $H_0$ mean, and what does a p-value < 0.05 mean for this specific test?"**