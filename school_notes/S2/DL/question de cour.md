
### Section I : Méthodologie CRISP-DM et Données
1.  **Citer les 6 étapes de la méthodologie CRISP-DM** dans l'ordre chronologique. Laquelle de ces étapes consomme généralement le plus de temps (environ 40 à 60% de la charge globale) ?
2.  **Expliquez la différence entre une donnée qualitative "nominale" et "ordinale"**. Donnez un exemple pour chaque type.
3.  **Pourquoi le pré-traitement des données est-il indispensable ?** Citez 3 tâches courantes effectuées lors de cette phase.
4.  **Quelle est la différence fondamentale entre une technique descriptive (non-supervisée) et une technique prédictive (supervisée)** au regard de la variable "cible" ?

### Section II : Le Perceptron et le MLP (Multi-Layer Perceptron)
5.  **Le Perceptron de Rosenblatt est-il capable de résoudre le problème du "XOR" (OU exclusif) ?** Justifiez votre réponse en expliquant la notion de séparabilité linéaire.
6.  **Faites la correspondance entre le neurone biologique et le neurone artificiel** pour les éléments suivants : Dendrites, Corps cellulaire, Axone, Synapse.
7.  **Citer 3 fonctions d'activation couramment utilisées** dans les réseaux de neurones et précisez pour chacune si elle est adaptée à la classification binaire ou à la régression.
8.  **Calcul de paramètres (Style Question 4 de l'image) :** Combien de poids (weights) et de biais (bias) au total sont ajustés lors de l'apprentissage d'un MLP possédant :
    *   3 neurones en entrée.
    *   Une couche cachée de 5 neurones.
    *   1 neurone en sortie.
    *(Astuce : N'oubliez pas le biais pour chaque neurone des couches cachées et de sortie).*

### Section III : Apprentissage et Optimisation
9.  **Expliquez le principe de la "Rétropropagation de l'erreur" (Backpropagation).** Quel est son rôle vis-à-vis de l'algorithme de descente de gradient ?
10. **Citer 3 conditions d'arrêt possibles** pour un algorithme d'apprentissage par descente de gradient.
11. **Qu'est-ce qu'une "fonction de perte" (Loss function) ?** Donnez un exemple de fonction de perte utilisée pour un problème de classification.
12. **Comment l'ajout d'une contrainte (L1 ou L2) sur les poids** permet-il de lutter contre le "sur-apprentissage" (overfitting) ?

### Section IV : Deep Learning et CNN (Convolutional Neural Networks)
13. **Quelle est la différence majeure entre le Machine Learning "traditionnel" et le Deep Learning** concernant l'étape d'extraction des caractéristiques (*Feature Engineering*) ?
14. **Décrivez les 4 opérations principales appliquées dans un bloc de convolution d'un CNN :**
    *   La convolution (rôle des filtres/noyaux).
    *   L'activation (ReLU).
    *   Le Pooling (Max-pooling).
    *   Le Flattening.
15. **Quel est l'objectif du "Max-Pooling" dans un CNN ?** Citez deux avantages (un lié à la dimensionnalité, l'autre à l'invariance).
16. **Dans un CNN, à quoi sert la couche "Fully Connected"** placée à la fin de l'architecture ?

---

### Quelques conseils pour l'examen :
*   **Les calculs de paramètres :** Entraînez-vous bien à compter les connexions (entrées × neurones) + les biais. C'est une question classique.
*   **Le dilemme du taux d'apprentissage :** Revoyez bien la question 3 de votre image (Petit taux = lenteur/blocage local ; Grand taux = oscillation/divergence). La solution est souvent un taux adaptatif (décroissant).
*   **CRISP-DM :** Retenez bien que c'est une approche cyclique et non linéaire. On peut revenir de l'Evaluation à la Compréhension métier.