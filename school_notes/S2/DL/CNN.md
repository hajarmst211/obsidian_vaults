# what is deep learning?
Neural networks need non lineare functions 
- **Successive Layers:** It consists of neural networks with many layers (often dozens or hundreds). It is not "deep" in the sense of human-like understanding, but refers to the successive layers of representations
    
- **No Feature Engineering:** Unlike traditional Machine Learning, where features must be manually defined, Deep Learning learns features automatically 
    
- **Flexibility:** It is highly flexible and excels at processing unstructured data 
    
- **Why now?** Its recent success (since 2010) is due to three forces: **Hardware** (Power), **Datasets/Benchmarks**, and **Algorithmic advances** (Slide 21).
---
# CNN (Convolutional Neural Network)
- **def:** It is a specific type of algorithm particularly used to **classify images**

### CNN layers:
1. **Convolutional Layer:** This is the fundamental operation. It uses **filters (or kernels)**—small matrices of weights

2. **Activation Layer (ReLU):** It applies a non-linear function (Rectified Linear Unit) to allow the network to learn complex relationships and non-trivial representations.
    
3. **Pooling Layer:** This reduces the spatial dimensions of the feature maps to decrease the number of parameters and prevent overfitting, while preserving the most important information.
    
4. **Fully Connected Layer:** Toward the end of the network, the extracted features are "flattened" and passed into a traditional neural network to predict the final probability of the image belonging to a specific class (e.g., "Dog" or "Cat").


---
### 1. Convolution (The Extraction)
*   **Purpose:** To extract significant features (edges, textures, motifs) from an image.
*   **How it works:** A **Filter (or Kernel)** moves across the image. At each position, it performs a mathematical operation (multiplied pixels are added together).
*   **Key Results:** 
    *   It creates a **Feature Map** (a map showing where specific features are located).
    *   It uses **Stride** (the step size of the filter) and **Padding** (adding pixels at the edges) to manage the size of the result.
    *   It builds a hierarchy: low-level layers detect edges, while higher layers detect complex shapes (Slide 39).

### 2. ReLU Activation (The Non-Linearity)
*   **Purpose:** To introduce **non-linearity** into the network.
*   **How it works:** It transforms all negative values to **0** and keeps all positive values as they are.
*   **Why it is essential:** Real-world data is non-linear. ReLU allows the network to learn complex relationships and "non-trivial" representations, which improves the network's ability to generalize to new data (Slide 44).

### 3. Pooling (The Reduction)
*   **Purpose:** To reduce the spatial dimensions (size) of the feature maps.
*   **How it works:** Generally using **Max-pooling**, which only keeps the maximum value from a specific "slice" of the image.
*   **Key Benefits (Slide 48):**
    *   **Reduces parameters:** Decreases the computational load.
    *   **Prevents Overfitting:** By reducing the amount of data the network has to memorize.
    *   **Translation Invariance:** It allows the network to detect a feature even if it has moved slightly in the image.

### 4. Flattening (The Transformation)
*   **Purpose:** To prepare the data for the final classification step.
*   **How it works:** It takes all the values from the final 2D matrices (the pooled feature maps) and **stacks them** into a single, long 1D column (vector).
*   **Goal:** This vector is then fed into the input layer of a standard "Fully Connected" neural network to make the final prediction (Slide 50).


