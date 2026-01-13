

### 1. Différences entre Descente de Gradient (DG) et Descente de Gradient Stochastique (DGS)

*   **La fréquence des mises à jour :** Dans la **DG**, les poids ne sont mis à jour qu'une seule fois après avoir parcouru l'ensemble des données d'apprentissage (le "batch"). Dans la **DGS**, les poids sont mis à jour après chaque exemple individuel passé dans le réseau.
*   **La trajectoire de convergence :** La **DG** suit une trajectoire fluide et directe vers le minimum de la fonction de perte. La **DGS** a une trajectoire "bruyante" et oscillante, ce qui est plus rapide mais moins stable, bien que cela puisse aider à sortir des minima locaux.

### 2. Paramètres et Hyperparamètres d'un MLP

*   **Paramètres (appris par le modèle) :** Les **poids** (weights) et les **biais** (biases).
*   **Hyperparamètres (fixés par l'utilisateur) :** Le **taux d'apprentissage**, le nombre de **couches cachées**, le nombre de **neurones par couche**, les **fonctions d'activation** (ReLU, Sigmoïde, etc.), la taille des lots (batch size) et le nombre d'époques.

### 3. Le Taux d'Apprentissage (Learning Rate)

*   **Définition :** C'est un scalaire qui détermine la taille du "pas" effectué à chaque itération de la descente de gradient pour minimiser l'erreur.
*   **Petit taux :** L'apprentissage est très lent et risque de rester bloqué dans un minimum local.
*   **Grand taux :** L'apprentissage peut être instable, "sauter" par-dessus le minimum global, et finir par diverger sans jamais converger vers une solution.
*   **Remède au dilemme :** On peut utiliser des **taux d'apprentissage adaptatifs** (comme avec les optimiseurs Adam ou RMSprop) ou un **décroissance programmée du taux** (learning rate decay) qui réduit le pas au fur et à mesure que l'on s'approche de la solution.

### 4. Calcul du nombre de paramètres

On compte les poids (connexions) et les biais (1 par neurone de destination) :

1.  **Entrée (4) $\rightarrow$ Couche Cachée (2) :**
    *   Poids : $4 \times 2 = 8$
    *   Biais : $2$
    *   Sous-total : **10**
2.  **Couche Cachée (2) $\rightarrow$ Sortie (3) :**
    *   Poids : $2 \times 3 = 6$
    *   Biais : $3$
    *   Sous-total : **9**

**Total :** $10 + 9 = \mathbf{19}$ **paramètres ajustés.**

### 5. La Régularisation

*   **Définition :** C'est une technique qui consiste à ajouter un terme de pénalité à la fonction de perte (comme L1 ou L2) basé sur la magnitude des poids du réseau.
*   **Raisons d'utilisation :** Elle est principalement utilisée pour éviter le **sur-apprentissage (overfitting)**. En pénalisant les poids trop élevés, on force le modèle à rester "simple", ce qui lui permet de mieux généraliser ses prédictions sur de nouvelles données qu'il n'a jamais vues.