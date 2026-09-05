Quelle règle mathématique permet de décomposer une distribution jointe en un produit de distributions conditionnelles ?
<mark style="background: #FFB86CA6;">La règle de la chaîne de probabilité.</mark>
### Generative Modeling Paradigms: One-Shot vs. Sequential
*   **The VAE/GAN Approach (One-Shot / Parallel):**
    *   Variational Autoencoders (VAEs) and Generative Adversarial Networks (GANs) learn a mapping from a latent space to the data space.
    *   Generation involves sampling a random noise vector $z$ and passing it through a generator or decoder network.
    *   The entire output (such as a complete $256 \times 256$ image) is generated simultaneously in a single forward pass.
*   **The Autoregressive Approach (Sequential):**
    *   This approach avoids generating the entire output at once. Instead, <mark style="background: #FFF3A3A6;">the data is broken down into individual elements</mark> (such as pixels, characters, or words) which are generated sequentially, one step at a time.
    *   To generate a specific element (e.g., the 10th pixel), the model must first look at all preceding elements (pixels 1 through 9).

---

### Sequence Modeling Foundations
*   **Causality and Time:**
    *   <mark style="background: #FFF3A3A6;">Sequence data is inherently temporal and causal.</mark>
    *   In a time-series (like stock prices), the future depends on the past.
    *   In speech, the next sound produced depends on the syllables previously spoken.
    *   <mark style="background: #FFF3A3A6;">Autoregressive modeling is designed to directly mirror this forward flow of time.</mark>
*   **Discrete Structure:**
    *   Human language follows strict grammatical and syntactical rules where the order of elements is critical (e.g., "The dog bit the man" has a completely different meaning than "The man bit the dog").
    *   <mark style="background: #FFF3A3A6;">Generating tokens sequentially helps the model maintain grammatical consistency step-by-step</mark>.

---

### Mathematical Formulations and the Chain Rule of Probability
*   **The Problem:** Direct calculation of the joint probability of a long sequence $p(x_1, \dots, x_T)$ is <mark style="background: #BBFABBA6;">computationally intractable</mark>.
	- To represent the full joint distribution `p(x1,…,xN)` without any assumptions, we would need to assign a probability to `every possible combination of variables.
	- The total number of possible combinations is $K^{N}$
	- To store this distribution as a lookup table, we would need to learn and store $K^{N}-1$ parameters
*   **The Solution:** The chain rule of probability is used to decompose the joint distribution into a product of conditional distributions.
*   **The Equation:** 
    $$p(x) = p(x_1, x_2, \dots, x_T) = p(x_1) \cdot p(x_2 \mid x_1) \cdot p(x_3 \mid x_1, x_2) \cdots p(x_T \mid x_1, \dots, x_{T-1})$$
*   This sequence can be represented as a directed graphical model where each token $x_t$ receives directed edges from all preceding tokens $x_{<t}$.

---

### Core Definition and Mechanics of Autoregressive Models
*   **Definition:** An autoregressive model explicitly learns the conditional distribution $p(x_t \mid x_{<t})$.
*   **Output:** The model does not directly output the next token itself. Instead, <mark style="background: #BBFABBA6;">it outputs a probability distribution over all possible next tokens in the vocabulary.</mark>
*   **Objective:** The parameters $\theta$ are trained so that the true next token in the training data is assigned the highest probability.
*   **Training View (Conceptual):**
    *   Ground truth sequences are used.
    *   Input to model: $\left[\text{"The", "cat", "sat"}\right]$
    *   Target for model: $\text{"on"}$, which is represented as a one-hot vector or vocabulary index corresponding to the target word.
*   **Generation View (Inference Loop):**
    1.  **Input Context:** Provide the initial sequence (e.g., $\left[\text{"The", "cat", "sat"}\right]$).
    2.  **Model Forward Pass:** Compute the output probabilities.
    3.  **Output Probabilities:** <mark style="background: #FFB86CA6;">The model generates probability values for the next step</mark> (e.g., $\text{"on"}: 0.85$, $\text{"the"}: 0.10$, etc.).
    4.  **Sampling:** Select the next token (e.g., $\text{"on"}$) based on the generated probability distribution.
    5.  **Update Context:** Append the sampled token to the sequence (e.g., new context becomes $\left[\text{"The", "cat", "sat", "on"}\right]$) and repeat.

---
#### why using the log-likelihood:
1. Pobabities are between 0 and 1 so when you multiply many small fractions together, the number quickly becomes incredibly small. he computer’s memory will round this number down to 0. This is called <mark style="background: #FFB8EBA6;">numerical underflow.</mark>
2. <mark style="background: #FFB8EBA6;">Simpler Calculus</mark>. Taking the derivative of a sum (`∑`) is very simple because <mark style="background: #FFB8EBA6;">the derivative of a sum is just the sum of the derivatives</mark>. This makes gradient calculations much faster and more stable.

### The Log-Likelihood Objective
*   Using the chain rule, the likelihood of a sequence is:
    $$p(x) = \prod_{t=1}^T p(x_t \mid x_{<t})$$
*   For numerical stability, instead of maximizing the raw product $p(x)$, models are trained by maximizing the log-likelihood:
    $$\log(p(x)) = \sum_{t=1}^T \log\left(p(x_t \mid x_{<t})\right)$$
*   In deep learning frameworks, this is framed as a minimization problem. Minimizing the loss corresponds to minimizing the negative log-likelihood:
    $$\text{Loss} = -\log(p(x)) = -\sum_{t=1}^T \log\left(p(x_t \mid x_{<t})\right)$$
    *   This exact formulation corresponds to the <mark style="background: #BBFABBA6;">Softmax Cross-Entropy Loss</mark>.

---

# Modeling Sequence Probabilities with Recurrent Neural Networks (RNNs)
*   <mark style="background: #FFB8EBA6;">The RNN is a common choice for sequence modeling because it processes data sequentially and maintains an internal "memory" or "summary" of past inputs, known as the hidden state</mark> ($h$).
*   To compute $p(x_t \mid h_{t-1})$ in PyTorch, the system utilizes a three-step pipeline:
    1.  **The Embedding Layer (Categorical to Continuous):** Initially, each word in the vocabulary is represented by a simple whole identifier (e.g. "cat" = 1, "dog" = 2). These numbers have no real mathematical value 
		- **The solution :** The Embedding layer transforms these identifiers into **Continuous vectors** (lists of decimal numbers) in a fixed dimension space (e.g., a 768-dimensional vector for a complex model
	    - Pytorch provides an “<mark style="background: #FFB86CA6;">nn.Embedding</mark>” module for this task.
    2.  **The RNN Core (Context Processing):** <mark style="background: #FFB86CA6;">Input: </mark> $x_t$ and the previous hidden state $h_t$The RNN cell mixes the old memory with the new word using weight matrices ($W_{hh}$ and $W_{xh}$) and passes them through a non-linear activation function (typically <mark style="background: #FFB86CA6;">tanh</mark>). <mark style="background: #FFB86CA6;">Output:</mark> A newly updated hidden state $h_t$
    3.  **The Output Head (Continuous to Probability):** Converts the processed context vector back into a <mark style="background: #FFF3A3A6;">probability distribution </mark>over the vocabulary using a <mark style="background: #FFF3A3A6;">linear projection</mark> followed by a <mark style="background: #FFF3A3A6;">Softmax</mark> function(for mormalization).<mark style="background: #FFB86CA6;">Output: </mark>A probability distribution over the entire vocabulary, indicating how likely each word is to be the next token

---

### PyTorch Implementation Details: Embeddings and Batching
*   **Embeddings:**
    *   <mark style="background: #FFF3A3A6;">Embedding layers function as lookup tables to map integer values to dense vector representations.</mark>
    *   PyTorch provides the `nn.Embedding` module for this task.
    *   *Example configuration:* `nn.Embedding(vocab_size=5, out_dimension=3)` maps integer indices ($0$ to $4$) for words like $\text{"a", "b", "c", "go", "run"}$ to 3-dimensional continuous vectors.
##### Batching:
*   Training on batches of data improves GPU utilization and reduces training time.
* **Def:** Batch size is a hyperparameter in machine learning that specifies the **number of training examples processed together in a single step before the weights are updated**
*   When the batch size is greater than one, sequences often have varying lengths.
- **Why batching**
  - <mark style="background: #FFB86CA6;">Without Batching (Batch Size = 1): </mark>You feed one sentence into the model, calculate the error, update the weights, and repeat. This is highly inefficient because modern hardware like GPUs is designed to perform thousands of mathematical calculations in parallel. Processing one sentence at a time leaves 95% of the GPU's processing power idle.
    
- <mark style="background: #FFB86CA6;">With Batching (Batch Size = 32, 64, etc.):</mark> You group 32 or 64 sentences together into a single large matrix (a tensor) and feed them to the GPU at once. The GPU computes the outputs for all of them simultaneously, **significantly reducing training time** (as noted on Slide 10)

##### PAdding:
- A GPU requires data to be in a uniform, rectangular matrix to perform parallel calculations. To force sequences of different lengths into a uniform matrix, we must use **Padding**.
- We ad tokens called "PAD" to the end of all shorter sentences so they match the maximum length
- **Padding in pyTorch:** 
	La taille du lot pour cette étape de temps est réduite pour ne traiter que les séquences encore actives.

##### How Pytorch handels batched sequences:
1. **Sorting:** The sequences in the batch are sorted from longest to shortest.
    
2. **Step-by-Step Processing:** Instead of processing the entire rectangular matrix (including padding), PyTorch processes the batch step-by-step vertically, dynamically shrinking the batch size as shorter sequences finish:
    t: time steps
    - At t=3, all 5 sequences have real words 
    --> **Batch of 5**.
    - At t=4,  two sequences have hit PAD, leaving only 3 active sequences
    ---> **Batch of 3**.
    - At t=5,  only 2 sequences are still active
    ---> **Batch of 2**.
    - At t=6, only 1 sequence is active
    ---> **Batch of 1**.
![[Pasted image 20260604123048.png]]
        
This method allows the model to enjoy the speed benefits of GPU batching without wasting computation on padded tokens.

---

### RNN Mathematical Mechanics and Training
*   **The State Update:**
    $$h_t = f(h_{t-1}, x_t)$$
    Specifically:
    $$h_t = \tanh\left(W_{hh}h_{t-1} + W_{xh}x_t + b_h\right)$$
    *   This mixes the old memory ($h_{t-1}$) with the incoming word representation ($x_t$) to produce the updated memory ($h_t$).
*   **The Prediction:**
    $$p(x_{t+1} \mid x_{<t+1}) = g(h_t)$$
    *   This is implemented via a <mark style="background: #FFB86CA6;">linear projection </mark>followed by a <mark style="background: #FFB86CA6;">Softmax function</mark> to produce the probability distribution over the vocabulary for the next step.
#### Teacher Forcing vs. Free-Running Training:
*   **Free-Running Training (Inference style during training):** The model generates a prediction at step $t$ and feeds its own output as the input for step $t+1$. If the model makes a mistake, the error compounds, leading to gibberish and making gradients unstable or useless. <mark style="background: #FFF3A3A6;">Doesn't correct the errors till the end</mark>
*   **Teacher Forcing Training:** At each training step, if the model guesses incorrect, instead of feeding the model's mistake ("ct") into the next step, **you ignore the mistake and feed the ground-truth word "cat" from your dataset** as the next input. This keeps training fast and stable.
##### Drawback: Exposure bais
**Exposure Bais:** Le fait que le modèle ne sache pas récupérer de ses erreurs car il a toujours reçu des entrées parfaites durant l'entraînement.

- **During training:** The model is "babied" by the teacher and always receives perfect inputs.
- **During inference (real-world testing):** There is no teacher. The model has to rely on its own previous outputs. If it makes a small mistake early on, it may not know how to recover because it was never allowed to see or practice recovering from its own mistakes during training.

---

### Limitations and Bottlenecks of RNNs
##### The Sequential Bottleneck (Hardware Limitation):
*   The recurrence relation $h_t = f(h_{t-1}, x_t)$ creates a strict <mark style="background: #FFB86CA6;">data dependency where $h_t$ cannot be computed until $h_{t-1}$ is completed.</mark>
*   GPUs are designed for parallel matrix multiplication. <mark style="background: #FFB86CA6;">RNNs force sequential, single-step computation</mark>, leading to low GPU utilization and rendering training on long sequences computationally expensive. <mark style="background: #FFB86CA6;">No parrallelisme </mark>
##### The Long-Range Dependency Problem (Mathematical Limitation):
*   <mark style="background: #ABF7F7A6;">Information must pass through many sequential steps to influence distant future predictions.</mark>
*   <mark style="background: #ABF7F7A6;">Backpropagation Through Time (BPTT) </mark>requires multiplying gradients across every step:
        $$\frac{\partial L}{\partial h_1} = \prod_{t=2}^T \frac{\partial h_t}{\partial h_{t-1}}$$
*   **Vanishing Gradients:** If the derivative values are less than 1, by the time the error signal travels back to the first few words of the sentence, the gradient has "vanished" (it has become virtually zero). The model forgets early inputs, and no learning signal reaches the initial steps.
*   **Exploding Gradients:** If derivative values are greater than 1, gradients grow exponentially, causing numerical instability<mark style="background: #FFF3A3A6;"> (NaN values) and model crashes.</mark>
##### How These Problems Are Mitigated
- **Gradient Clipping (For Exploding Gradients):** If the gradient exceeds a certain threshold (e.g., 5.0), <mark style="background: #FFF3A3A6;">the computer artificially clips it back down</mark> to prevent the model from crashing.
    
- **LSTMs and GRUs (For Vanishing Gradients - Slide 15):** Long Short-Term Memory networks (LSTMs) introduce **additive cell states**. Instead of multiplying gradients through time, they use addition, which allows the gradient to travel backward over hundreds of steps without shrinking.
    
- **The Attention Mechanism (Direct access):** Instead of squeezing the past into a single vector, the model keeps <mark style="background: #BBFABBA6;">all previous words (hidden states) active in memory weighting them by importance for the current prediction..</mark> When predicting the next word, the model **looks back directly** at all previous words simultaneously and decides which ones are most relevant.
##### The Three Key Bottlenecks
1.  **The Speed Bottleneck:** Sequential computation forces the hardware to remain mostly idle (e.g., 5% computation, 95% idle silicon).
2.  **The Memory Bottleneck:** Information from early steps becomes diluted or lost by the time the sequence spans hundreds of steps.
3.  **The Routing Bottleneck:** Related tokens separated by distance have no direct path. Signals must pass through all intermediate tokens, increasing the effective path length.

---

### The Transition to the Attention Mechanism
*   **The New Ideal for Sequence Models:**
    *   **Parallelizability:** Process all tokens at once during training via <mark style="background: #BBFABBA6;">matrix multiplication rather than a sequential loop.</mark>
    *   **Direct Connections:** Allow any token to directly influence any other token, reducing the path length between any two tokens to exactly 1 step.
    *   **Computational Efficiency:** Maintain a constant parameter count while preserving information over long distances.
*   **The Core Concept of Attention:**
    *   Instead of compressing the entire history into a single, fixed-size hidden state $h_t$ (which dilutes early information), the attention mechanism allows the model to "look back" at all previous hidden states directly, weighting them dynamically by relevance.
*   **Conceptual Mechanics of Attention:**
    1.  **Scoring:** When processing a specific word (e.g., "it"), <mark style="background: #FFF3A3A6;">the model compares it to every previous word in the sequence, assigning a relevance score</mark> (e.g., "it" vs "the" receives a low score; "it" vs "cat" receives a high score).
    2.  **Weighting:** These relevance scores are normalized using a <mark style="background: #FFF3A3A6;">Softmax function so they represent percentages that sum to 100%. </mark>
    3.  **Mixing:** The model computes an enriched representationfor the word by taking a weighted mixture of the representations of all previous words (e.g., if "cat" has a 90% weight, the new representation of "it" is dominated by the meaning of "cat").
*   **Paradigm Comparison:**
    *   **RNN Computational Graph:** Path length from start to current is $O(N)$, features information bottlenecks at each step, and is strictly sequential.
    *   **Attention Computational Graph:** Path length from start to current is $O(1)$, has no information bottlenecks due to direct access to all past tokens, and computes connections in parallel via matrix mathematics.

---

### Bahdanau Attention (Additive Attention)
* **BIdirectional RNN:** Ils capturent à la fois ce qui précède et ce qui suit un mot dans une phrase
*   **Architecture:** <mark style="background: #FFF3A3A6;">Employs a bidirectional RNN as an encoder and a standard RNN as a decoder</mark>, <mark style="background: #FFB86CA6;">with an attention mechanism operating between them.</mark>
* The attention mechanism is not calculated once and for all. He is **Repeated sequentially** for each new word generated
*   **Key Components:**
    *   $s_{t-1}$: The hidden decoder state at the previous time step $t-1$.
    *   $c_t$: Il contient une version filtrée de l'entrée, focalisée sur les mots pertinents pour l'étape actuelle du décodeur. C'est pour gerer la variabilité de l'importance des mots d'entrée. 
    *   $h_i$: The annotation capturing information from the input sentence $\{x_1, \dots, x_T\}$, focusing around the $i$-th input word.
    *   $\alpha_{t,i}$: The normalized attention weight assigned to annotation $h_i$ at decoder step $t$.
    *   $e_{t,i}$: The raw attention score indicating how well $s_{t-1}$ matches $h_i$.
*   **The Encoder Mechanics:**
    *   The bidirectional RNN reads the input sentence of length $T$ in the forward direction to produce forward states $\vec{h}_i$, and in the reverse direction to produce backward states $\overleftarrow{h}_i$.
    *   The final annotation $h_i$ for word $x_i$ is the concatenation of these states:
        $$h_i = \left[ \vec{h}_i^T, \overleftarrow{h}_i^T \right]^T$$
    *   <mark style="background: #ABF7F7A6;">This representation contains summary information from both preceding and succeeding contexts around</mark> $x_i$.
*   **The Decoder Mechanics:**
    *   The alignment model $a(\cdot)$ <mark style="background: #FFF3A3A6;">evaluates the compatibility of the previous decoder state</mark> $s_{t-1}$ and each annotation $h_i$ to output an attention score $e_{t,i}$:
        $$e_{t,i} = a(s_{t-1}, h_i)$$
    *   **Two Common Alignment Implementations:**
        1.  $a(s_{t-1}, h_i) = v^T \tanh\left(W[h_i ; s_{t-1}]\right)$
        2.  $a(s_{t-1}, h_i) = v^T \tanh\left(W_1 h_i + W_2 s_{t-1}\right)$  *(where $v$ is a learned weight vector)*
    *   The attention scores are normalized using Softmax to produce weights:
        $$\alpha_{t,i} = \text{softmax}(e_{t,i}) = \frac{\exp(e_{t,i})}{\sum_{j=1}^T \exp(e_{t,j})}$$
    *   The context vector $c_t$ is computed as the weighted sum of the annotations:
        $$c_t = \sum_{i=1}^T \alpha_{t,i} h_i$$
*   **Step-by-Step Bahdanau Attention Algorithm:**
    1.  The encoder processes the input sequence to generate a set of annotations $\{h_i\}$.
    2.  The annotations and the previous hidden decoder state $s_{t-1}$ are fed to the alignment model to produce raw attention scores $e_{t,i}$.
    3.  A Softmax function is applied to the raw scores, normalizing them into weights $\alpha_{t,i}$ bounded between 0 and 1.
    4.  The context vector $c_t$ is computed as the weighted sum of the annotations using these weights.
    5.  The context vector $c_t$ and the previous hidden state $s_{t-1}$ are passed to the decoder to compute the current hidden state $s_t$ and output token $y_t$.
    6.  The process (steps 2 through 5) repeats sequentially for each step until the end of the target sequence is reached.

#### Why Bahdanau attention algorithm is additive:
<mark style="background: #FFB8EBA6;">Parce qu'elle utilise souvent une somme à l'intérieur de la fonction tanh pour calculer les scores</mark>