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
##### 1. La Couche d'Entrée (*Input Layer*)
*   **Ce qu'elle fait :** Elle reçoit l'image brute. 
*   **Détail :** On y définit les dimensions de l'image (largeur, hauteur, et nombre de canaux, ex: 3 pour une image couleur RGB).

##### 2. La Couche de Convolution (*Convolutional Layer*)
*   **Ce qu'elle fait :** C'est le cœur du CNN. Elle sert à **extraire des caractéristiques** (*features*) de l'image.
*   **Comment :** Elle utilise des **filtres** (ou noyaux) qui glissent sur l'image pour détecter des motifs spécifiques : des bords, des textures, ou des formes plus complexes (Slide 39).
*   **Résultat :** Elle produit des **Feature Maps** (cartes de caractéristiques).

##### 3. La Couche d'Activation (généralement **ReLU**)
*   **Ce qu'elle fait :** Elle introduit de la **non-linéarité** dans le réseau.
*   **Comment :** La fonction ReLU remplace toutes les valeurs négatives par des zéros et conserve les valeurs positives (Slide 42).
*   **Utilité :** Cela permet au réseau d'apprendre des relations complexes et des représentations non triviales (Slide 44).

##### 4. La Couche de Pooling (souvent Max-Pooling)
*   **Ce qu'elle fait :** Elle réduit la **dimension spatiale** (la taille) des Feature Maps.
*   **Avantages (Slide 48) :**
    *   Réduit le nombre de paramètres (donc moins de calculs).
    *   Aide à prévenir le **sur-apprentissage** (*overfitting*).
    *   Rend le réseau invariant aux petites translations (si l'objet bouge un peu, il est toujours détecté).

##### 5. La Couche de Flattening (Mise à plat)
*   **Ce qu'elle fait :** Elle transforme les matrices (2D/3D) issues des couches précédentes en un **vecteur unique (1D)** (Slide 50).
*   **Utilité :** C'est une étape de transition nécessaire pour pouvoir injecter les données dans un réseau de neurones classique (MLP) à la fin.

##### 6. La Couche Entièrement Connectée (Fully Connected Layer / Dense)
*   **Ce qu'elle fait :** Elle effectue la **classification** finale.
*   **Comment :** C'est un MLP classique où chaque neurone est connecté à toutes les entrées du vecteur de flattening (Slide 51). Elle combine les caractéristiques extraites par les couches de convolution pour prendre une décision globale.

##### 7. La Couche de Sortie (Output Layer)
*   **Ce qu'elle fait :** Elle donne le résultat de la prédiction.
*   **Détail :** Le nombre de neurones correspond au nombre de classes à prédire (ex: 2 neurones pour "Chien" ou "Chat"). Elle utilise souvent une fonction finale comme **Softmax** pour donner des probabilités (ex: 90% de chances que ce soit un chien).

---

**Résumé du flux (Slide 53) :**
`Image` -> `Convolution + ReLU` -> `Pooling` -> `Flattening` -> `Fully Connected` -> `Prédiction (Sortie)`


---

- **Feature Maps** are used to **predict** (Forward).
- **The Prediction** is used to **calculate Error** (at the Output Layer).
- **The Error** is used to **adjust the Filters** that create the Feature Maps (Backward).


