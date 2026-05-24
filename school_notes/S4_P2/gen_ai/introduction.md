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
- When performing discriminative modeling, each observation in the training data has a label
- Discriminative modeling estimates p(y|x), which aims to model the probability of a label y given some observation x
#### Generative:
- Doesn’t require the dataset to be labeled because it concerns itself with generating entirely new images.
- Input variables: vectors of numbers that aren’t related to real-world values at all and are often even randomly generated
- Generative modeling estimates p(x), which aims to model the probability ofobserving an observation x. Sampling from this distribution allows us to generate new observations.

### Density estimation models:
### **1. Explicit Density Estimation Models**
These models focus on defining the mathematical structure of the data's probability distribution.

*   **Core Idea:** The model explicitly defines and learns the actual probability distribution of the data.
*   **Analogy:** It is like having a **detailed statistical blueprint** or a **topographic map** of the data. You know exactly where the "peaks" and "valleys" of the data are.
*   **Key Feature:** You have the ability to **calculate the likelihood** of any specific data point ($x$). This means you can plug a piece of data into the model and get a number representing how likely that data is to exist within the learned distribution.
*   **Mathematical Goal:** To learn $p_{\theta}(x)$ (the probability of $x$ given the model parameters $\theta$).
*   **Examples:**
    *   **Variational Autoencoders (VAEs):** These use an encoder-decoder structure to approximate the distribution.
    *   **Autoregressive Models:** Such as **GPT**, which predict the next piece of data (like a word) based on previous pieces, thereby building an explicit probability chain.

---

### **2. Implicit Models**
These models focus on the ability to create new data that looks like the original data, without needing a mathematical formula for the distribution itself.

*   **Core Idea:** The model learns a **process or mechanism** to draw new samples from the distribution ($p_{\theta}(x)$) without ever knowing the actual mathematical formula of that distribution.
*   **Analogy:** It is compared to a **master artisan or an art forger**. This person can produce incredibly realistic artifacts (data) by following a craft, even if they cannot explain the underlying mathematical rules or physics of the art they are creating.
*   **Key Feature:** The model is limited to **generating samples**. Unlike explicit models, you **cannot calculate the likelihood** of a specific data point ($x$). You can make more data, but you can’t mathematically score how "likely" a specific input is.
*   **Mathematical Goal:** To learn a sampler $G_{\theta}(z)$. 
    *   The process involves taking a random noise vector $z$ from a simple distribution ($z \sim p(z)$) and passing it through the generator function ($G_{\theta}(z)$) to produce a realistic data point $x$.
*   **Examples:**
    *   **Generative Adversarial Networks (GANs):** These use a "generator" and a "discriminator" competing against each other to create realistic data.
    *   **Diffusion Models:** These learn to generate data by reversing a process of adding noise to images.
