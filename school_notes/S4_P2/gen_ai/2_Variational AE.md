### 1. Introduction and Core Purpose of VAEs
* **From Theory to Practice:** 
  The Evidence Lower Bound (ELBO) is a tractable objective function derived to simultaneously maximize data likelihood and minimize the divergence between the approximate posterior distribution $q$ and the true posterior distribution $p$.
* **VAE:** it is a directed probabilistic generative model that combines <mark style="background: #BBFABBA6;">unsupervised deep learning with Bayesian variational inference</mark>
* **The VAE Architecture:** 
  The VAE serves as a direct physical implementation of the variational inference framework, transforming theoretical probabilistic concepts into a deep learning neural network architecture.
* **Variational inference:** 
we use when we want to calculate something that is mathematically impossible to solve. it's like an estimation from a distribution very similar to the truth.
#### VAE vs AE:
- A standard AE maps an input image x to a single, discrete point z in the latent space, and then tries to reconstruct it.
- **VAE's solution:** Instead of mapping an input to a <mark style="background: #FFB86CA6;">fixed point</mark>, it maps the input to a <mark style="background: #FFF3A3A6;">probability distribution</mark> over the latent space
---

### 2. Network Components: Encoder and Decoder

#### The Encoder Network $q_\phi(z|x)$ (The Approximate Posterior)
* **Definition:** A neural network parameterized by weights $\phi$.
* **Function:** It takes a high-dimensional input data point $x$ (such as an image) and compresses it into the parameters of a <mark style="background: #BBFABBA6;">lower-dimensional probability distribution</mark>. 
* **Design Choice:** The distribution $q$ is chosen to be a <mark style="background: #BBFABBA6;">multivariate Gaussian with a diagonal covariance matrix</mark>. Because a Gaussian is defined by its mean and variance, the encoder does not output a single latent vector $z$. Instead, it outputs two separate vectors:
  1. <mark style="background: #FFB86CA6;">The mean vector,</mark> $\mu(x)$.
  2. <mark style="background: #FFB86CA6;">The log-variance vector, </mark> $\log \sigma^2(x)$.
* **Role:** This represents the inference component of the model. It addresses the question: *"Given an input $x$, what is its likely latent code $z$?"

* **NB:** The encoder outputs a variance (`σ2`), which represents a "cloud of plausibility" around that mean. Because this forces the decoder to learn to <mark style="background: #FFB86CA6;">reconstruct the image</mark> not just from one perfect point, but from <mark style="background: #FFB86CA6;">any point nearby in that cloud</mark>. This is what makes the latent space continuous.

##### The cross-attention:
- **Q** : "Qu'est-ce que je cherche ?" (État du décodeur).
- **K** : "Qu'est-ce que je propose comme correspondance ?" (Index de l'encodeur).
- **V** : "Quelle est l'information réelle que je transmets ?" (Contenu de l'encodeur).

**Cross-Attention** is the computational bridge that connects the encoder and the decoder.

- **In Self-Attention:** 
	- Q , K, and V all come from the **same** input sequence.
- **In Cross-Attention:**
    - **Queries(Q):** Generated from the **Decoder's** current state (representing the target sequence generated so far—"What am I currently trying to write?").
    - **Keys (K):** Generated from the **Encoder's** final output (representing the source sequence—"What was the original input?").
    - **Values (V):** Generated from the **Encoder's** final output (representing the actual semantic details of the source sequence—"What did the original input actually mean?").

#### The Decoder Network $p_\theta(x|z)$ (The Likelihood / Generator)
* **Definition:** A neural network parameterized by weights $\theta$.
* **Input:** The actual latent vector`z`that is passed to the decoder is not either of these values. Instead, it is<mark style="background: #BBFABBA6;"> randomly sampled from the distribution</mark> defined by the mean and the mean and the variance:  `z∼N(μ,σ2)`. 
* Why the randomness in z necessary?
	it forces the latent space to be continuous and regularized
	
* **Function:** It takes a single sampled latent vector $z$ as input and maps it back to the parameters of a data distribution matching the original input space.
* **Design Choice (Data Dependent):** The choice of output distribution $p(x|z)$ depends on the data type:
  * *For Binary Data (e.g., MNIST):* It uses a <mark style="background: #FFF3A3A6;">Bernoulli</mark> distribution. The final layer of the decoder applies a sigmoid activation function to output a probability vector between 0 and 1 for each pixel.
  * *For Continuous Data (e.g., Color Images):* It assumes pixels are drawn from a <mark style="background: #FFF3A3A6;">Gaussian</mark> distribution. The decoder outputs the mean values for the pixels, while the variance is typically assumed to be fixed for simplicity.
* **Role:** This represents the generative component of the model. It addresses the question: *"Given a latent code $z$, what image $x$ does it generate?"*

---

### 3. VAE Loss Function Formulation

#### Conceptual Objective
The goal is to maximize the ELBO so the VAE loss function is defined as <mark style="background: #FFB8EBA6;">the negative ELBO</mark>:
$$\text{Loss} = -\text{ELBO}$$

Given the alternative form of the ELBO:
$$\text{ELBO} = \mathbb{E}_{z \sim q}[\log p(x|z)] - D_{\text{KL}}(q(z|x) \mid\mid p(z))$$

The exact VAE loss function is:
$$\text{Loss} = -\mathbb{E}_{z \sim q}[\log p(x|z)] + D_{\text{KL}}(q(z|x) \mid\mid p(z))$$

<mark style="background: #FFB8EBA6;">This loss is composed of two distinct parts:</mark>

#### Part A: The Reconstruction Loss
* **Intuition:** Measures how successfully the decoder reconstructs the original input $x$ from a sampled latent code $z$.
* Prevents the encoder from <mark style="background: #FF5582A6;">mapping everything to the origin</mark> (ignores the prior)
* **Practical Calculation:** Mathematically, the expectation requires multiple samples of $z$ from $q(z|x)$. In practice, using **just one sample $z$** per data point (generated via the reparameterization trick) provides a highly effective approximation.
* **Implementation by Data Type:**
  * *For Binary Data:* Minimizing the negative log-likelihood is equivalent to using <mark style="background: #FFF3A3A6;">the Binary Cross-Entropy (BCE) loss</mark> between the original pixels $x$ and the reconstructed pixels $x'$.
  * *For Continuous Data:* Minimizing the negative log-likelihood is proportional to <mark style="background: #FFF3A3A6;">the Mean Squared Error</mark> (MSE) between $x$ and $x'$.

- **If we remove it:** 
The encoder will try to make its output distribution`q(z∣x)` look exactly like the prior`p(z)`(a standard standard normal distribution,N(0,1)) because that makes the KL divergence exactly zero

#### Part B: The KL Divergence Loss
* **Intuition:** Acts as a regularizer. It measures <mark style="background: #FFF3A3A6;">how much the approximate posterior</mark> $q(z|x)$ <mark style="background: #FFF3A3A6;">deviates from the designated prior distribution</mark> $p(z)$. 
* The prior is defined as a <mark style="background: #FF5582A6;">simple standard Gaussian</mark>, $\mathcal{N}(0, I)$. This term forces the encoder to distribute latent codes closely around the origin.
* Prevents the encoder from <mark style="background: #FF5582A6;">scattering representations</mark> all over the place with huge empty gaps (ignores standard autoencoder fragmentation).
* **Practical Calculation:** Because both $q$ and $p$ are Gaussians with diagonal covariances, the KL divergence can be solved analytically. No sampling is required. It is computed directly and efficiently using the $\mu$ and $\log \sigma^2$ vectors:
  $$D_{\text{KL}}(\mathcal{N}(\mu, \sigma^2) \mid\mid \mathcal{N}(0, I)) = \frac{1}{2} \sum_{j} (1 + \log(\sigma_j^2) - \mu_j^2 - \sigma_j^2)$$
  *(Alternatively written as: $\frac{1}{2} \sum_{j=1}^{d} (1 + \log(\text{var})_j - \mu_j^2 - \exp(\log(\text{var})_j))$ where $\text{var} = \sigma^2$)*

- **If we remove it:** 
	- The model behaves exactly like a <mark style="background: #FF5582A6;">standard, deterministic Autoencoder</mark>
	- The latent space ends up with massive "empty" gaps and no structure. --> We can't generate new data
---

### 4. The Reparameterization Trick

###### Reminder:
<mark style="background: #ADCCFFA6;">steps of a single training loop:</mark>
forward pass(prediction) -> Calculating loss -> backward propagation -> weight update
#### How backward propagation works:
Backpropagation works by calculating the gradient of
the loss with respect to each parameter using the chain rule. To compute the gradient for
the encoder's weights φ, we need to compute ∂Loss / ∂φ. By the chain rule, this involves a
term like ∂z / ∂φ
#### The Optimization Problem
By the chain rule, this calculation involves the gradient component $\partial z / \partial\phi$. 

However, in the standard forward pass, the latent vector is sampled randomly: 
$$z \sim \mathcal{N}(\mu, \sigma^2)$$

 $z$ is a <mark style="background: #FFF3A3A6;">random</mark> outcome rather than a deterministic function of the encoder's weights, so the gradient $\partial z / \partial\phi$ is undefined, which <mark style="background: #FFF3A3A6;">blocks</mark> backpropagation through the latent node.

#### The Mathematical Solution
The <mark style="background: #FF5582A6;">reparameterization</mark> trick resolves this by relocating the source of randomness outside the trainable computation graph:
$$z = \mu + \sigma \odot \epsilon$$

* **$\epsilon$ (Epsilon):** A random noise vector sampled from a static, parameter-free distribution: $\epsilon \sim \mathcal{N}(0, I)$. The model does not update or learn parameters for $\epsilon$.
* **$\mu$ and $\sigma$:** The deterministic outputs of the encoder network.
* **$\odot$:** Element-wise multiplication.

This formulation isolates the stochasticity in $\epsilon$ while making $z$ a deterministic, differentiable function of $\mu$ and $\sigma$. Consequently, backpropagation can flow freely from the loss function, through the decoder, and back to update the encoder parameters $\phi$.

---

### 5. Training and Implementation

#### The Training Algorithm
1. Sample a minibatch of data $\{x_i\}$.
2. For each $x_i$, pass it through the encoder network to obtain parameters $\mu_i$ and $\sigma_i$.
3. Sample random noise $\epsilon_i \sim \mathcal{N}(0, I)$ and calculate the latent representation: $z_i = \mu_i + \sigma_i \odot \epsilon_i$.
4. Pass $z_i$ through the decoder network to generate the reconstructed output $x'_i$.
5. Compute the total loss: $\text{Loss} = \text{BCE}(x, x') + \text{KL}(\mu, \sigma)$.
6. Run backpropagation to calculate gradients and update the network parameters $\theta$ (decoder) and $\phi$ (encoder).

#### PyTorch Code Structure
The implementation is structured as a class inheriting from `nn.Module`:

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class VAE(nn.Module):
    def __init__(self, imageChannels=1, featureDim=32*20*20, zdim=256):
        super(VAE, self).__init__()
        
        # Encoder Layers
        self.enConv1 = nn.Conv2d(imageChannels, 16, 5)
        self.enConv2 = nn.Conv2d(16, 32, 5)
        self.enFC1 = nn.Linear(featureDim, zdim) # Outputs Mean
        self.enFC2 = nn.Linear(featureDim, zdim) # Outputs Log-Variance
        
        # Decoder Layers
        self.deFC1 = nn.Linear(zdim, featureDim)
        self.deConv1 = nn.ConvTranspose2d(32, 16, 5)
        self.deConv2 = nn.ConvTranspose2d(16, imageChannels, 5)
        
    def encoder(self, x):
        x = F.relu(self.enConv1(x))
        x = F.relu(self.enConv2(x))
        x = x.view(-1, 32*20*20)
        mu = self.enFC1(x)
        logVar = self.enFC2(x)
        return mu, logVar
        
    def reparameterize(self, mu, logVar):
        std = torch.exp(logVar / 2)
        eps = torch.rand_like(std) # Random noise vector
        return mu + eps * std
        
    def decoder(self, z):
        x = F.relu(self.deFC1(z))
        x = x.view(-1, 32, 20, 20)
        x = F.relu(self.deConv1(x))
        x = torch.sigmoid(self.deConv2(x))
        return x
        
    def forward(self, x):
        mu, logVar = self.encoder(x)
        z = self.reparameterize(mu, logVar)
        out = self.decoder(z)
        return out, mu, logVar
```

---

### 6. Analyzing the Latent Space

#### Standard Autoencoders (AEs) vs. Variational Autoencoders (VAEs)
Comparing 2D projections of the latent spaces reveals structural differences:
* **Standard Autoencoder Space:** The distribution is highly unorganized, containing vast gaps and unmapped regions. The coordinates are unconstrained and can span wide ranges (e.g., from $-10.0$ to $10.0$) with no central clustering.
* **VAE Latent Space:** The distribution is structured, continuous, and clustered symmetrically around the origin (ranging mostly between $-5.0$ and $5.0$). Distinct classes (such as MNIST digits) organize into distinct but contiguous clusters.

#### Latent Space Interpolation
The key advantage of a VAE's structured latent space is the ability to perform interpolation. This is the process of generating <mark style="background: #FFB8EBA6;">entirely new, coherent data</mark> points by moving along a continuous path between two known coordinates in the latent space (for example, smoothly transitioning from a generated digit "0" to a "6").

---

### 7. VAE Challenges and Variants

#### Problem 1: Blurry Reconstructions
* **The Cause:** VAEs optimize to maximize data likelihood. When a dataset contains multiple modes (such as different writing styles for the number "7"), <mark style="background: #FFB8EBA6;">the model averages these modes</mark> to maximize coverage across the distribution, resulting in <mark style="background: #FFB8EBA6;">blurred edges rather than crisp details</mark>.

#### Problem 2: Posterior Collapse
* **The Cause:** This occurs when the <mark style="background: #FFB8EBA6;">encoder learns to output the exact same latent distribution regardless of the input</mark> $x$. For a standard normal prior, this means the encoder defaults to outputting $\mu(x) = 0$ and $\sigma(x) = 1$ for all inputs.
* **The Result:** The encoder fails to extract useful features, and the VAE behaves like a basic generative model that simply samples from the prior and passes the same static noise to the decoder.
* **The Dynamic:** The loss function has competing forces:
  * *KL Loss:* Aims to minimize deviation from the prior, pulling $q(z|x)$ toward $p(z)$ to reach a minimum value of zero.
  * *Reconstruction Loss:* Aims to keep $z$ highly representative of input $x$, pulling $q(z|x)$ away from $p(z)$ toward data-specific coordinates. When the KL term dominates, posterior collapse occurs.

#### Variant 1: The $\beta$-VAE
<mark style="background: #FFB8EBA6;">To control the level of regualrisation we want</mark>
To address the balance between reconstruction and regularization, $\beta$-VAE introduces a hyperparameter $\beta$ to scale the KL term:
$$\text{Loss} = L_{\text{recon}} + \beta \cdot L_{\text{KL}}$$

* **$\beta > 1$:** <mark style="background: #FFB8EBA6;">Increases regularization</mark>. This yields highly disentangled and organized latent representations, but results in lower-quality, blurrier reconstructions.
* **$\beta < 1$:** <mark style="background: #FFB8EBA6;">Decreases regularization</mark>. This results in sharper reconstructed images, but produces a less organized, less continuous latent space.

#### Variant 2: InfoVAE (Information Maximizing VAE)
* **Objective:** Designed to overcome the trade-off inherent in $\beta$-VAE, aiming to simultaneously achieve high-quality reconstructions and a well-organized, disentangled latent space.
* **Method:** InfoVAE <mark style="background: #FFB8EBA6;">replaces the standard KL Divergence penalty with Maximum Mean Discrepancy (MMD)</mark>.
* **MMD:** A kernel-based mathematical method used <mark style="background: #FFB8EBA6;">to evaluate whether two sets of samples belong to the same probability distribution</mark>, allowing the model to align the latent space without triggering posterior collapse.