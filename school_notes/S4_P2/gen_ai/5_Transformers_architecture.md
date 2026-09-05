### Architecture 1: The End-to-End Decoder-Only (GPT) Architecture
*This is the entire model, from the raw prompt you type to the next-word prediction.*

```
[Raw Tokens] 
     ↓
1. Input Preparation (Embeddings + Positional Encoding)
     ↓ [Input Matrix]
2. N x Decoder Blocks (Stack of Layers)
     ↓ [Contextualized Matrix]
3. Final Layer Norm
     ↓ [Normalized Matrix]
4. LM Head (Linear Layer)
     ↓ [Logits]
5. Softmax
     ↓
[Probability Distribution]
```

#### 1. Input Preparation
*   **Takes:** Raw text tokens (e.g., $N$ tokens: `["The", "cat", "sat"]`).
*   **Process:** Looks up the static word <mark style="background: #FFB8EBA6;">embedding</mark> for each token and adds its corresponding Positional Embedding vector.
*   **Outputs:** An input representation matrix $X_0$ of shape $[N \times d_{\text{model}}]$.

#### 2. Stacks of Decoder Blocks (Repeated $N$ times)
*   **Takes:** The matrix $X_{l-1}$ from the previous layer (or $X_0$ for the first layer) of shape $[N \times d_{\text{model}}]$.
*   **Process:** Passes the matrix through <mark style="background: #FFB8EBA6;">self-attention and feed-forward</mark> operations (detailed in Architecture 2 below) to blend token meanings.
*   **Outputs:** A highly contextualized matrix $X_{\text{final}}$ of shape $[N \times d_{\text{model}}]$.

#### 3. Final Layer Norm
*   **Takes:** Contextualized matrix $X_{\text{final}}$ of shape $[N \times d_{\text{model}}]$.
*   **Process:** <mark style="background: #FFB8EBA6;">Normalizes the activations of the final layer.</mark>
*   **Outputs:** A stabilized matrix of shape $[N \times d_{\text{model}}]$.

#### 4. LM Head (Linear Layer)
*   **Takes:** The vector corresponding to the *very last* token in the sequence (shape: $[1 \times d_{\text{model}}]$).
*   **Process:** Multiplies this vector by a projection matrix mapping it to the size of the vocabulary.
*   **Outputs:** A vector of raw scores (**Logits**) of shape $[1 \times \text{Vocab Size}]$ (e.g., $1 \times 50,257$).

#### 5. Softmax
*   **Takes:** Logits vector of shape $[1 \times \text{Vocab Size}]$.
*   **Process:** Exponentiates and normalizes the scores so they sum to 1.0.
*   **Outputs:** A probability distribution over the vocabulary of shape $[1 \times \text{Vocab Size}]$, identifying the most likely next word.

---

### Architecture 2: A Single Transformer Decoder Block
*This is one repeating block inside the $N$-layer stack.*

```
                 Input Matrix [N x d_model]
                       /             \
                      |         [Layer Norm]
                      |               ↓
                      |       Masked Multi-Head Attention
                      |               ↓
                       \------------(+)  <-- Residual Connection
                                      │
                                [Layer Norm]
                                      │
                               Feed-Forward Net (FFN)
                                      │
                                     (+)  <-- Residual Connection
                                      │
                                 Output Matrix [N x d_model]
```

#### 1. Masked Multi-Head Attention Sublayer (Pre-LN Setup)
*   **Takes:** Input token matrix $X$ of shape $[N \times d_{\text{model}}]$.
*   **Process:** 
    1. Normalizes the input: $X_{\text{norm}} = \text{LayerNorm}(X)$.
    2. Runs masked attention on $X_{\text{norm}}$ (tokens can only attend to past tokens).
    3. Adds the original input $X$ (residual skip connection): $X_{\text{attn}} = X + \text{Attention}(X_{\text{norm}})$.
*   **Outputs:** Intermediate matrix $X_{\text{attn}}$ of shape $[N \times d_{\text{model}}]$.

#### 2. Feed-Forward Network (FFN) Sublayer (Pre-LN Setup)
*   **Takes:** Intermediate matrix $X_{\text{attn}}$ of shape $[N \times d_{\text{model}}]$.
*   **Process:**
    1. Normalizes the intermediate input: $X_{\text{attn\_norm}} = \text{LayerNorm}(X_{\text{attn}})$.
    2. Passes each token independently through a 2-layer MLP (linear $\rightarrow$ activation $\rightarrow$ linear).
    3. Adds the original $X_{\text{attn}}$ (residual skip connection): $Y = X_{\text{attn}} + \text{FFN}(X_{\text{attn\_norm}})$.
*   **Outputs:** Block output matrix $Y$ of shape $[N \times d_{\text{model}}]$.

---

### Architecture 3: Multi-Head Attention (MHA)
*This is the calculation occurring inside the Attention sublayer mapped above.*

```
                       Input Matrix X [N x d_model]
                               /    |    \
                             W_Q   W_K   W_V  (Linear Projections)
                             /      |      \
                           Q_i     K_i     V_i (for each head h)
                             \      |      /
                              [Attention] (Scaled Dot-Product per head)
                                    ↓
                            Head Outputs [N x d_k]
                                    ↓
                               [Concatenate]
                                    ↓
                          Concat Matrix [N x d_model]
                                    ↓
                               W_O (Linear Output Projection)
                                    ↓
                           Output Matrix Z [N x d_model]
```

#### 1. Projection Stage
*   **Takes:** Input matrix $X$ of shape $[N \times d_{\text{model}}]$.
*   **Process:** Multiplies $X$ by learned matrices $W_Q, W_K, W_V$ for each of the $h$ heads.
*   **Outputs:** $h$ sets of Query, Key, and Value matrices ($Q_i, K_i, V_i$), each of shape $[N \times d_k]$ (where $d_k = d_{\text{model}} / h$).

#### 2. Parallel Scaled Dot-Product Attention
*   **Takes:** $Q_i, K_i, V_i$ matrices of shape $[N \times d_k]$ for each head.
*   **Process:** For each head, calculates $\text{softmax}\left(\frac{Q_i K_i^T}{\sqrt{d_k}}\right)V_i$.
*   **Outputs:** $h$ contextualized head matrices, each of shape $[N \times d_k]$.

#### 3. Concatenation
*   **Takes:** $h$ matrices of shape $[N \times d_k]$.
*   **Process:** Joins the matrices side-by-side along the column dimension.
*   **Outputs:** A single concatenated matrix of shape $[N \times d_{\text{model}}]$.

#### 4. Final Output Projection ($W^O$)
*   **Takes:** Concatenated matrix of shape $[N \times d_{\text{model}}]$.
*   **Process:** Multiplies by a final learned linear layer $W^O$ to mix the information gathered by the different heads.
*   **Outputs:** Final attention output matrix $Z$ of shape $[N \times d_{\text{model}}]$.