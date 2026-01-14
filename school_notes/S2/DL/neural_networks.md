#### RNA: Réseaux neuron artificielle
- **def:** a collection of connected units or nodes called **artificial neurons**

---
# Perceptron:
- A network with only **two layers** (input and output) that are entirely interconnected
- A linear classifier that calculates a weighted sum of inputs (`∑wixi`) and applies a threshold or **activation function** (specifically the Heaviside function) to produce an output.(slide 30-31)
###### Limitation:
It is only capable of separating classes that are **linearly separable**  and is incapable of solving non-linear patterns like the XOR problem

### Structure  of a RNA/NLP(multi layers perceptron):
The structure of an RNA/MLP (Multi-Layer Perceptron) is organized into **Layers** (Slides 21, 56):

- **Input Layer (Couche d'entrée):** Corresponds to the input variables/descriptors (Slide 56).
- **Hidden Layers (Couches cachées/intermédiaires):** One or more layers located between the input and output where the processing happens (Slide 56).
- **Output Layer (Couche de sortie):** Provides the final result of the network (Slide 56).
    
#### Inside each neuron (PE), the components are :
- **Inputs (**`xi​`**):** Received from the "outside world" or previous layers.
- **Synaptic Weights (**`wiwi​`**):** Values assigned to each connection.
- **Bias/Threshold (**`θ` or`b`):** A constant used to shift the activation.
- **Summing Function (**`∑∑`**):** Calculates the total input.
- **Activation/Transfer Function:** (e.g., Sigmoid, ReLU, or Step function) which determines the final output signal (Slides 31, 61).

### Threshold vs bias model:
The comparaison is explained by the Heaviside activation function because it is simpler
#### The threshold model:
- In this model, the neuron calculates the weighted sum of its inputs and only "fires" if that sum surpasses a specific value called the **threshold (**`θ`**)**
- In the math we compare the weighted sum with `θ`, if > the neurone fires if not it doesn't
#### The extra entry model(The bias `x0`)
-  The simple reason for this model is that we want everything to be a "weight." To do this, we add a "fake" input to the neuron, usually called `x0` , which is **permanently set to 1**. We then assign a weight to this entry (`w0​`), which we call the **Bias**.
- Here we compare our weighted sum with 0 .


### Activation function:
- *def:*  
	The activation function is the mathematical function applied to the total input (the weighted sum) to produce the final output
 - **The Math:** 
	 First, the neuron calculates the "total input" (`∑wixi`). Then, it applies the activation function`f(x)`to this sum (Slide 31).

- **Purpose:**
    - It determines if the neuron "fires" or not.
    - It introduces **non-linearity** into the network, which allows it to solve complex problems that are not linearly separable (Slide 54).
    - It filters or scales the signal (e.g., removing negative values or squashing values into a range like 0 to 1).
- All the PE have the same activation function
- ALL PEs of the same layer are updated at the same time.
#### Examples:
<mark style="background: #ADCCFFA6;">x represents the Weighted Sum (plus Bias)</mark>
- **Heaviside (Step):** Output is 1 if input $> 0$, else 0.
**Linear Function (**`u=x`**):** Used for <mark style="background: #FFF3A3A6;">regression</mark> tasks.
     There is no transformation of the data
 - **Sigmoid:** $1 / (1 + e^{-x})$. Outputs values between 0 and 1. Used for <mark style="background: #FFF3A3A6;">classification.</mark>
- **ReLU:** $\max(0, x)$. Filters out negative values.
- **Tanh(tangente hyperbolique):** Outputs values between -1 and 1. Alternative to Sigmoid in classification problems.
#### Limitations of the Heaviside function:
- **The Problem:** 
	The Step Function is flat everywhere (slope = 0) and has a sudden "jump" at zero.
- **The Result:** 
	If you change a weight slightly, the output stays exactly the same (either still 0 or still 1). The computer gets no "feedback" on whether it’s getting closer to the answer. It’s like trying to find the bottom of a dark room where the floor is perfectly level but has random, vertical cliffs.

### Transfer function:
- **Def:**
	It is the mathematical formula that defines how a neuron transforms its input signals into an output signal

---
### Law of hebb
- **Principal:**
	Iterative process to <mark style="background: #FFF3A3A6;">adjust the weights </mark>of a perceptron so that it eventually learns to classify data correctly. The algorithm stops only when all examples have been presented without <mark style="background: #FFF3A3A6;">no modification </mark>of the weights.
	- we give the<mark style="background: #FFB86CA6;"> initial weights</mark>, preferably different

- **The math:** 
	For each input`xi​`, the weight`wi​` is updated using the following formula: `wi→wi+(c−o)xi​`
Where:
- `wi​`: The weight of the connection.
- `c`: The desired output (target).
- `o`: The output calculated by the perceptron.
- `xi`​ The value of the input.

- **When the modification occurs:**
If the perceptron is:
- <span style="color:rgb(63, 183, 222)"> correct (o = c): </span>The weights are not modified.
- <span style="color:rgb(63, 183, 222)"> under-responding (o = 0 but c = 1):</span> This means the perceptron didn't take the active inputs into account enough. The rule *adds* the input values to the weights: $w_i \rightarrow w_i + x_i$.
- <span style="color:rgb(63, 183, 222)"> over-responding (o = 1 but c = 0):</span> This means the perceptron was too sensitive to the inputs. The rule *subtracts* the input values from the weights: $w_i \rightarrow w_i - x_i$.
###### Limitations:
- If the data is not linearly separable, the law of hebb algorithm never converges and we can't know that. 
**Solution:** 
	1. tolerate a bit of errors
	2. Try to classify the MAXIMUM of points only.

#### Exercice:
page 20 slide 40

---
### Widrow-Hoff algorithm
>Widrow-Hoff cares about **how far off** the prediction is not if it is right or wrong.

- **Principal:**
	1. Initialize weights $w_i$ **randomly**
	2. Take an example $(x, c)$ from your training set.
	3. Calculate the output $o$ (this is often the linear sum before the final hard threshold).
	4. Update the weights using this formula: $w_i \rightarrow w_i + \epsilon(c - o)x_i$.
		Note: $\epsilon$ is the learning rate, a small value typically between 0 and 1.
- **stopping rule:**
 After a complete pass through the entire training set (an epoch), the algorithm stops if **all weight modifications are below a predefined threshold**: you stop when the weights are no longer changing significantly.
 
---
# PMC: les réseaux multi couches
### Fonction d'activation:
- The PMC uses a combination of linear separators.
- You might use a **Sigmoid** in the hidden layers to handle non-linearity. 
- But you might use a **Linear** function in the output layer if you are trying to predict a continuous price or value rather than a category.
### Retro propagation de l’erreur
- **Def:** This is the fundamental process by which a Multi-Layer Perceptron (PMC) learns.
*   **Mechanism:** It uses the **Gradient Descent** algorithm to minimize the difference (Error $E$) between the network's outputs and the desired targets.

#### Process:
- **A. La Passe Avant (Forward Pass) :**
    1. On injecte les données en entrée.    
    2. Le signal traverse le réseau (multiplication par les poids + fonctions d'activation).       
    3. On obtient une sortie calculée ($a_k$).
    4. On calcule l'erreur totale du réseau par rapport à la sortie désirée ($d_k$).
        
- **B. La Passe Arrière (Backward Pass / Rétropropagation) :**
    1. On calcule l'erreur des neurones de la **couche de sortie**.
    2. On "rétropropage" cette erreur vers les neurones de la **couche cachée** en utilisant les poids des connexions.    
    3. On ajuste les poids de toutes les couches pour réduire cette erreur.

#### The Objective 
The course summarizes the goal of this title as:
*   Reducing the error $E$ of the network.
*   The error formula provided is: $E(w) = \frac{1}{2} \sum \sum (y_i - g_i(in))^2$ (the sum of squared differences between target $y$ and obtained output $g$).

#### Important Stopping Conditions (Slide 74)
Because back propagation is iterative, the several ways to know when to stop:
*   After a fixed number of iterations.
*   When weights stabilize.
*   When the error on a **validation set** falls below a certain limit (to prevent overfitting).
---
**Fonction de perte :** Mesure l'écart entre la prédiction du modèle et la valeur réelle attendue.
    *   **Exemple classification :** L'Entropie croisée binaire (Binary Cross-Entropy).