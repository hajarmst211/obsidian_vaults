source: https://www.researchgate.net/publication/386426531_NLP_FOR_SENTIMENT_ANALYSIS

### 1. Granularity & Types of Sentiment Analysis
The document outlines several levels of granularity and classification categories used in sentiment analysis:
*   **Document-Level Sentiment Analysis:** Evaluates an entire document or review to determine if the overall opinion is positive, negative, or neutral. This level assume the text evaluates a single entity.
*   **Sentence-Level Sentiment Analysis:** Determines the sentiment of individual sentences. This is closely related to *subjectivity classification*, which separates factual sentences from opinionated ones.
*   **Aspect-Based / Feature-Based Sentiment Analysis:** Identifies sentiments directed at specific features or components of an entity rather than the whole entity (e.g., recognizing that "the battery life is poor" refers specifically to the battery, not the entire device).
*   **Fine-Grained Sentiment Analysis:** Extends binary or ternary classifications into more precise categories (e.g., very positive, positive, neutral, negative, very negative) often aligned to 1-to-5 star ratings or a 0-to-100 scale.
*   **Emotion Detection:** Aims to recognize specific emotional states (such as happiness, anger, frustration, grief, and surprise) instead of just positive or negative polarity.
*   **Intent-Based Analysis:** Determines the motivation behind a text (e.g., identifying whether a customer review expressing frustration contains an implicit request to be contacted by customer support).

---

### 2. Fundamental Text Preprocessing & NLP Techniques
Before sentiment classification occurs, several foundational text processing techniques are used to clean and structure the data:
*   **Tokenization:** Dividing text into separate elements (tokens).
    *   *Word Tokenization:* Segmenting text into individual words.
    *   *Sentence Tokenization:* Segmenting text into individual sentences.
*   **Stemming:** Reducing inflected words to their base form by stripping affixes using deterministic rules (e.g., using the *Porter Stemmer*). This method is computationally fast but can sometimes produce non-meaningful root words (e.g., "communication" becomes "commun").
*   **Lemmatization:** Converting words to their dictionary root form (*lemma*) using morphological analysis and Part-of-Speech (POS) context (e.g., using *WordNetLemmatizer*). This always results in a linguistically valid word.
*   **Stop-Word Removal:** Filtering out high-frequency words that carry little sentiment information (such as "is", "the", articles, punctuation, and URLs).
*   **Part-of-Speech (POS) Tagging:** Assigning morphosyntactic classes (e.g., noun, verb, adjective) to each word using standardized tagsets like the *Penn Treebank* (e.g., Brill's tagger).
*   **Parsing (Syntactic Analysis):** Determining the structural organization of sentences.
    *   *Constituent-Based Models:* Focus on nested grammatical categories (noun phrases, verb phrases).
    *   *Dependency-Based Models:* Analyze binary structural connections (head-dependent relationships) between words.
    *   *Parsing Formalisms:* The text mentions several linguistic frameworks used in parsing, including Lexical Functional Grammar (LFG), Generalized Phrase Structure Grammar (GPSG), Head-Driven Phrase Structure Grammar (HPSG), and Tree Adjoining Grammar (TAG).

---

### 3. Feature Extraction & Text Representation
To feed text into machine learning models, it must be converted into numerical representations:
*   **Dictionary / Lexicon of Manually Defined Keywords:** Operating on a predefined list of words associated with positive or negative scores.
*   **Bag-of-Words (BOW):** Representing a document as a multiset (bag) of its words, counting the frequency of occurrence for each word while ignoring grammar and word order.
*   **N-grams Model:** Grouping adjacent words to preserve localized sequence context (e.g., unigrams, bigrams, trigrams).
*   **TF-IDF (Term Frequency-Inverse Document Frequency):** A weighting technique that balances how often a word appears in a specific document against how common it is across the entire corpus. This down-weights generic terms (like "and" or "the") and highlights rare, informative words.
*   **Word Embeddings:** Mapping words to dense vector spaces where semantically similar words are positioned closer together:
    *   *Word2Vec:* A shallow two-layer neural network utilizing two training architectures:
        *   *Continuous Bag of Words (CBOW):* Predicts a target word based on its surrounding context words.
        *   *Skip-gram:* Predicts surrounding context words given a single target word.
    *   *GloVe (Global Vectors):* An unsupervised learning model that constructs representations based on global word-word co-occurrence statistics.
    *   *FastText:* An extension of Word2Vec that represents words as bags of character n-grams, allowing it to generate embeddings for out-of-vocabulary words.
    *   *ELMo (Embeddings from Language Models):* Generates contextualized, deep bi-directional word embeddings using a multi-layer BiLSTM, resolving issues with polysemy (words with multiple meanings, like "stick").
*   **Sentence Embeddings:** Condensing entire sentences into a single vector using pooling strategies (mean pooling, max pooling) or specialized tokens (like BERT's `[CLS]` token).

---

### 4. Machine Learning & Statistical Models
The text discusses using classic machine learning algorithms trained on vectorized text features:
*   **Support Vector Machines (SVM) & Least Squares Support Vector Regression (LS-SVR):** Used for linear or non-linear classification and regression tasks (such as predicting stock market trends or classifying text polarity).
*   **Naive Bayes (NB):** Often combined with Senti-Lexicon methods for probabilistic sentiment classification.
*   **Logistic Regression:** Employed as a baseline classifier.
*   **Decision Trees & Random Forests:** Utilized for building structured prediction models.
*   **VADER (Valence Aware Dictionary and Sentiment Reasoner):** A rule-based, lexicon-dependent tool tailored specifically for social media contexts that accounts for word order and degree modifiers.
*   **TextBlob:** A Python library utilizing a simplified API to perform basic lexicon-based sentiment scoring.
*   **Flair:** A PyTorch-based framework that provides state-of-the-art NLP features, including contextual string embeddings optimized for sentiment analysis.

---

### 5. Deep Learning Architectures
Deep neural networks are highlighted for their ability to process sequential data and capture complex sentiment patterns:
*   **Recurrent Neural Networks (RNNs):** Utilize feedback loops to maintain a hidden state, allowing them to process sequential information.
*   **Long Short-Term Memory (LSTM):** Resolves the vanishing gradient problem of standard RNNs by introducing memory cells and three gating mechanisms (input, forget, and output gates) to retain long-range context.
*   **Gated Recurrent Units (GRU):** A streamlined version of LSTM that combines the input and forget gates into a single update gate, lowering computational complexity.
*   **Bidirectional LSTM (BiLSTM) with Attention:**
    *   *BiLSTM:* Processes text sequences in both forward and backward directions to capture full context.
    *   *Attention Mechanisms:* Dynamically weight different parts of a sentence, allowing the model to focus on the most sentiment-relevant words or phrases (such as modifiers or key adjectives) regardless of distance.
*   **Convolutional Neural Networks (CNNs):** Originally designed for computer vision, CNNs are applied to text sequences to capture local translation-invariant features (hierarchical n-grams) using convolutional filters.

---

### 6. Transformer-Based & Large Language Models (LLMs)
The document emphasizes that modern state-of-the-art sentiment analysis relies on pre-trained transformer architectures:
*   **BERT (Bidirectional Encoder Representations from Transformers):** A bidirectional transformer encoder model trained on masked language modeling, highly capable of understanding contextual nuances (e.g., recognizing that "not bad" is positive).
*   **RoBERTa (Robustly Optimized BERT Approach):** An optimized version of BERT trained with larger batch sizes, more data, and without the next-sentence prediction objective, outperforming BERT on various benchmarks.
*   **GPT (Generative Pre-trained Transformer) / GPT-3:** Unidirectional, decoder-based generative models. While designed for text generation, they can be fine-tuned or prompted to categorize feelings and generate nuanced, empathetic conversational responses.
*   **Sentence-BERT (SBERT):** A modification of pre-trained BERT networks using siamese and triplet network structures to derive semantically meaningful sentence embeddings that can be quickly compared using cosine similarity.
*   **Baidu ERNIE Bot & SentiBERT:** Incorporate specialized sentiment-understanding architectures (such as SentiBERT) to extract emotional features from short texts.
*   **PaLM / PaLM-E:** Multi-modal language models capable of integrating textual data with visual inputs to analyze sentiment holistically.



# comparaison:
- **Naive Bayes:** Historically one of the most common baseline algorithms for sentiment analysis. It relies on Bayes' Theorem and assumes feature independence. Despite its simplicity, it is highly efficient and often performs surprisingly well on text classification.
    
- **Support Vector Machines (SVM):** Often preferred for traditional text classification because they handle high-dimensional sparse data (which is typical of TF-IDF matrices) very effectively.
    
- **Decision Trees:** Less commonly used as standalone models for text due to their tendency to overfit high-dimensional data, but they (and their ensemble variants like Random Forests or Gradient Boosting) are still utilized in certain NLP pipelines.