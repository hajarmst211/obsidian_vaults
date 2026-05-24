#### 1. Définition 
*   **Qu'est-ce que le Topic Modeling ?** C'est une technique statistique permettant d'identifier des groupes de mots similaires (clusters) au sein d'un texte pour découvrir des thèmes cachés.
*   **Concepts clés :** 
    *   C'est une forme de **réduction de dimensionnalité** (représenter un texte dans un espace de "sujets" plutôt que de "mots").
    *   C'est un **apprentissage non supervisé** : le nombre de sujets ($K$) est un hyper paramètre à définir.
    *   Un texte est considéré comme un mélange de plusieurs sujets, chacun ayant un certain poids.

#### 2. Fonctionnement et Applications
*   **Principe :** Modéliser les statistiques du corpus. Chaque document possède une distribution de sujets latents, et chaque sujet est décrit par une distribution de mots.
*   **Schéma :** On part d'une matrice <mark style="background: #BBFABBA6;">documents-termes </mark>(très creuse) pour obtenir deux matrices plus petites : <mark style="background: #BBFABBA6;">une matrice Word-Topic (WTM) et une matrice Topic-Document (TDM).</mark>
*   **Applications :**
    *   **Classification de textes :** Améliorer la classification en utilisant des sujets plutôt que des mots bruts.
    *   **Systèmes de recommandation :** Recommander des articles ayant une structure thématique similaire à ce que l'utilisateur a déjà lu.
    *   **Détection de tendances :** Analyser les publications en ligne.

#### 3. Algorithmes Populaires
*   **LDA (Latent Dirichlet Allocation) :** Fondé sur les modèles  probabilistes.
*   **LSA / LSI (Latent Semantic Analysis) :** Utilise la SVD (Singular Value Decomposition) sur la matrice document-terme. Basé sur l'algèbre linéaire.
*   **NMF (Non-Negative Matrix Factorization) :** Basé sur l'algèbre linéaire.

---

### Section II : Topic Modeling non supervisé (LSA & LDA)

#### Latent Semantic Analysis (LSA)
- Les mots qui apparaissent dans des contextes similaires ont des significations proches.
- Entree: Documents -> SVD->
*   **Avantages :** Capture le sens sémantique, fonctionne sur de grands corpus.
*   **Limites :** Difficulté d'interprétation, sensibilité au choix des termes, gestion limitée de la polysémie.

#### Latent Dirichlet Allocation (LDA)
- LDA découvre les sujets latents à partir d'un corpus de texte en attribuant des distributions de sujets aux documents et des distributions de mots aux sujets, permettant ainsi d'identifier les thèmes principaux présents dans les données textuelles.
*   Modèle probabiliste génératif. Chaque document est un mélange de sujets, chaque sujet est une distribution de mots.
*   **Principes :** Utilise le "Bag of Words" (ignore l'ordre des mots).

#### NMF (Non-Negative Matrix Factorization) :
- **Principe :** Basé sur l'algèbre linéaire, mais avec des contraintes de valeurs non-négatives

### LDAs
*   **Le problème du LDA standard :** Il est purement non supervisé et ne peut pas intégrer les préférences ou annotations de l'utilisateur.
*   **Supervised LDA (sLDA) :** Permet d'inclure des étiquettes (labels) de documents directement dans le processus génératif.
*   **Seeded LDA :** Forme de supervision où l'utilisateur fournit des "mots-graines" (seed words) pour orienter la formation des sujets (Guided Topic-Word Distribution).
---

### Section III : Approche (Clustering + Embedding)
- **Embeddings :** Utilisation de vecteurs de mots pré-entraînés comme **Word2Vec, GloVe, fastText**, ou des modèles de langage profonds comme **BERT** ou **ELMo**.
    
- **Clustering :** Une fois les textes transformés en vecteurs (embeddings), on applique des algorithmes de regroupement comme **k-means (KM)** ou **GMM** (Gaussian Mixture Models).
