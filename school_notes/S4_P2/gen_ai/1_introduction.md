- **AI** is the broad, overarching field; **ML** is a technique that powers AI; **DL** is an advanced method of ML; and **Generative AI** is a specialized branch of DL

### Types of data and domains of generative ai:
○ Text generation
○ Image generation
○ Audio generation
○ Video generation

### Generative modeling:
Generative modeling is a branch of deep learning that involves training a model to produce new data that is similar to a given dataset

### Discriminative vs generative models
#### Descriminative:
- When performing discriminative modeling, each observation in the training data has a <mark style="background: #FFB8EBA6;">label</mark>
- Discriminative modeling estimates p(y|x), which aims to model the probability of a label y given some observation x
#### Generative:
- Doesn’t require the dataset to be labeled because it concerns itself with generating entirely new images.
Generative models learn the underlying probability distribution p(x) of a dataset. By sampling from a latent space z and passing it through the learned mapping f(z) = y, the model can synthesize new data points that are statistically consistent with the training distribution.

### Density estimation models:
### **1. Explicit Density Estimation Models**
These models focus on defining the <mark style="background: #FFF3A3A6;">mathematical structure of the data's probability </mark>distribution(it is describing the **mathematical tool** used to achieve that real data, that's why we use it).

*   **Core Idea:** The model explicitly defines and learns the actual probability distribution of the data.
*   **Key Feature:** You have the ability to **calculate the likelihood** of any specific data point ($x$). This means you can plug a piece of data into the model and get a number representing how likely that data is to exist within the learned distribution.
*   **Mathematical Goal:** To learn $p_{\theta}(x)$ (the probability of $x$ given the model parameters $\theta$).
*   **Examples:**
    *   **Variational Autoencoders (VAEs):** These use an encoder-decoder structure to approximate the distribution.
    *   **Autoregressive Models:** Such as **GPT**, which predict the next piece of data (like a word) based on previous pieces, thereby building an explicit probability chain.

---

### **2. Implicit Models**
These models focus on the ability to create new data that looks like the original data, <mark style="background: #FFF3A3A6;">without needing a mathematical formula</mark> for the distribution itself.

*   **Core Idea:** The model learns a **process or mechanism** to draw new samples from the distribution ($p_{\theta}(x)$) without ever knowing the actual mathematical formula of that distribution.
*   **Key Feature:** The model is limited to **generating samples**. Unlike explicit models, you **cannot calculate the likelihood** of a specific data point ($x$). You can make more data, but you can’t mathematically score how "likely" a specific input is.
*   **Mathematical Goal:** To learn a sampler $G_{\theta}(z)$. 
    *   The process involves taking a random noise vector $z$ from a simple distribution ($z \sim p(z)$) and passing it through the generator function ($G_{\theta}(z)$) to produce a realistic data point $x$.
*   **Examples:**
    *   **Generative Adversarial Networks (GANs):** These use a "generator" and a "discriminator" competing against each other to create realistic data.
    *   **Diffusion Models:** These learn to generate data by reversing a process of adding noise to images.
---
### The High-Dimensional Data challenge

- For each image we have: 256×256×3=196,60 individual numbers. The possible number of combinations is: $256^{196,608}$
- Because <mark style="background: #FFF3A3A6;">the space of all possible pixel</mark> combinations is so vast, almost every point in this space represents meaningless, random static
- **The Sparsity Obstacle:** Because almost all of this space is <mark style="background: #FFF3A3A6;">empty</mark>, it is mathematically impossible to <mark style="background: #FFF3A3A6;">collect enough data</mark> to model the probability distribution`p(x)` directly.
##### Solution:
- **The Manifold Assumption:** To bypass this, we assume that real, meaningful data actually lies on a much <mark style="background: #ABF7F7A6;">simpler, lower-dimensional surface </mark>(a manifold).
- **The Latent Solution:** Latent variable models solve the problem by <mark style="background: #ABF7F7A6;">mapping the data</mark> to a smaller, manageable space (`z`) that represents the <mark style="background: #ABF7F7A6;">coordinates</mark> of this lower-dimensional surface.

---
# The Search for Latent Structure
- High-dimensional data (such as a highly detailed image of a swan, represented as`x` often contains a **low-dimensional essence**. The search for latent structure aims to **compress** this high-dimensional data into a low-dimensional latent code (`z`) representing <mark style="background: #FFF3A3A6;">fundamental factors </mark>of variation like pose, color, or texture.

### Encoders and the Latent Space
##### Deterministic Encoders:
- A deterministic encoder network, denoted as`gϕ​`, is used to <mark style="background: #ABF7F7A6;">compress</mark> high-dimensional input`x`into a low-dimensional latent representation`z`at the "<mark style="background: #ABF7F7A6;">bottleneck layer</mark>".

- **PCA Vs AE:** Unlike Principal Component Analysis (PCA), which is limited to <mark style="background: #FFB8EBA6;">linear relationships</mark>, neural-network-based AE are highly flexible and capable of learning <mark style="background: #FFB8EBA6;">complex, non-linear</mark> features and representations.
- **The Fatal Flaws of AE:** 
	- Their <mark style="background: #FFB8EBA6;">latent space is unstructured and contains "holes"</mark>.
	- There is <mark style="background: #FFB8EBA6;">no defined probability</mark> distribution`p(z)`to sample from
	--> Sampeling a random z will end up in a random place the decoder has never seen    --> garbage output

---
### Latent variable model frameworks:
- the **latent vector** (`z`) is a <mark style="background: #BBFABBA6;">low-dimensional</mark>, compressed numerical representation of a high-dimensional data point `x`.
- It encodes the <mark style="background: #BBFABBA6;">abstract</mark> concepts needed to generate x.

- <mark style="background: #BBFABBA6;">p(x, z) = p(x|z) p(z)</mark> statistical view (deterministic version: x= f(z))⇒ tells us how to create a data point x from a latent code z.
#### The components of the framework:
##### The Prior Distribution, p(z):
- It's the probability of a specific latent vector z
- To make things simple, we typically choose a standard, easy-to-sample-from distribution, like a multivariate Gaussian: <mark style="background: #ABF7F7A6;">z ~ N(0, I)</mark>

##### The Generator (or Decoder),p(z|x)
- This is the complex, non-linear part of the model, implemented as a <mark style="background: #ABF7F7A6;">deep neural network</mark>.
- it maps a latent vector `z` to a probability distribution over the data space `x` (often generating an approximation `x'`

#### How Data is Generated in this Framework 

1. **Sample** a latent vector`z` from the simple prior distribution`p(z)`(i.e., choosing a random setting on the "control panel").
    
2. **Feed** this sampled vector`z` into the generator network`p(x∣z)`to produce a new, realistic data sample`x`.
*NB:*
`p(x∣z)`s written as a probability, but in practice, it acts as a "Generator."**

---
### Marginalization:
- <mark style="background: #BBFABBA6;">Marginalization</mark> is the process of getting from the joint distribution p(x, z) to the data distribution p(x).
- we do it because we almost never know the latent code z for a piece of data x.
- The marginal likelihood p(x) is the bridge that connects our latent variable model to our training objective: by integrating out `z`(calculating `p(x)=∫p(x∣z)p(z)dz`) , <mark style="background: #BBFABBA6;">we convert our latent-based model equations into a pure probability</mark> of the observed data,`p(x)p(x)`
- **Problem:** Even the latent space stays very large to compute the integral.

---
## Inference problem:
- **The inference problem:** determining the hidden, low-dimensional latent variables (`z`) <mark style="background: #FFF3A3A6;">that correspond to a specific observed piece of data</mark> (`x`).
#### Why?
- We can compress a massive, complex file into a small, clean vector of just a few numbers
- each coordinate in the latent vector `z` represents a single, interpretable concept. By finding `z`, we can analyze semantic features directly from the numbers
- Once we know the latent code `z` of an existing image, we can tweak specific coordinates (e.g., manually increase the "smiling" coordinate) and decode it back to create a modified version of the original image

#### Mathematically:
- It requires calculating the **posterior probability distribution**, denoted as $p(z \mid x) = \frac{p(x \mid z)\, p(z)}{p(x)}$

#### Problem:
- We do not have `p(x)` (the marginal likelihood, which requires the impossible integration over the entire latent space)

#### Solution:
we must use approximation techniques, such as **Variational Inference**, to estimate`p(z∣x)`using a simpler, tractable distribution`q(z∣x)`

#### Variational inference:
we use when we want to calculate something that is mathematically impossible to solve. it's like an estimation from a distribution very similar to the truth.


---
## Most likelihood estimation:

### 1. The Core of MLE 
* **The Principle:** The "best" model is <mark style="background: #FFB86CA6;">the one that makes the real-world data we actually observed most probable</mark>. 
* **The Goal:** It selects the exact model settings (`θ`) that make the data you actually observed have the highest possible chance of matching the reality.
* **Distribution Alignment:** In doing so, we are actively trying to make our <mark style="background: #FFB86CA6;">model's probability distribution</mark>, $p_{\text{model}}(x)$, <mark style="background: #FFB86CA6;">match the real-world distribution of reality,</mark> $p_{\text{data}}(x)$, as closely as possible.
* **Dataset Assumption:** We assume the training dataset $D = \{x_1, x_2, \dots, x_N\}$ consists of $N$ <mark style="background: #FFB86CA6;">independent and identically distributed (IID)</mark> data points.
* **The value calculated:** 
	* For one image in our dataset,`pθ(x)`is the **probability** (or "chance") that our model, with its current settings (`θ`), would generate that exact image.
	* For the Whole Dataset: The Joint Likelihood,`L(θ)`. It tells us <mark style="background: #FFB86CA6;">if the model is able to generate the entire training dataset </mark>as it is. 

---

### 2. Mathematical Formulation
#### A. The Likelihood Function, $L(\theta)$
The likelihood of the entire dataset $D$ given the parameters $\theta$ is the joint probability of observing all data points under our model:
$$L(\theta) = p_\theta(D) = p_\theta(x_1, x_2, \dots, x_N)$$

#### B. Factoring the Probability (The IID Assumption)
Because the data points are assumed to be independent, this joint probability can be factored into a simple product of individual data point probabilities:
$$L(\theta) = \prod_{i=1}^N p_\theta(x_i)$$

#### C. Transitioning to Log-Likelihood
- <mark style="background: #FFB8EBA6;">It is much easier to work with sums. Therefore, we apply a logarithm to maximize the log-likelihood instead:</mark>
$$\log L(\theta) = \log \left( \prod_{i=1}^N p_\theta(x_i) \right) = \sum_{i=1}^N \log p_\theta(x_i)$$
#### D. Interpretation:
Because we take the logarithm of these probabilities (which are decimals between  0 and 1), the actual value we measure is almost always a **negative number**.

Here is how to interpret the value during training:

- **If the value is a large negative number (- 100000):** This means the model is performing poorly. It assigns a near-zero probability to the real-world data.
    
- **If the value gets closer to 0:** This means the model is improving. The probabilities it assigns to the real-world images are getting closer to 1 (100%). 

---
# Kullback-Leibler Divergence (KLD):
### 1. Definition and Purpose 
- It Acts as the <mark style="background: #BBFABBA6;">"Loss Function" for the Encoder</mark>
* **What it does:** It quantifies the "distance" or "difference" between the real data and the generated data($P$ and $Q$).
* **Formula:** The KLD from distribution $P$ to distribution $Q$ is defined as:
  $$D_{KL}(P \parallel Q) = \mathbb{E}_{x \sim P} \left[ \log \frac{P(x)}{Q(x)} \right]$$

---

### 2. Properties of KLD
* **Non-Negative**
* **Zero iff Identical:** It is exactly zero if, and only if, $P$ and $Q$ are the same distribution ($P = Q$).
* **Asymmetric:** The "distance" from $P$ to $Q$ is not equal to the "distance" from $Q$ to $P$ ($D_{KL}(P \parallel Q) \neq D_{KL}(Q \parallel P)$).

---

### 3. Connection to Maximum Likelihood
* Minimizing the KLD between the true distribution of reality ($p_{\text{data}}$) and our model's distribution ($p_\theta$) is mathematically <mark style="background: #BBFABBA6;">identical to maximizing the expected log-likelihood</mark>.
* Therefore:
  $$\text{Maximizing the Log-Likelihood} \equiv \text{Minimizing the KLD to the Data Distribution}$$

---

### 4. Role in Variational Inference 
Because the true posterior distribution $p(z|x)$ (finding the latent code $z$ from image $x$) is impossible to calculate, we approximate it using a simpler distribution $q_\phi(z|x)$ output by an encoder network. 

* **The Goal:** We want to minimize the KLD between our approximation and the true posterior :
  $$\min_\phi D_{KL}(q_\phi(z|x) \parallel p(z|x))$$
* **The Catch:** We cannot compute this KLD directly because it still contains the intractable true posterior $p(z|x)$ inside the equation\.
* **The Solution (The ELBO):** Through algebra, the course derives an identity linking the marginal likelihood to the KLD:
  $$\log p(x) = \text{ELBO} + D_{KL}(q(z|x) \parallel p(z|x))$$
  KLD is always $\geq 0$ (bcs it's a distance) so the ELBO is a lower bound on the log-likelihood ($\text{ELBO} \leq \log p(x)$). 
* Therefore, **maximizing the ELBO is mathematically equivalent to minimizing this impossible-to-calculate KLD**. Then we get closer to p(x)
* We push the ELBO up using **Stochastic Gradient Descent (SGD)**—the same engine that powers every neural network.