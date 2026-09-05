# Source 1: 
link: [https://www.researchgate.net/profile/Sathish-Kumar-26/publication/342288300_A_review_of_topic_modeling_methods/links/5ef14381299bf1faac6f22f9/A-review-of-topic-modeling-methods.pdf?_tp=eyJjb250ZXh0Ijp7ImZpcnN0UGFnZSI6InB1YmxpY2F0aW9uIiwicGFnZSI6InB1YmxpY2F0aW9uIn19](https://www.researchgate.net/profile/Sathish-Kumar-26/publication/342288300_A_review_of_topic_modeling_methods/links/5ef14381299bf1faac6f22f9/A-review-of-topic-modeling-methods.pdf?_tp=eyJjb250ZXh0Ijp7ImZpcnN0UGFnZSI6InB1YmxpY2F0aW9uIiwicGFnZSI6InB1YmxpY2F0aW9uIn19)

### resume:
- **Self-Aggregation-Based Topic Model (SATM):** A "pseudo-document" approach that clusters short texts with similar topics into longer pseudo-documents during the topic inference stage.
- **Pseudo-Document-Based Topic Model (PTM):** A simplified, less computationally intensive alternative to SATM that condenses the short-text self-aggregation into a single generative process where pseudo-documents act as hybrid topics.
- **Sparsified Pseudo-Document-Based Topic Model (SPTM):** A variation of PTM designed for scenarios with a small number of pseudo-documents, utilizing a Spike and Slab prior to keep topics specific and meaningful.

##### MU, BTM, DSTM
- assumes that the entire document is mapped into one single topic (unlike standard LDA: multiple topics)
- **Mixture of Unigrams (MU):** If a single document is very short, it is highly unlikely to contain a complex blend of 10 different topics. MU simplifies the extraction process by forcing the model to assign the entire single document to a single topic. While highly effective, the authors note it is sometimes too simplistic because even a short text can occasionally touch on more than one topic.
- **Biterm Topic Model (BTM):** Instead of analyzing the words within the boundary of a single document, BTM extracts topics from "biterms" (pairs of words) across the entire corpus. This bypasses document boundaries entirely to learn global topics, which can then be applied back to single documents.
- **Dual Sparse Topic Model (DSTM):** Traditional LDA uses Dirichlet priors, which tend to smooth word distributions. DSTM uses "Spike and Slab" priors to force sparsity. This helps the algorithm focus on extracting a highly specific, small set of terms and topics for each individual document, stripping away the "noise.
##### code:

Self-Aggregation-Based Topic Model (SATM): [https://github.com/WHUIR/SATM](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2FWHUIR%2FSATM)
Code of Biterm Topic Model:https://github.com/xiaohuiyan/BTM

# Source 2: LDA (latent Dirichlet Allocation)
source:https://www.researchgate.net/publication/398877584_Topic_Modeling_on_Online_NewsPortal_Using_Latent_Dirichlet_Allocation_LDA#fullTextFileContent

- It assumes that the document is a mixture of multiple topics

The base paper: **"Topic Modeling on Online News Portal Using Latent Dirichlet Allocation (LDA)"** (Azhari & Fahlevvi, 2022)

---

### 1. The Core Concept of LDA (According to the Paper)
LDA is an unsupervised machine learning algorithm used to extract hidden (latent) themes or topics from a collection of documents (a corpus). 

The paper describes the model through these core theoretical characteristics:
*   **Generative Probabilistic Model:** LDA assumes that documents are represented as a mixture of various latent topics, and each topic is characterized by a specific distribution of words.
*   **Observed vs. Latent Variables:** 
    *   **Observed variables** are the actual documents and words written in the news portal.
    *   **Latent (hidden) variables** are the topics assigned to each word.
*   **Complexity:** Because of complex mathematical distributions, estimating the posterior distributions for LDA models is difficult to perform manually, which is why computational algorithms are required to compute them.

---

### 2. The Implementation Workflow
The paper's 8-stage research methodology to implement LDA on online news data (from the Indonesian news portal *tempo.co*):

#### Stage 1: Data Collection

#### Stage 2: Preprocessing
*   Removing punctuation, numbers, and special characters.
*   Converting text to lowercase.
*   Removing stop words
*   Stemming or lemmatization.

#### Stage 3: N-gram Formation
To capture context, words are grouped into sequences. This step creates:
*   **Unigrams:** Single words.
*   **Bigrams:** Two-word combinations (e.g., "social_media").
*   **Trigrams:** Three-word combinations.
--> helps recognize when <mark style="background: #FFB8EBA6;">words frequently appear together as a single concept</mark>.

#### Stage 4: Dictionary Representation
The preprocessed words and n-grams are mapped into a unique dictionary. Each unique word/phrase is assigned a specific ID, converting the textual data into a structured vocabulary list.

#### Stage 5: Weighting
The words in the documents are converted into a numerical format, typically using a Bag-of-Words (BoW) model or Term Frequency-Inverse Document Frequency (TF-IDF). 

#### Stage 6: Topic Model Validation
Before finalizing the topics, the model's parameters must be validated. This is done by testing different combinations of parameters (try: the number of topics, the number of training passes...) and evaluating them using quantitative metrics(perplexity and coherence value).
#### Stage 7: Topic Model Formation
The LDA algorithm is executed using the prepared dictionary and weighted document representations. During this stage, the model iteratively associates words with latent topics based on probability distributions.
#### Stage 8: Topic Modeling Results
The output is generated, which consists of a set of topics. Each topic is represented by a cluster of words that have high probability weights within that specific topic.

---

### 3. Key Evaluation Metrics Mentioned
1.  **Perplexity:** It tells you how many equally likely options a model is effectively choosing between at any given time.  
	lower perplexity : the probability distribution is sharp, focused, and less surprised by real events.
	 higher perplexity : the data is spread out, uncertain, and chaotic.
2.  **Coherence Value:**  measures the semantic similarity between high-scoring words in a topic. It evaluates whether the words composing a topic actually make sense together to a human reader.

---

### 4. Specific Results and Configuration
According to the study's findings, the performance of the topic model is highly dependent on two parameters: the **number of topics** and the **number of passes** (iterations over the entire corpus during training).

*   **Optimal Configuration:** The best results in this study were obtained using:
    *   **Number of Topics ($k$):** 5 topics
    *   **Number of Passes:** 20 passes
*   **Coherence Score:** This configuration achieved a coherence value of **0.53**.
*   **Conclusion on Stability:** Based on standard coherence metrics, a value of 0.53 is considered relatively stable, indicating that the model successfully grouped semantically related words into cohesive, interpretable topics from the news portal.
#### Tips to use LDA for one document:
- **Paragraphs:** Treat each paragraph as a separate document.
- **Sentences:** Treat each sentence as a document (though sentences can sometimes be too short for LDA to perform well).
- **Sliding Windows:** Divide the text into fixed-size chunks of words (e.g., every 100 words).

#### varient:
Probabilistic Latent Semantic Analysis (PLSA)  


---
#### single document analysis methods:
- **F-IDF (Term Frequency-Inverse Document Frequency):** While TF-IDF also usually requires a background corpus to calculate the "Inverse Document Frequency," you can use a pre-calculated background corpus to find which words in your single document are uniquely important.
- **TextRank / SingleRank:** These are graph-based algorithms (inspired by Google's PageRank) designed specifically for keyphrase extraction and summarization of a single document. They do not require a corpus.
- **YAKE! (Yet Another Keyword Extractor):** A light-weight, unsupervised keyword extraction tool that relies on text features statistical features from a single document to find important keywords.
- **Pre-trained Embeddings / Transformers:** You can use pre-trained models (such as BERT or other sentence-transformer models) to extract semantic meaning, summarize, or cluster sentences within a single document without needing to train a model from scratch

----
# TextRank
### 1. Graph-Based & Statistical Extraction 
These algorithms analyze the internal structure, word frequency, and word relationships of a single text to find the most central terms. They require zero training data and run quickly.

#### **TextRank**
*   **How it works:** Based on Google's PageRank algorithm, TextRank constructs a graph where words are vertices, and edges are drawn between words that co-occur within a specified sliding window (e.g., 2 to 5 words). It then iteratively ranks the vertices. The highest-ranked words/phrases represent the primary "topics" or key concepts of the document.
*   **Suitability for Scenario A:** Highly effective because it relies entirely on the lexical relationships within that single document.
*   **Scientific Research:** *Mihalcea, R., & Tarau, P. (2004). "TextRank: Bringing Order into Texts."*
*   **Link to explore:** [TextRank Research Paper (ACL Anthology)](https://aclanthology.org/W04-3252.pdf)

#### **YAKE! (Yet Another Keyword Extractor)**
*   **How it works:** YAKE! is a light-weight, unsupervised statistical method that does not rely on external dictionaries or corpora. It evaluates individual term features (such as casing, word position within the text, word frequency, and context with surrounding words) to calculate a score for each candidate phrase.
*   **Suitability for Scenario A:** Designed specifically for single-document extraction, meaning it does not lose accuracy when running on one isolated text.
*   **Scientific Research:** *Campos, R., et al. (2020). "YAKE! Unsupervised Light-Weight Keyword Extraction."*
*   **Link to explore:** [YAKE! Research Paper on arXiv](https://arxiv.org/abs/1801.03126)

---

### 2. Embedding-Based Semantic Alignment (Uses Pre-trained Models)
These methods leverage pre-trained language models to understand the semantic meaning of the entire document and extract sub-phrases that closely match that meaning.

#### **KeyBERT (using Sentence-BERT)**
*   **How it works:** This method uses a pre-trained transformer model (like BERT) to create a vector embedding of the entire document. It then creates embeddings for all individual words or phrases (N-grams) within that same document. By calculating the cosine similarity between the document embedding and the phrase embeddings, it extracts the phrases that best represent the overall topic of the text.
*   **Suitability for Scenario A:** Excellent if you want the extracted "topic" to be semantically representative of the whole text, even if the exact topic word does not appear frequently.
*   **Scientific Research:** *Reimers, N., & Gurevych, I. (2019). "Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks."* (This is the underlying model that powers embedding-based single-document extraction).
*   **Link to explore:** [Sentence-BERT Research Paper on arXiv](https://arxiv.org/abs/1908.10084)

---

### 3. Zero-Shot Topic Classification (Inference-Based)
If you have a set of potential topics in mind (e.g., "Sports," "Politics," "Technology") and want to assign the best one to your single document without training a classifier, zero-shot classification is the standard approach.

#### **NLI-based Zero-Shot Classification**
*   **How it works:** This approach reframes topic extraction as a Natural Language Inference (NLI) task using a pre-trained transformer model (such as BART or DeBERTa). The model takes the single document as a premise and tests it against hypotheses like *"This text is about [Topic]"*. It calculates the probability of entailment for each candidate topic you provide.
*   **Suitability for Scenario A:** Ideal if you want to classify single documents into a standardized set of target topics on the fly, without needing any training examples.
*   **Scientific Research:** *Yin, W., Hay, J., & Roth, D. (2019). "Benchmarking Zero-Shot Text Classification: Datasets, Evaluation and Entailment Approach."*
*   **Link to explore:** [Zero-Shot Text Classification Research Paper on arXiv](https://arxiv.org/abs/1909.00161)


---
# TextRank
source: https://aclanthology.org/W04-3252.pdf

## 1. Mathematical Properties of the Model
TextRank is an unsupervised graph-based ranking algorithm derived from Google's PageRank. It determines the importance of a vertex (node) within a graph by recursively utilizing global information from the entire graph structure.

### The Recommendation (Voting) Model
The underlying concept is "voting." When vertex $V_j$ connects to $V_i$, it casts a vote for $V_i$. The strength of this vote is determined by the overall importance score of the voting vertex $V_j$.

### Unweighted PageRank Formula
For a directed graph $G = (V, E)$, where $In(V_i)$ represents the set of vertices pointing to $V_i$ (predecessors), and $Out(V_j)$ represents the set of vertices that $V_j$ points to (successors):

$$WS(V_i) = (1 - d) + d \sum_{j \in In(V_i)} \frac{WS(V_j)}{|Out(V_j)|}$$

Where:
*   $WS(V_i)$ is the score of vertex $i$.
*   $d$ is a damping factor (typically set to $0.85$), representing the probability of jumping to a random vertex.

### Weighted Graph Formulation
Because natural language entities have variable connection strengths, the authors introduced a weighted formulation to account for edge weights ($w_{ji}$):

$$WS(V_i) = (1 - d) + d \sum_{j \in In(V_i)} \frac{w_{ji}}{\sum_{V_k \in Out(V_j)} w_{jk}} WS(V_j)$$

### Convergence and Graph Types
*   **Initialization:** Vertices are initialized with arbitrary values (e.g., $1.0$). The final score ranking is mathematically independent of the initial values; only the number of iterations required to converge is affected.
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

### Step 3: Graph Construction (Edge Creation)
*   **Vertices ($V$):** Each unique candidate word that passed the syntactic filter becomes a vertex.
*   **Edges ($E$):** An edge is drawn between two vertices if their corresponding words co-occur within a sliding window of $N$ words in the original text.
*   **Graph Mode:** The default and most effective mode for keyword extraction is an **undirected, unweighted graph**.

### Step 4: Iterative Ranking
1. Initialize the score of all vertices to $1.0$.
2. Run the TextRank iterative computation using the chosen formula until convergence (error rate threshold $\le 0.0001$).

### Step 5: Post-Processing & Keyphrase Reconstruction
1. Sort the vertices in descending order of their final TextRank scores.
2. Select the top $T$ vertices as potential keywords. (In the paper, $T$ is dynamically set to $1/3$ of the total number of vertices in the graph).
3. **Collapsing Multi-word Keywords:** Go back to the original text. If any of the selected top $T$ words appear adjacent to one another, merge them into a single multi-word keyphrase (e.g., if "linear" and "constraints" are both top-ranked and appear together as "linear constraints" in the text, they are collapsed into a single keyphrase).

---

## 3. Optimal Configurations for Keyword Extraction
Based on the empirical evaluations presented in Section 3.2 of the paper, the optimal parameters for topic/keyword extraction are:

*   **Graph Directionality:** **Undirected** graphs perform better than directed graphs. Adding direction (either forward or backward based on text flow) degraded performance.
*   **Syntactic Filters:** **Nouns and Adjectives only** yielded the highest precision and F-measure. Restricting to only nouns, or expanding to all open-class words (nouns, verbs, adjectives, adverbs), resulted in lower performance. Removing POS filters entirely and relying solely on standard stopword lists produced significantly worse results.
*   **Co-occurrence Window Size ($N$):** **$N = 2$** (a window of two adjacent words) is the optimal window size. Increasing the window size to $3, 5,$ or $10$ led to a steady decrease in precision, as weak associations between words that are far apart introduce noise into the graph.
*   **Damping Factor ($d$):** Set to **$0.85$** (consistent with standard PageRank implementations).
*   **Number of Keywords Extracted ($T$):** Dynamically setting $T$ to **$1/3$ of the total vertices** in the filtered graph works well for short documents (abstracts). For longer documents, a fixed cutoff (e.g., top 10–20 words) or a relative percentage threshold may be used.

---

## 4. Evaluation Methodology and Metrics

The authors evaluated the keyword extraction method against a standard benchmark dataset to compare its performance with supervised models.

### Dataset
*   **Source:** 500 abstracts from the Inspec database (journal papers in Computer Science and Information Technology).
*   **Ground Truth:** The "uncontrolled" set of keywords manually assigned to each abstract by professional human indexers.

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
