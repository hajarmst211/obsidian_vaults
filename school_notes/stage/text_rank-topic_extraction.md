testing dataset: [https://www.kaggle.com/code/maartengr/topic-modeling-arxiv-abstract-with-bertopic](https://www.kaggle.com/code/maartengr/topic-modeling-arxiv-abstract-with-bertopic)
# TextRank
source: https://aclanthology.org/W04-3252.pdf
their dataset:  500 abstracts from the Inspec database (journal papers in Computer Science and Information Technology).
## 1. Mathematical Properties of the Model
TextRank is an unsupervised graph-based ranking algorithm derived from Google's PageRank. It determines the importance of a vertex (node) within a graph by recursively utilizing global information from the entire graph structure.

### The Recommendation (Voting) Model
The underlying concept is "voting." When vertex $V_j$ connects to $V_i$, it casts a vote for $V_i$. The strength of this vote is determined by the overall importance score of the voting vertex $V_j$.

### Unweighted PageRank Formula
For a directed graph $G = (V, E)$, where $In(V_i)$ represents the set of vertices pointing to $V_i$ (predecessors), and $Out(V_j)$ represents the set of vertices that $V_j$ points to (successors):

$$WS(V_i) = (1 - d) + d \sum_{j \in In(V_i)} \frac{WS(V_j)}{|Out(V_j)|}$$

Where:
*   $WS(V_i)$ is the score of vertex $i$.
*   $d$ is a damping factor (typically set to $0.85$), representing the probability of jumping to a random vertex. This helps with representing the global context

### Weighted Graph Formulation
Because natural language entities have variable connection strengths, the authors introduced a weighted formulation to account for edge weights ($w_{ji}$):

$$WS(V_i) = (1 - d) + d \sum_{j \in In(V_i)} \frac{w_{ji}}{\sum_{V_k \in Out(V_j)} w_{jk}} WS(V_j)$$
the results showed that working with unweighted graph is better
### Convergence and Graph Types
*   **Initialization:** Vertices are initialized with arbitrary values (mostly 1) The final score ranking is mathematically independent of the initial values; only the number of iterations required to converge is affected.
*   **Convergence Criterion:** Iterations continue until the difference between scores in successive steps falls below a specified threshold (typically $0.0001$). This usually requires $20$ to $30$ iterations.
*   **Undirected Graphs:** While PageRank was designed for directed web links, the authors demonstrated that TextRank works effectively on undirected graphs. In undirected graphs, the in-degree of a vertex equals its out-degree ($In(V_i) = Out(V_i)$). 
*   **Graph Connectivity:** Highly connected graphs (more edges per vertex) converge faster than sparsely connected graphs.

---

## 2. Step-by-Step Keyword Extraction Algorithm

### Step 1: Tokenization and Part-of-Speech (POS) Tagging
The input text is tokenized and tagged with POS categories. This is a crucial preprocessing step to enable syntactic filtering.
part of speach: ==a category that describes the role a word plays in a sentence==, such as **nouns**, **verbs**, and **adjectives**

### Step 2: Syntactic Filtering
To prevent the graph from growing excessively, only single words (unigrams) that pass a syntactic filter are considered as candidates for graph vertices. Words of other categories and common stopwords are discarded.
syntactic filters only allow the grammatical labels YOU allow them to pass (ex: only nouns and adjectives...)

### Step 3: Graph Construction (Edge Creation)
*   **Vertices ($V$):** Each unique candidate word that passed the syntactic filter becomes a vertex.
*   **Edges ($E$):** An edge is drawn between two vertices if their corresponding words co-occur within a sliding window of $N$ words in the original text.
*   **Graph Mode:** The default and most effective mode for keyword extraction is an **undirected, unweighted graph**.

### Step 4: Iterative Ranking
1. Initialize the score of all vertices to $1.0$.
2. Run the TextRank iterative computation using the chosen formula until convergence :the mathematical "flow" has reached a steady state. The error rate measures how much the scores are still changing from one iteration to the next and the error rate threshold is $\le 0.0001$.

so the thing the scores of each vertex is related to its neighbors, and with each iteration, the scores change and gets centered around the most important words
### Step 5: Post-Processing & Keyphrase Reconstruction
1. Sort the vertices in descending order of their final TextRank scores.
2. Select the top $T$ vertices as potential keywords. (In the paper, $T$ is dynamically set to $1/3$ of the total number of vertices in the graph).
3. **Collapsing Multi-word Keywords:** Go back to the original text. If any of the selected top $T$ words appear adjacent to one another, merge them into a single multi-word keyphrase (e.g., if "linear" and "constraints" are both top-ranked and appear together as "linear constraints" in the text, they are collapsed into a single keyphrase).

### Tip
at the end of this step and after collapsing the vertex, you will probably end up with two or three words/multi-words. but we need to select only one as our topic. So use one of these technics to get ur topic:
- **Fixed Cutoff :** Simply take the top scoring phrases . This is the standard approach for most modern topic extraction pipelines.
- **The "Elbow" Method:** Plot the final scores of your sorted keywords. You will often see a steep drop-off where a few words have very high scores, and the rest flatten out. Cut off your selection at the "elbow" of this curve.
- **Standard Deviation Thresholding:** Calculate the meanand standard deviation of all vertex scores. Only keep vertices with a score greater than a threshold.

---

## 3. Optimal Configurations for Keyword Extraction
Based on the Section 3.2 of the paper, the optimal parameters for topic/keyword extraction are:

*   **Graph Directionality:** **Undirected** graphs perform better than directed graphs. 
*   **Syntactic Filters:** **Nouns and Adjectives only** yielded the highest precision and F-measure. Restricting to only nouns, or expanding to all open-class words (nouns, verbs, adjectives, adverbs), resulted in lower performance. Removing POS filters entirely and relying solely on standard stopword lists produced significantly worse results.
*   **Co-occurrence Window Size ($N$):** **$N = 2$** (a window of two adjacent words) is the optimal window size. Increasing the window size to $3, 5,$ or $10$ led to a steady decrease in precision, as weak associations between words that are far apart introduce noise into the graph.
*   **Damping Factor ($d$):** Set to **$0.85$** (consistent with standard PageRank implementations).
*   **Number of Keywords Extracted ($T$):** Dynamically setting $T$ to **$1/3$ of the total vertices** in the filtered graph works well for short documents (abstracts). For longer documents, a fixed cutoff (e.g., top 10–20 words) or a relative percentage threshold may be used.

---

## 4. Evaluation Methodology and Metrics
### Metrics
The system was evaluated using standard information retrieval metrics:
1.  **Precision ($P$):** The ratio of correctly extracted keywords to the total keywords assigned by the algorithm.
2.  **Recall ($R$):** The ratio of correctly extracted keywords to the total keywords assigned by the human indexers. 
    *   *Note:* The maximum possible recall on this dataset is less than 100% because human indexers sometimes generated keywords that do not explicitly appear in the text (concept abstraction), which an extractive algorithm cannot capture.
3.  **F-measure ($F_1$):** The harmonic mean of Precision and Recall:
    $$F_1 = \frac{2 \cdot P \cdot R}{P + R}$$

### Performance Benchmarks
The optimal TextRank configuration (Undirected, Co-occurrence Window = 2, Nouns & Adjectives filter) achieved the following results on the Inspec dataset:

*   **Average keywords assigned per document:** 13.7
*   **Precision:** 31.2%
*   **Recall:** 43.1%
*   **F-measure:** 36.2%

This unsupervised performance compared favorably with state-of-the-art supervised learning methods trained on annotated datasets, such as Hulth (2003) (which achieved F-measures of 33.9% and 33.0% depending on the candidate selection patterns) and earlier supervised algorithms like GenEx and Kea.

---
### hyperparameters:
#### 1. Sliding Window Size (`window_size`)
* **What it does:** Determines the maximum distance between two words in the text for an edge to be created between them in the graph.
* **Current value:** `2` (only connects adjacent words).
* **Tuning range:** Typically `2` to `8`.
* **Impact:** A larger window size (e.g., `4` or `5`) creates a denser graph with more connections, allowing terms that are separated by minor words to share importance. Too large a window can introduce noise by connecting conceptually unrelated words.

#### 2. Candidate Selection Threshold (`top_count`)
* **What it does:** Dictates how many of the top-ranked words are retained to reconstruct multi-word keywords.
* **Current value:** `len(vertices) // 3` (top 33% of all valid words).
* **Tuning range:** 
  * A smaller percentage (e.g., top `10%` to `20%`).
  * A fixed count (e.g., top `10`, `15`, or `20` words).
* **Impact:** This is often the most critical parameter for balancing metrics. Lowering this threshold reduces the number of candidate words, which typically improves **Precision** (fewer false positives) but may lower **Recall** (missing some valid keywords).

#### 3. POS Tag Filter (`is_valid_pos`)
* **What it does:** Restricts which words are allowed to become vertices in the graph based on their grammatical category.
* **Current configuration:** Nouns and Adjectives (`['NN', 'NNS', 'NNP', 'NNPS', 'JJ', 'JJR', 'JJS']`).
* **Tuning variations:**
  * **Nouns only:** `['NN', 'NNS', 'NNP', 'NNPS']`. This often leads to highly specific keywords but might miss descriptive terms.
  * **Nouns, Adjectives, and Verbs:** Adding verbs (e.g., `['VB', 'VBG']`) can sometimes capture action-oriented key phrases, though it can also introduce noise.

#### 4. Damping Factor (`d`)
* **What it does:** Represents the probability that the PageRank walk continues to a neighboring node rather than jumping to a random node in the graph.
* **Current value:** `0.85` (the standard PageRank default).
* **Tuning range:** `0.70` to `0.95`.
* **Impact:** Lowering the damping factor (e.g., to `0.75`) reduces the influence of highly central hub nodes, slightly distributing scores more evenly across the graph.

#### 5. Maximum Phrase Length
* **What it does:** Limits the number of words that can be joined together during the phrase reconstruction step.
* **Current configuration:** Unlimited (any adjacent top words are merged).
* **Tuning range:** Maximum of `2` (bigrams) or `3` (trigrams).
* **Impact:** Restricting this parameter prevents the generation of unnaturally long phrases (e.g., four or five words combined), which rarely match human-defined keywords.

#### 6. Convergence Threshold and Max Iterations (`threshold`, `max_iter`)
* **What they do:** Control when the iterative PageRank score calculation stops.
* **Current values:** `0.0001` and `50`.
* **Tuning range:** Thresholds between `1e-3` and `1e-6`; max iterations between `20` and `100`.
* **Impact:** These primarily affect execution time rather than keyword quality. If the algorithm converges too early, the scores may not fully stabilize, but minor changes here rarely result in significant differences in evaluation metrics.