### 1. Paradigm Shift: From Sequential to Attention-Only Models (architecture 1)

#### Legacy Architectures
*   **Recurrent Layers (RNNs/LSTMs):** These process inputs in a single-direction, step-by-step sequential order over time, creating a processing bottleneck.
*   **Convolutional Layers (CNNs):** These rely on local operations, which can restrict their receptive fields.
*   **Sequential Processing:** Transformers eliminate sequential processing by treating the entire input sequence as a single matrix (X) rather than a series of individual steps. And <mark style="background: #FFB86CA6;">removing the requirement of waiting for the previous state</mark>. 

#### The Transformer Block Approach
*   **Self-Attention Mechanisms:** These establish direct connection lines between all parts of a sequence simultaneously.
*   **Residual Connections & Layer Norm:** These assist in stabilizing signal paths throughout deep networks.
*   **Fully Parallel Processing:** This mitigates the sequential bottleneck by allowing entire sequence grids to process at once.
*   **The Core Concept:** An input sequence matrix passes directly into a self-attention layer, then into a feed-forward network, which allows tokens to associate with other tokens with a path length of $O(1)$.

#### Key Architectural Results
*   **Long-Range Dependency Mitigation:** Information path length is minimized, helping to preserve context over longer sequences.
*   **Parallelization:** These designs train significantly faster on GPUs compared to traditional sequential architectures.
*   **Foundation of Modern Models:** This architecture serves as the underlying engine behind models such as <mark style="background: #FFB86CA6;">GPT-4, Llama, and Claude</mark>.

---

### 2. High-Level Transformer Architectures (architecture 2)

#### The Original Encoder-Decoder Split
*   **Encoder Half:** Historically used for <mark style="background: #FFF3A3A6;">bidirectional tasks and translation (such as BERT</mark>). It maps inputs to a continuous representation.
*   **Decoder Half:** Designed to focus on autoregressive generation.

#### The Decoder-Only Architecture
*   Modern <mark style="background: #FFB8EBA6;">Simplified</mark> Standard:
*   **Cross-Attention Removal:** The architecture is simplified by <mark style="background: #FFB8EBA6;">removing the Cross-Attention layer Because there's no encoder</mark>(which originally looked at the encoder's output), reducing the block to: The main parts of a decoder: <mark style="background: #FFB86CA6;">Masked Self-Attention + Feed-Forward Network (FFN).</mark>
###### Architecture
1. **Preparation:**
```
Input Vector=Token Embedding+Positional Embedding
```
This combined vector contains both what the word means and where it is located in the sequence.

2. **The Core Decoder Block (Repeated N times)**
- <mark style="background: #FFB86CA6;">Masked Self-Attention (Context Gathering):  </mark>
    Tokens look at other tokens to build contextual meaning. To prevent the model from seeing future words during next-token prediction, a mathematical **causal mask** is applied, blocking access to all future tokens and <mark style="background: #FFF3A3A6;">forcing the model to look only backward.</mark>
    
- <mark style="background: #FFB86CA6;">Feed-Forward Network (Refinement):  </mark>
    The contextualized vectors are passed through a standard 2-layer MLP applied to each token individually. This processes the gathered context and updates the token's vector using the model's learned knowledge.
    
- <mark style="background: #FFB86CA6;">Add & Norm (Stabilization):  </mark>
    Throughout these two steps, **residual connections** (bypasses that prevent vanishing gradients) and **Layer Normalization** (which scales activations to prevent numerical explosion) stabilize the signal as it travels through the deep layers of the network.

###### Structural flow:
Inputs $\rightarrow$ Input Token Embeddings + Positional Embeddings $\rightarrow$ Stacks of Decoder Blocks (maintaining a constant shape of $N \times d_{\text{model}}$ but contextualizing representations) $\rightarrow$ Final Layer Norm $\rightarrow$ Linear Layer (LM Head projecting to vocabulary size) $\rightarrow$ Softmax $\rightarrow$ Probability distribution output over the next token.

---
## Applies to both architectures

There are several types of attention mechanisms. They generally differ in three ways: **where the vectors come from**, **what information is allowed to be seen (masking)**, and **how the attention heads are organize**

---
### 3. The Core Self-Attention Mechanism 

#### Operational Steps
1.  **The Input:** Starts with static <mark style="background: #FFB86CA6;">word embeddings + positional information</mark>, where words are initially isolated.
2.  **The Mechanism:** Every token simultaneously calculates a dynamic relevance score against every other token in the sequence (e.g., in "The animal didn't cross it", the token "it" computes a high relevance weight to "animal", and lower weights to "didn't" and "cross").
3.  **The Output:** Generates contextualized vectors where the representation for a pronoun like "it" is infused with the semantic information of the noun "animal".

#### Properties of Self-Attention
*   **Direct Paths:** Facilitates the handling of long-range dependencies with an $O(1)$ path length.
*   **Dynamic Representation:** <mark style="background: #FFB86CA6;">Vector representations are context-dependent</mark> (e.g., the word vector for "bank" shifts based on whether the surrounding context refers to a river or a financial institution).
*   **Parallel Computation:** All relevance scores are computed simultaneously using matrix multiplication.

---

### 4. Mathematical Computation of Attention (Q, K, V)

#### Vector Generation
An input embedding vector ($x_i$) is multiplied by three learned weight matrices to produce three unique vectors:
*   **Query vector ($q_i$):** Represents "What am I looking for?" $\rightarrow$ Calculated as $q_i = x_i \cdot W_Q$
*   **Key vector ($k_i$):** Represents "What do I contain?" $\rightarrow$ Calculated as $k_i = x_i \cdot W_K$
*   **Value vector ($v_i$):** Represents "What is my actual meaning?" $\rightarrow$ Calculated as $v_i = x_i \cdot W_V$
*   *Note:* This vector split occurs for every word in a sequence simultaneously.
* In PyTorch code, these matrices correspond to standard linear layers (<mark style="background: #FFB86CA6;">nn.Linear</mark>).

#### The Search Engine Analogy
*   **Query (Q):** Represents the search request typed into a browser.
*   **Key (K):** Represents database tags or titles used for matching.
*   **Value (V):** Represents the actual content or payload retrieved once a matching Key is found.
*   **Takeaway:** The model does not compare words directly to words; it compares a word's search request ($Q$) to other words' labels ($K$) to retrieve their semantic meanings ($V$).

* The matrix operation $Q$x$K_T$  allows to calculate simultaneously the relevance of each word with respect to all the others. The result is one **matrix of raw scores** where every box (i,J) indicates the importance of the word J for the word i

---

### 5. Calculating and Scaling Attention Weights

#### Score Calculation
*   **Micro View (Vector Level):** <mark style="background: #FFB86CA6;">The relevance of token $i$ to token $j$ is calculated as the dot product:</mark> $\text{score}(i, j) = Q_i \cdot K_j^T$. A smaller angle ($\theta \approx 0$) between vectors results in a higher dot product.
*   **Macro View (Matrix Level):** The entire sequence is processed as a matrix operation: $Q \times K^T = \text{Raw Attention Scores}$.

#### Dimensional Scaling
*   **The Scaling Problem:** In high dimensions ($d_k$), raw dot products can produce large values. <mark style="background: #FFB86CA6;">Passing these unscaled values into the Softmax function can create sharp peaks with flat bases,</mark> leading to <mark style="background: #FFF3A3A6;">vanishing gradients</mark> during backpropagation.
*   **The Mitigation:** Divide the dot product by the square root of the key dimensionality ($\sqrt{d_k}$), yielding scaled scores: $\frac{QK^T}{\sqrt{d_k}}$. <mark style="background: #FFB86CA6;">This controls variance and helps maintain stable gradient flow</mark>.

#### Softmax Transformation
*   **Formula:** $\text{Softmax}(x_i) = \frac{\exp(x_i)}{\sum_{j=1}^N \exp(x_j)}$
*   **Exponentiation:** The exponential function ensures all attention weights are positive.
*   **Normalization:** Dividing by the sum of exponentials row-by-row normalizes the row values to sum to 1.0, creating a valid probability distribution.
*   **Takeaway:** <mark style="background: #FFB86CA6;">Each row of the resulting matrix acts as a unique weighting scheme dictating how much attention one word pays to every other word.</mark>

#### Output Generation
*   **Weighted Sum:** The final output is computed by multiplying the normalized Attention Weight matrix by the Value Matrix $V$:
    $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
*   **Contextualization:** Every row in the output is a weighted mixture of Values. The shape of the data matrix remains unchanged, but the vectors are now contextualized.

#### Dimensional Flow Tracker
Assuming $N=5$ words and $d=4$ dimensions, the matrix shapes flow as follows:
$$Q [5 \times 4] \times K^T [4 \times 5] \rightarrow \text{Scores} [5 \times 5] \xrightarrow{\text{Softmax}} \text{Weights} [5 \times 5] \times V [5 \times 4] \rightarrow \text{Output} [5 \times 4]$$

---

### 6. Multi-Head Attention (MHA)

#### Limitations of Single-Head Attention
*   **Muddled Representations:** If a model uses only a single attention head, <mark style="background: #FFB8EBA6;">a query must try to capture multiple different types of relationships at once, which can compromise the vector representation.</mark>

#### The Multi-Head Solution
*   **Parallel Tracking:** Rather than using a single "head" to capture context, the Transformer block options **Multi-Head Attention**. This allows the model to parallel-track different types of linguistic relationships simultaneously. The outputs of these parallel heads are eventually concatenated and projected back to the model's standard dimension
    *   *Head 1:* Can focus on Subject-Verb relationships.
    *   *Head 2:* Can focus on Noun-Modifier relationships.
    *   *Head 3:* Can focus on Article relationships.

#### Parameter Allocation
*   **Dimensionality Scaling:** To keep the computational cost comparable to single-head attention, the feature dimensionality of each head is scaled down using the number of heads ($h$):
    $$d_k = \frac{d_{\text{model}}}{h}$$

#### Multi-Head Attention Workflow
1.  **Project:** The input matrix $X$ (e.g., shape $N \times 512$) is projected into $h$ separate subspaces using learned linear layers ($W^{iQ}, W^{iK}, W^{iV}$). This transforms the shape from $N \times 512$ to $N \times h \times 64$.
2.  **Parallel Attention:** Scaled Dot-Product Attention is applied independently within each of the $h$ subspaces.
3.  **Final Project:** The concatenated tensor is passed through a final linear layer ($W^O$) to mix the insights of all heads, yielding the output $Z$ ($N \times 512$).

---

### 7. The Problem of Order & Positional Encodings

#### Permutation Invariance
*   **Lack of Sequence Order:** The core dot product ($QK^T$) measures content similarity but does not have an inherent concept of token order. Shuffling the rows of the input matrix $X$ simply shuffles the output matrix $Z$ in the same manner.
*   **Semantic Ambiguity:** Without positional context, a model would process "The dog bit the man" and the scrambled sequence "Bit the man dog the" identically.

#### Position Injection Method
*   **Addition:** Position information is added directly to the static token embeddings at the beginning of the network:
    $$X_{\text{input}} = \text{Embedding}(x) + \text{PE}_{\text{position}}$$
*   **Dimensional Preservation:** Adding the matrices keeps the dimensions identical ($d_{\text{model}}$), allowing the network to learn to separate semantic meaning from sequence position.
*   **Efficiency:** Requires zero sequential operations; the positional encoding (PE) matrix can be pre-calculated and added instantly via GPU operations.

#### Sinusoidal Encodings
*   **Mathematical Formula:** Calculated using sine and cosine waves at varying frequencies:
    $$\text{PE}_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$
    $$\text{PE}_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$
*   **Frequency Breakdown:** High-frequency dimensions (top of the vector) track micro-level changes (e.g., word 1 vs word 3), while low-frequency dimensions (bottom of the vector) track macro-level changes (e.g., beginning vs end of a document).
*   **Key Properties:**
    *   *Bounded:* Values remain between -1 and 1, preventing embedding explosion.
    *   *Extrapolation:* Can theoretically extrapolate to sequence lengths longer than those seen in training.
    *   *Relative Relationships:* Simple linear transformations allow the model to capture relative distances between tokens.

#### Learned Positional Embeddings
*   **Alternative Design:** Modern models often assign a learned embedding lookup table (`nn.Embedding`) for positions 1 through $N$, which is optimized via backpropagation.
*   **Trade-Offs:**
    *   *Pros:* Simple to implement and often yields strong empirical performance on standard benchmarks.
    *   *Cons:* Sets a hard limit on context length; if trained on a 2,000-token context, it cannot natively process token 2,001.

---

### 8. Key Components of the Transformer Block

Each block is structured using the repeated pattern: $\text{Output} = \text{Norm}(\text{Input} + \text{Sublayer}(\text{Input}))$.

#### Masked Self-Attention Layer
*   **Scope:** Sequence-level (tokens interact with other tokens).
*   **Role:** Allows a token to evaluate past tokens to determine its role in the current sentence context.

#### Feed-Forward Network (FFN)
*   **Scope:** Token-level (applied to each token vector independently).
* It is applied to each token vector in a manner **independent**.
- This means that <mark style="background: #FFB8EBA6;">the network processes each word of the sequence separately using the same parameters learned for each</mark>

*   **Role:** Refines representations via a standard 2-layer MLP (Multi-Layer Perceptron) that updates the token's vector based on overall learned parameters.


#### Add & Norm Layer
*   **Scope:** Structural (applied to the outputs of both the Attention and FFN layers).
*   **Residual Connection ("Add"):** Passes the input $x$ around the sublayer ($x + \text{Sublayer}(x)$). 
* Elle sert a <mark style="background: #FFB86CA6;">stabiliser les gradients et faciliter le passage du signal dans les réseaux profonds</mark>
*   **Layer Normalization ("Norm"):** <mark style="background: #FFF3A3A6;">Stabilizes activations by forcing the vector dimensions to maintain a consistent mean and variance</mark>, preventing numerical instability.
*   **Takeaway:** This residual-normalization loop is crucial for optimizing models with deep stacks of transformer blocks.

---

### 9. Causal Masking

#### Causal Masking Objective
*   **Autoregressive Enforcement:** <mark style="background: #FFF3A3A6;">To force the decoder to predict only the next word without using information from future tokens</mark>, future positions must be hidden during training.

#### Masking Process
1.  **Raw Attention Scores:** Calculated normally via $\frac{QK^T}{\sqrt{d_k}}$.
2.  **Apply Causal Mask:** An upper-triangular matrix of $-\infty$ values (with $0$ on the diagonal and lower triangle) is added to the raw score matrix:
    $$\text{Causal Mask} = \begin{pmatrix} 
    0 & -\infty & -\infty & -\infty \\ 
    0 & 0 & -\infty & -\infty \\ 
    0 & 0 & 0 & -\infty \\ 
    0 & 0 & 0 & 0 
    \end{pmatrix}$$
3.  **Masked Scores:** Adding $-\infty$ to the future token positions suppresses their raw attention scores.
4.  **Softmax:** Since $\exp(-\infty) = 0$, the attention weights for future tokens become exactly $0.0$, preventing information leakage from future positions.

---

### 10. The Training Objective & Loss Function

#### Output Generation Head
*   **Vocabulary Mapping:** The final contextualized vector of a token passes through a Linear Layer (LM Head) projecting it to raw vocabulary logits.
*   **Probability Generation:** A Softmax layer transforms these logits into a probability distribution over the vocabulary.

#### Loss Function
*   **Cross-Entropy Loss:** Measures the discrepancy between the predicted probability distribution and the ground truth target:
    $$\text{Loss} = -\log(P(\text{true\_token}))$$

#### Sequence Alignment Shift
*   **Shift Mechanics:** During training, target labels are shifted left by one step relative to the inputs:
    *   At $t=0$: Input is `"The"` $\rightarrow$ Target Label is `"cat"`
    *   At $t=1$: Input is `"cat"` $\rightarrow$ Target Label is `"sat"`
*   **Rationale:** This design discards the first step's dummy prediction and ensures the model calculates loss only on tokens for which it has established context.

---

### 11. PyTorch Implementation

#### Implementation Overview
*   **Pre-LN Architecture:** Modern implementations apply LayerNorm before the sublayers (Pre-LN) rather than after (Post-LN), which has been shown to improve training stability in deep models.

```python
import torch.nn as nn
import torch.nn.functional as F

class TransformerBlock(nn.Module):
    def __init__(self, n_embd, n_head):
        super().__init__()
        # MultiHeadAttention and FeedForward classes are defined elsewhere
        self.attn = MultiHeadAttention(n_head, n_embd)
        self.ffn = FeedForward(n_embd)

        # Stabilization layers
        self.ln1 = nn.LayerNorm(n_embd)
        self.ln2 = nn.LayerNorm(n_embd)

    def forward(self, x):
        # Sublayer 1: Attention + Residual Connection (Pre-LN)
        x = x + self.attn(self.ln1(x))

        # Sublayer 2: FFN + Residual Connection (Pre-LN)
        x = x + self.ffn(self.ln2(x))
        return x
```