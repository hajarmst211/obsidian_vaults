### 1. Lexicon-Based Sentiment Analysis
This is an unsupervised NLP technique because it does not require a machine learning model to be trained on labeled datasets. Instead, it relies on a pre-defined "lexicon" (a dictionary or vocabulary) where words are pre-assigned sentiment scores (e.g., positive, negative, or neutral). 

### 2. The Two Primary Sub-approaches
As your text and Table 2 indicate, lexicon-based methods are generally categorized into two types:
*   **Dictionary-based approaches:** These rely on existing lexical resources (such as WordNet, SentiWordNet, or General Inquirer) to find synonyms and antonyms of words to determine their sentiment.
*   **Corpus-based approaches:** These derive sentiment dynamically from a specific set of texts (a corpus). This is often done using statistical co-occurrence or semantic association with a set of seed words (e.g., finding how often a word appears near the word "good" versus "bad").

### 3. The Challenge of Domain Reliance (Context-Dependency)
The text correctly highlights a major challenge in NLP: **domain dependence**. Words often change sentiment depending on the context or industry. 
*   In a project management context, *"long"* suggests delay (negative).
*   In a physical description context, *"long"* can imply elegance or a desirable trait (positive).

### 4. Dictionary Adaptation
To address domain reliance, researchers use **dictionary adaptation** (or domain adaptation) techniques. This involves adjusting the baseline sentiment scores of a general lexicon using a domain-specific corpus so that the sentiment values align more closely with the specific field being analyzed (e.g., financial texts, medical reports, or product reviews).


# source1 :
source : https://www.researchgate.net/publication/376389784_Textual_Sentiment_Analysis_using_Lexicon_Based_Approaches

### 1. Overview of the Lexicon-Based Approach
The paper proposes an approach named the **Senti\_Con\_Acron Algorithm** designed to perform sentence-level sentiment classification (categorizing text as positive, negative, or neutral). It addresses three critical limitations of standalone lexicon methods:
*   Context-dependent word meanings.
*   The complexity of acronyms, emoticons, and contextual words.
*   Optimization of lexicon-based performance.

Instead of utilizing a standard machine learning classifier that requires training on labeled datasets, this is a rule-based lexicon method that maps extracted features (unigrams, emoticons, acronyms, and contextual words) to specialized dictionaries to determine sentiment polarity.

---

### 2. Technical Details and System Architecture
The proposed framework is structured into three main levels:

#### Level 1: Data Collection and Preprocessing
*   **Data Source:** Twitter dataset.
*   **Data Storage/Processing:** Stored and extracted using the Hadoop Distributed File System (HDFS). The working dataset is saved in `.csv` format.
*   **Preprocessing Steps:** 
    *   Noise removal (filtering out images, audio, videos, etc.).
    *   Stop words removal.
    *   Stemming and Lemmatization.
    *   Negation handling (identifying and reversing sentiment where negations occur).

#### Level 2: Feature Selection (Extraction)
The method extracts four primary types of features:
1.  **Unigrams:** Assumes occurrence of words is independent (e.g., "I", "have", "a", "lovely", "dog").
2.  **Contextual Words:** Identifies phrases or neighboring words that change the base meaning of a word using a specialized "contextual dictionary."
3.  **Emoticons (Emojis):** Maps emojis to identify expressions (happiness, anger, sadness, surprise, disgust, neutrality) and assigns them positive or negative sentiment values using an emoticon dictionary.
4.  **Acronyms/Abbreviations:** Identifies and expands short-hand text (e.g., expanding "TIA" to "Thank You in advance") before sentiment calculation to ensure the base sentiment terms can be properly parsed by the lexicon.

#### Level 3: Dictionaries Used
The framework relies on three main dictionaries (referenced in Figure 1):
*   **SentiWordNet:** Used for general sentiment classification of unigrams.
*   **SentiSem:** Likely a sentiment semantics dictionary.
*   **ConAcron:** A specialized dictionary mapping contextual words, emoticons, and acronyms to their corresponding sentiment meanings or expansions.

---

### 3. The Senti\_Con\_Acron Algorithm
The paper outlines the algorithmic flow to process the data and assign polarity:

#### Algorithm Pseudocode (Adapted from Pages 5 & 6):
```text
Input: DT (Pre-processed Twitter data)
Output: Classes of sentiments (Positive, Negative, Neutral)

For each item ∈ DT do:
    dt <- item['text']
    Apply Unigram feature model
    Count each sentiment
    
    If 'dt' item contains ['Neighboring Words']:
        Replace with correct contextual words from dictionary
        
    Else if 'dt' item contains ['Acronyms']:
        Replace with correct expanded sentiment words
        
    Else if 'dt' item contains ['Emoticons']:
        Replace with correct emoticon sentiments
End for

Calculate the final polarity value (Positive, Negative, or Neutral)
Evaluate overall metrics (Precision, Recall, F-measure, Accuracy)
Determine frequency rank using Brevity's Mandelbrot law
```

#### Term Frequency and Ranking
To handle keyword frequency and avoid relying solely on repeated occurrences of keywords, the paper applies **Brevity’s Mandelbrot Law** to compute ranking and frequency. It checks ranking values where if $k > k_0$, the ranking is treated as the same, and if $k < k_0$, it is added as $k_0 + k$:

$$\text{Frequency ranking formula (Page 5):} \quad (Tt) \propto f_k(k + k_0)^{-b}$$

*(Where $f$ represents the frequency of a word, and $k$ represents the ranking of a word).*

---

### 4. Training or Fine-Tuning Methods
Because this is a **purely lexicon-based approach**, there is **no traditional machine learning training or model fine-tuning** involved (such as backpropagation or gradient descent). 

To recreate this system, instead of training a model, you would:
1.  **Build/Acquire the Lexicons:** Obtain SentiWordNet 3.0 (or a similar version), construct a lookup table for common Twitter acronyms (like "TIA" $\rightarrow$ "Thank you in advance"), and construct an emoji-to-sentiment mapping dictionary.
2.  **Establish Rule-Based Replacements:** Programmatically scan text tokens. When an acronym or emoji is matched, replace it with its corresponding textual equivalent or directly inject its sentiment score before computing the overall sentence score.

---

### 5. Evaluation Metrics
The performance of the proposed classification method is measured using standard classification metrics derived from a confusion matrix:
*   **Precision:** The ratio of correctly predicted positive observations to the total predicted positives.
*   **Recall (Sensitivity):** The ratio of correctly predicted positive observations to all actual positives.
*   **F-Measure (F1-Score):** The weighted average of Precision and Recall.
*   **Accuracy:** The proportion of total correct predictions (positive, negative, and neutral) over the entire dataset.

---

### 6. How Results Were Displayed
The results were presented using two structured tables on page 6 of the paper:

#### Table 1: Performance of the Proposed Model
This table details the exact metrics achieved by the **Senti\_Con\_Acron** algorithm on the evaluated Twitter dataset:

| Metric | Score (%) |
| :--- | :--- |
| **Precision** | 76.75% |
| **Recall** | 73.52% |
| **F-Measure** | 75.14% |
| **Accuracy** | 80.13% |

#### Table 2: Benchmark Comparison with Existing Work
The paper compares the classification accuracy of their proposed method against four other previously published papers to demonstrate its comparative performance:

| Authors / Reference | Accuracy Result (%) |
| :--- | :--- |
| Ahmad Aloqaily et al. [15] | 68.00% |
| M. Edison et al. [16] | 68.75% |
| VallikannuRamanathan et al. [17] | 76.00% |
| SeydehAkramSaadatNeshan et al. [18] | 76.30% |
| **Proposed Work (Senti_Con_Acron)** | **80.13%** |
