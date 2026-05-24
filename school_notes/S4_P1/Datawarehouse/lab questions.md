### 1. Granularity (The "Level of Detail" Question)
*   **What is it?** The atomic level of a single row in a fact table.
*   **The Key Trade-off:** 
    *   **Fine Granularity:** (e.g., individual flight ticket) Allows for the most detailed "drilling down" in your cube, but requires massive storage and slower performance.
    *   **Coarse Granularity:** (e.g., daily sales total) Faster and smaller, but limits the business user's ability to analyze specific transactions (you lose the ability to see who bought what).
*   **Rule of Thumb:** Always try to design at the finest (atomic) grain possible, as it is easier to roll up data into summaries than it is to break apart summarized data.

### 2. Slowly Changing Dimensions (SCDs)
*   **Type 1 (Overwrite):** You replace the old value with the new one (e.g., correcting a typo in a customer's name). You lose historical tracking.
*   **Type 2 (Add a New Row):** You keep the old version and add a new row with a new Surrogate Key. **Benefit:** This allows you to track historical reporting (e.g., "What was the customer's sales performance when they lived in New York vs. after they moved to California?").
*   **Type 3 (Add a New Column):** You keep the "current" and "previous" value in the same row. Good for simple comparisons but limited in history.

### 3. Conformed Dimensions (The "Unified Product" Question)
*   **Concept:** A dimension (like Product or Date) that is shared across multiple fact tables (e.g., Flight Sales and Hotel Sales).
*   **Why use them?** They allow you to perform "drill-across" reports. If a user wants to know total revenue from a specific flight + hotel package, the data must be linked by a common, identical dimension.
*   **Drawbacks:**
    *   **Maintenance Complexity:** If you change the logic for a "product category," you must update it everywhere.
    *   **Sparse Attributes:** If you cram everything into one "Product" table, you get many `NULL` values (e.g., a "Flight" product won't have a "Bed-size" attribute), which can confuse the user and bloat the table.

### 4. Designing the Bus Matrix
*   **The Goal:** It’s a map that aligns **Business Processes** (rows) with **Conformed Dimensions** (columns).
*   **Why do it?** It forces you to define which dimensions are "common" to which processes. If you find a dimension that only applies to one process, it might be a "junk dimension" or a degenerate dimension.
*   **Justification:** When designing, you must justify your model by showing that it answers specific business questions. "We chose this grain because the business needs to analyze revenue by flight time (for the date dimension) and by aircraft type (for the product dimension)."

### 5. Cube vs. Relational (SSAS vs. SQL)
*   **Why use a Cube (Multidimensional/OLAP)?** 
    *   **Performance:** Pre-calculating aggregations (sums/averages) means the user gets instant results on massive data.
    *   **User Friendliness:** It organizes complex database schemas into "measures" and "dimensions," which are easier for non-technical users to drag and drop in tools like Excel or SSRS.
*   **When to avoid it?** If the data changes every second (Real-time analytics), cubes are bad because they require a "processing" step to update the data.

