### Section I : Méthodologie CRISP-DM et Données

1.  **Les 6 étapes de CRISP-DM :**
    *   1. Compréhension métier (Business Understanding)
    *   2. Compréhension des données (Data Understanding)
    *   3. Préparation des données (Data Preparation)
    *   4. Modélisation (Modeling)
    *   5. Évaluation (Evaluation)
    *   6. Déploiement (Deployment)
    *   **L'étape la plus longue :** La **Préparation des données** (elle représente souvent entre 40 et 60% de la charge globale du projet).

2.  **Qualitative Nominale vs Ordinale :**
    *   **Nominale :** Les valeurs n'ont pas d'ordre logique. *Exemple : La couleur des yeux (Bleu, Vert, Marron).*
    *   **Ordinale :** Les valeurs suivent un ordre ou une hiérarchie. *Exemple : Niveau de satisfaction (Faible, Moyen, Fort).*

3.  **Indispensable car :** Les données réelles sont souvent "bruitées" (erreurs), incomplètes (valeurs manquantes) ou incohérentes. Des données de mauvaise qualité mènent à des modèles peu fiables.
    *   **3 tâches :** Nettoyage (imputation), Transformation (normalisation), Réduction de dimension (agrégation).

4.  **Différence fondamentale :**
    *   **Technique descriptive :** Il n'y a **pas de variable cible**. On cherche à structurer ou résumer les données (ex: segmentation/clustering).
    *   **Technique prédictive :** Il y a une **variable cible** ($Y$) que l'on cherche à expliquer ou prédire à partir des entrées ($X$).

---

### Section II : Le Perceptron et le MLP

5.  **Problème du XOR :** **Non**, le Perceptron simple de Rosenblatt ne peut pas résoudre le XOR.
    *   **Justification :** Le Perceptron est un séparateur **linéaire** (il trace une droite). Or, les points du XOR ne sont pas **linéairement séparables** (on ne peut pas les séparer par une seule ligne droite).

6.  **Correspondance neurone biologique/artificiel :**
    *   Dendrites = **Entrées** (Signaux/Variables $x_i$).
    *   Corps cellulaire = **Sommation** ($\sum$) + **Fonction d'activation**.
    *   Axone = **Sortie** (Signal transmis).
    *   Synapse = **Poids** (Coefficients synaptiques $w_i$).

7.  **3 Fonctions d'activation :**
    *   **Sigmoïde :** Adaptée pour la **classification binaire** (sortie entre 0 et 1).
    *   **ReLU :** Très utilisée dans les **couches cachées** des réseaux profonds.
    *   **Linéaire :** Adaptée pour la **régression** (prédire une valeur numérique continue).

8.  **Calcul de paramètres (3 entrées $\rightarrow$ 5 cachés $\rightarrow$ 1 sortie) :**
    *   **Poids (Weights) :** $(3 \text{ entrées} \times 5 \text{ neurones}) + (5 \text{ cachés} \times 1 \text{ neurone}) = 15 + 5 = \mathbf{20}$
    *   **Biais (Bias) :** $5 \text{ (couche cachée)} + 1 \text{ (couche sortie)} = \mathbf{6}$.
    *   **Total :** $20 + 6 = \mathbf{26}$ paramètres à ajuster.

---

### Section III : Apprentissage et Optimisation

9.  **Rétropropagation :** C'est un algorithme qui calcule l'erreur à la sortie et la propage vers l'arrière du réseau pour évaluer la contribution de chaque poids à cette erreur.
    *   **Rôle :** Elle sert à calculer le **gradient** (la direction de correction) nécessaire à l'algorithme de descente de gradient pour mettre à jour les poids.

10. **3 conditions d'arrêt :**
    *   Atteindre un nombre fixe d'itérations (époques).
    *   Stabilisation des poids (ils ne changent plus beaucoup).
    *   L'erreur sur l'échantillon de validation commence à augmenter (signe de sur-apprentissage).

11. **Fonction de perte :** Mesure l'écart entre la prédiction du modèle et la valeur réelle attendue.
    *   **Exemple classification :** L'Entropie croisée binaire (Binary Cross-Entropy).

12. **Contrainte L1 ou L2 :** Elles ajoutent une "pénalité" à la fonction de perte basée sur la taille des poids. Cela force le modèle à garder des poids petits et simples, évitant ainsi qu'il n'apprenne par cœur le "bruit" des données d'entraînement (sur-apprentissage).

---

### Section IV : Deep Learning et CNN

13. **Différence majeure :**
    *   **ML traditionnel :** L'extraction des caractéristiques (*features*) est faite **manuellement** par l'humain.
    *   **Deep Learning :** L'extraction des caractéristiques est **automatique** (le réseau apprend lui-même les filtres pertinents).

14. **Les 4 opérations du CNN :**
    *   **Convolution :** Utilise des filtres pour détecter des motifs (bords, textures).
    *   **Activation (ReLU) :** Remplace les valeurs négatives par 0 pour introduire de la non-linéarité.
    *   **Pooling :** Réduit la taille des données tout en gardant l'information la plus importante.
    *   **Flattening :** Convertit les matrices 2D en un long vecteur 1D pour la classification finale.

15. **Objectif du Max-Pooling :** Réduire la dimension spatiale des cartes de caractéristiques.
    *   **Avantage 1 (Dimension) :** Réduit le nombre de paramètres et les calculs (prévention de l'overfitting).
    *   **Avantage 2 (Invariance) :** Rend le réseau invariant aux petites translations de l'objet dans l'image.

16. **Couche Fully Connected :** Elle se situe à la fin du CNN pour effectuer la **classification finale**. Elle combine toutes les caractéristiques locales extraites par les couches précédentes pour prédire la probabilité de chaque classe.