### (Extractive) Genetic Algorithm (GA)

**purpose:** identify and extract the relationship between the main features of the input text and the creation of repetitive patterns in order to produce and optimize the vector of the main document features in the production of the summary document compared to other previous method.

By representing sentences as binary arrays, the algorithm interactively generates and refines subsets of sentences—mimicking biological evolution—to find the optimal combination that maximizes coverage, relevance, and coherence while minimizing redundancy

Combine a genetic algorithm with **machine learning models** (like BERT) for better semantic understanding

- The models combination of measurements are: 
	- **Standard GA** (uses TF-IDF similarity and redundancy).
	- **MCBA + GA** (adds sentence position rules).
	- **RPM + GA** (uses shared frequent word patterns). 

- BERT + GA(GaSUM technique): https://www.scitepress.org/Papers/2023/118930/118930.pdf
- Text feature weighting: https://www.researchgate.net/publication/236593719_Text_Feature_Weighting_for_Summarization_of_Documents_in_Bahasa_Indonesia_Using_Genetic_Algorithm

---

### Modified corpus-based approach (MCBA) and LSA-based T.R.M
(tested on 100 political articles)
the [score function](https://www.sciencedirect.com/topics/computer-science/score-function) is trained by the [genetic algorithm](https://www.sciencedirect.com/topics/engineering/genetic-algorithm) (GA)
- **MCBA + GA:** 52% F-measure
- **MCBA (standard):** 49% F-measure
- **LSA + T.R.M. (single-document level):** 44% F-measure
- **LSA + T.R.M. (corpus level):** 40% F-measure
--> https://www.sciencedirect.com/science/article/abs/pii/S0306457304000329?via%3Dihub=

###### MCBA + GA:
(a) in order to denote the importance of various sentence positions, these sentence positions are ranked, 
(b) Genetic Algorithm (GA)
- Because for use, humans, the important ideas of a text are located at the top or the bottom of the article, MCBA incorporates this structural knowledge into the GA's scoring system using **positional weights**
- It prevents the GA from selecting obscure sentences from the middle of the text that might have high TF-IDF scores due to technical jargon but lack general summary value.

--> **MCBA** (Modified Corpus-Based Approach) and **RPM** (Repetitive Pattern Mining) are not run after the Genetic Algorithm is complete. Rather, they are **alternative scoring systems (fitness functions)** that guide the GA while it is running

###### variants of GA:
- **Standard GA and MCBA + GA (TF-IDF Vector Space):**  
    These variants use a classical bag-of-words representation weighted by **TF-IDF** (Term Frequency-Inverse Document Frequency). They evaluate text based on exact word matches and term frequencies. They do not understand synonyms or contextual meaning.mmmmmm    
- **RPM + GA (Pattern-Based Representation):**  
    Instead of full vocabulary vectors, this model filters the document down to **frequent patterns** (words that appear in two or more sentences). It represents the document not by individual semantic values, but by a network of recurring talking points.
- **GaSUM (BERT Contextual Embeddings):**  
    GaSUM replaces traditional bag-of-words representation with **BERT deep learning embeddings** (using the [CLS] token vector). This allows GaSUM to evaluate the deep, contextual semantic meaning of sentences. It understands synonyms, metaphors, and context, even when different words are used to express the same idea.
---

### def: Latent Semantic Analysis (LSA)
 LSA represents the meaning of a word as a kind of average of the meaning of all the passages in which it appears, and the meaning of a passage as a kind of average of the meaning of all the words it contains
 
LSA is a fully automatic mathematical/statistical technique for extracting and inferring relations of expected contextual usage of words in passages of discourse. It is not a traditional natural language processing or artificial intelligence program; it uses no humanly constructed dictionaries, knowledge bases, semantic networks, grammars, syntactic parsers, or morphologies, or the like, and takes as its input only raw text parsed into words defined as unique character strings and separated into meaningful passages or samples such as sentences or paragraphs

Based on the provided text, Latent Semantic Analysis (LSA) has been evaluated across a range of tasks in cognitive psychology, educational assessment, and information science. LSA's performance is typically assessed by comparing its mathematical similarity metrics (such as cosines between vectors) to human performance, judgments, and standardized test results. 

The primary results and evaluations detailed in the text are organized below:

###### 1. Information Retrieval
* **Evaluation:** LSA (often termed Latent Semantic Indexing, or LSI, in this context) was evaluated on its ability to match search queries to relevant documents, even when they did not share literal keywords.
* **Results:** LSA performed between 16% and 30% better than standard vector-based retrieval methods that do not use dimensionality reduction. It has also been successfully used to automate the assignment of submitted manuscripts to appropriate reviewers.

###### 2. Synonym and Vocabulary Tests
* **Evaluation:** LSA was trained on a large corpus (such as an encyclopedia) and tested on an 80-item synonym test from the TOEFL (Test of English as a Foreign Language).
* **Results:** LSA selected the correct synonym in 65% of the cases, matching the average score of human college applicants from non-English speaking countries. 
* **Role of Dimensionality:** The evaluation showed that dimensionality reduction is critical. At an optimal level of 300 to 325 dimensions, LSA scored 52.7% correct (corrected for guessing), whereas using no dimensionality reduction (first-order co-occurrence) yielded only 15.8% correct.
*  When LSA chose wrongly and most students chose correctly, it sometimes appeared to be because LSA is more sensitive to contextual or paradigmatic associations and less to contrastive semantic or syntagmatic features. for example, LSA prefered nurse (cos = 0.47) of doctor (cos = 0.41) as association for physician. 

###### 3. Word Sorting and Categorization
* **Evaluation:** LSA was used to simulate developmental studies where children and adults sorted words into meaningful categories.
* **Results:** LSA’s similarity metrics correlated significantly with human sorting patterns ($r = 0.50$ with child data and $r = 0.35$ with adult data using a 3rd-grade reading corpus; these rose to $r = 0.61$ and $r = 0.50$ respectively when using a college-level corpus). 
* **Limitations:** LSA did not distinguish parts of speech as strongly as human participants did, which the authors attribute to LSA's disregard for word order and syntax.

###### 4. Subject-Matter Knowledge and Exams
* **Evaluation:** LSA was trained on psychology textbooks and tested on publisher-provided multiple-choice exams, as well as essay-based history concepts.
* **Results:** LSA scored well above chance on multiple-choice exams. While it scored below the class average of university students, its performance was high enough to receive a passing grade. In a study on historical texts, LSA's predictions of concept relatedness correlated significantly with human judgments, showing a stronger correlation with domain experts ($r = 0.41$) than with novices ($r = 0.36$).

###### 5. Semantic Priming
* **Evaluation:** LSA simulated psycholinguistic experiments involving word priming, context-based disambiguation of homographs (e.g., distinguishing "mint" as money versus candy), and sentence comprehension.
* **Results:** The pattern of LSA vector similarities between words and sentences closely resembled the priming patterns observed in human behavioral experiments.

###### 6. Essay Grading
* **Evaluation:** LSA graded student essay answers using various methods, such as comparing student essays to pre-graded essays or expert texts.
* **Results:** Across multiple subject areas (including anatomy, history, and marketing), the grades assigned by LSA correlated with human expert grades at a level comparable to the correlation between different human graders evaluating the same essays.

###### 7. Text Coherence and Learning Predictions
* **Evaluation:** LSA assessed textual coherence by calculating the similarity (cosines) between consecutive sentences or paragraphs.
* **Results:** LSA-derived coherence measures predicted student text comprehension. In one study regarding heart anatomy, LSA’s coherence measures predicted comprehension test scores with a correlation of $r = 0.93$. Additionally, LSA has been used to predict learning gains by matching students' pre-existing knowledge levels with instructional texts of optimal conceptual difficulty.

source: https://www.researchgate.net/publication/200045222_An_Introduction_to_Latent_Semantic_Analysis

###### Limitations and Caveats
While the results indicate that LSA approximates several aspects of human semantic processing, the authors note several limitations:
* **Lack of Syntax and Logic:** Because LSA completely ignores word order, syntax, and morphology, it cannot represent grammatical relations and can sometimes make errors or scramble meaning on specific individual cases.
* **Data Dependency:** LSA is highly dependent on the quality and size of its training corpus. It can underpredict global or situational relationships if they are not explicitly discussed in the training text.
* **Averaging Effect:** LSA represents words and passages as averages, meaning it performs best when simulating average human results over many cases rather than predicting highly specific, isolated word-pair interactions.

---

### Repetitive pattern mining with a genetic algorithm (GA)

To avoid high-dimensional feature vectors, the method identifies "repetitive patterns" across documents using a two-step process: keyword weighting via TF-IDF --> Frequent Pattern Mining (Apriori Algorithm) --> Term-Document Matrix: A binary matrix is constructed where rows represent sentences and columns represent the identified repetitive patterns

then the GA is used to find the most representative combination of sentences for the final summary

- Evaluation of the methode: Below is the average score of satisfaction feedback from readers
![[Pasted image 20260711100649.png]]

In this method, we solve the problems of inconsistency and ambiguity to the desirable level in the summary document
source: https://www.techscience.com/cmc/v67n1/41173/html

---

### Bi-Gram Pseudo Sentence (BGPS)

- It solves the feature sparseness problem, caused by obtaining features from a single sentence as BGPS contain a greater number of features (words) than a single sentence
- The hybrid statistical sentence extraction methods used here are: title method, location method, aggregation similarity method, frequency method and tf-based query method
- the probability of a word depends on the previous N‑1 words
- Uses word probability based on previous context
- Commonly applied in text generation, autocomplete and language modelling

algorithm : https://www.geeksforgeeks.org/nlp/sentence-generation-with-bi-tri-and-n-gram/
https://medium.com/@rupeshsushir18/sensitization-to-bigram-calculation-in-nlp-with-solved-examples-99f87b968000

###### How it works:
- **Sentence Scoring:** Every sentence in the document is scored by summing the probabilities of its constituent bigrams, divided by the number of bigrams in that sentence. This yields an average bigram probability score.
    
- **Sentence Selection:** The top N sentences with the highest scores are extracted and returned in their original order of appearance to maintain chronological flow.
###### Model Accuracy and Loss by N-gram

The authors trained five separate models (from uni-gram to 5-gram) for up to 300 epochs and reported the following performance:

- **Uni-gram model:** 35% accuracy
- **Bi-gram model:** 75% accuracy
- **Tri-gram model:** 95% accuracy
- **4-gram model:** 99% average accuracy, with an average loss of 2.04%
- **5-gram model:** 99.74% average accuracy, with an average loss of 1.11%
--> when trained on higher-order N-grams, yields a higher reported accuracy
![[Pasted image 20260711135728.png]]d

from: https://www.researchgate.net/publication/380151602_Enhancing_Bangla_Language_Next_Word_Prediction_and_Sentence_Completion_through_Extended_RNN_with_Bi-LSTM_Model_On_N-gram_Language?_tp=eyJjb250ZXh0Ijp7ImZpcnN0UGFnZSI6InB1YmxpY2F0aW9uIiwicGFnZSI6InNlYXJjaCIsInBvc2l0aW9uIjoicGFnZUhlYWRlciJ9fQ

##### evaluation:
- **ROUGE-1 (F1):** Represents how many of the individual words in the reference abstract were successfully captured in our extracted sentences.
    
- **ROUGE-2 (F1):** Represents the overlap of consecutive word pairs. Because our model is optimized based on bigram frequencies, this metric serves as an indicator of how well we preserve key technical phrases.
    
- **ROUGE-L (F1):** Measures the longest matching sequence of words. Higher scores indicate that structural flow and word ordering are closer to the reference layout.
    
- **Cosine Similarity:** Provides a score between 0 and 1. A higher score suggests that the overall vocabulary distributions of the summary and the target abstract are aligned.

---

###  Summarization of text through complex network approach(extractive)

Source text is represented through a network such that each source sentence is represented by a node and an edge is formed by connecting two nodes if their respective sentences have at least a single common word, i.e., lexical repetition.

There are seven network measurements :Degree, Shortest path,Locality index, d-rings, k-cores, w-cuts, communities

- Selection of the sentences to be in the summary:
The system calculates the mentioned measurements for each sentence, based on which it gets a rating. Then the system selects the top n highest-ranked sentences to form the summary, where n is determined by the desired compression rate (e.g., if a 20% compression rate is requested for a 50-sentence text, the top 10 ranked sentences are chosen)

Once the top n sentences are selected, their order in the final summary is typically decided in one of two ways:

- **Original Chronological Order (Standard Practice):** In most extractive summarization models of this type, the chosen sentences are reassembled in the order they originally appeared in the source text. This helps maintain the chronological flow and readability of the content.
- **Rank Order:** Alternatively, they can be presented in descending order of their importance (rank), though this is less common as it often disrupts the readability of the text.

##### Text summarization as a future application of this mesoscopic model

| Approach / Features Used                        | Adjusted Rand Index (ARI) | Accuracy          |
| ----------------------------------------------- | ------------------------- | ----------------- |
| **Proposed Mesoscopic Network (All features)**  | **0.749**                 | **91.1% (0.911)** |
| Mesoscopic Network (Clustering metric only)     | 0.679                     | 88.3% (0.883)     |
| Mesoscopic Network (Matching Index metric only) | 0.576                     | 83.3% (0.833)     |
| **Traditional Co-occurrence Network**           | **0.268**                 | **57.5% (0.575)** |

###### Key Statistical Observations from the Results:

- **Error Rates:** Using the proposed mesoscopic approach, only **8.9% of the texts were incorrectly clustered** overall, and there was only a **0.02% false negative rate** for the Shuffled Paragraphs (SP) class.
- **Failure of Traditional Networks on Paragraph Shuffling:** When using traditional word co-occurrence networks, the system could not reliably distinguish real texts from texts with shuffled paragraphs. In that setup, **72.5% of the Shuffled Paragraph (SP) texts were incorrectly classified as Real Texts (RT)**.
- **Principal Component Analysis (PCA):** In the visual projections of the network features, the first two principal components accounted for approximately **76%** of the variance in the mesoscopic networks, compared to **70%** in the co-occurrence networks.

article: https://arxiv.org/abs/1606.09636

---

### (abstractive) Semantic Graph Reduction

 - This approach works in three phases: firstly generation of a rich semantic graph from the original documents, secondly reduction of the rich semantic graph thus generated to a highly abstracted graph and finally generation of an abstract:

##### 1. The Rich Semantic Graph (RSG) Creation Phase
--> Respresenting the input document  as a Rich Semantic Graph where the nouns and verbs are represented as nodes and semantic relations as edges.
- First phase here is the preprocessing that includes: named entity recognition, morphological and syntactic analysis, cross-reference resolution, and pronominal resolution to resolve syntactic ambiguities. Secondly, generating sub-graphs; we begin by linking the words depending on the context using an ontology (a structured database or dictionary of a specific subject area) then it checks if those combinations make sense together and finally it validates the interconnections and gives scores to  the resulting sub-graphs. The scoring is based on formulas that look at how common or popular a specific word meaning is (using data from WordNet). The diagram with the highest score is chosen as the most likely representation of the sentence's actual meaning

##### 2. Second phase: 
the generated semantic graph is reduced to a more abstracted form. The model applies a set of heuristic rules to merge, delete, or consolidate graph nodes (subjects, verbs, and objects). These heuristic rules leverage WordNet semantic relations such as hypernyms, holonyms, and entailments to identify similarities and group redundant or related concepts.

##### 3. phase three:
Generating the final abstractive summary from the reduced Rich Semantic Graph by accessing the domain ontology and WordNet. 
![[Pasted image 20260711151844.png|546]]

evaluation: 
- how logically and smoothly the sentences in the generated paragraph connect with one another.
- Synonym Frequency (WordNet Rank): highly ranked synonyms receive a higher score.

source: https://www.academia.edu/12791557/Semantic_graph_reduction_approach_for_abstractive_Text_Summarization
optimization: https://www.sciencedirect.com/science/article/pii/S1110016826001766