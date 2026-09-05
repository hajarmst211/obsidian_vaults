### 1. Average Semantic Similarity (The Core Metric)
* **What it compares:** The conceptual meaning of your **Predicted Topic** vs. the **Actual Label**.
* **How it works:** It uses vector embeddings to calculate a score from `0.0` (completely unrelated) to `1.0` (identical meaning). 
* **Example:** Comparing `"sentiment analysis model"` to `"Computation and Language"` yields a high similarity score (e.g., `0.60`), even though they share no common words.

---

### 2. Precision
* **What it compares:** **Correct Predictions** vs. **Total Predictions Made**.
* **What it tells you:** Out of all the topics the model generated, how many were actually relevant (i.e., scored above your similarity threshold of `0.45`)?
* **Formula:** $\frac{\text{Predictions above threshold}}{\text{Total topics predicted}}$

---

### 3. Recall
* **What it compares:** **Correct Predictions** vs. **Total Expected Labels in Dataset**.
* **What it tells you:** Out of all the actual document categories in your dataset, how many did the model successfully cover?
* **Formula:** $\frac{\text{Predictions above threshold}}{\text{Total actual labels in dataset}}$

---

### 4. F1-Measure
* **What it compares:** **Precision** vs. **Recall**.
* **What it tells you:** The balanced average (harmonic mean) of the two. It gives you a single overall performance score, ensuring the model isn't cheating by maximizing only one of the metrics.