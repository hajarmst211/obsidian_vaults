### 1. Data Retrieval and Loading
* **Conditional Download:** The system checks for the presence of the dataset locally and downloads it only if it is missing.
* **Configurable Loading:** Users can specify the exact number of papers to load from the dataset for processing.

### 2. Category-Based Document Organization
* **Extraction:** For each paper, the system extracts the title, abstract, and assigned category.
* **Storage:** It writes the combined title and abstract into a text file and saves it within a directory named after the paper's specific category (e.g., papers categorized under "Computer Vision" are stored in a dedicated `Computer Vision` folder).

### 3. Category Description Embedding Precomputation
* **Model Initialization:** The system loads a lightweight text embedding model that generates dense vector representations with a dimensionality of 384.
* **Precomputation:** It computes the embeddings for the textual descriptions of each category (for example, mapping the label `astro-ph` to its full description: *"Astrophysics: general astronomy, observational astrophysics..."*).
* **Storage:** These category embeddings are saved locally to prevent redundant calculations during subsequent classification tasks.

### 4. Semantic Similarity Classification
* **Document Vectorization:** To classify a given document, the system generates an embedding vector for the combined text of its title and abstract.
* **Similarity Matching:** It calculates the cosine similarity between the document’s vector and the precomputed category description vectors.
* **Classification:** The document is assigned to the category (or categories) that yield the highest cosine similarity scores.