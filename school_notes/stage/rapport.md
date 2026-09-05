# Revue de la littérature

Ce chapitre présente une évaluation comparative et rigoureuse des approches algorithmiques explorées pour les trois fonctionnalités clés du projet Tech News Fetcher : le résumé automatique, la modélisation de thèmes et l'analyse de sentiment.

---

## 1. Résumé automatique de texte (Summarization)

L'évaluation de cette fonctionnalité s'appuie sur le jeu de données **ccdv/arxiv-summarization** (HuggingFace), composé de paires d'articles scientifiques et de résumés de la plateforme arXiv.

### 1.1 Approche extractive basée sur les N-grammes (BI-GRAM & 5-gram)

#### Description
Cette méthode s'appuie sur l'extraction statistique hybride de phrases en calculant la probabilité d'apparition d'un mot en fonction du contexte des $N-1$ mots précédents. Elle combine plusieurs indicateurs : méthode du titre, positionnelle, similarité d'agrégation, fréquence et requête TF. L'optimisation des hyperparamètres a été réalisée via une optimisation bayésienne (`scikit-optimize`).

#### Résultats expérimentaux
L'approche BI-GRAM affiche une meilleure préservation de la structure globale du texte par rapport au format 5-gramme.

| Configuration | ROUGE-1 | ROUGE-2 | ROUGE-L | Similarité Cosinus |
| :--- | :---: | :---: | :---: | :---: |
| **BI-GRAM (Optimisé)** | 0,4269 | 0,1845 | 0,2677 | 0,6082 |
| **5-gram (Optimisé)** | 0,4254 | 0,1609 | 0,2199 | 0,5907 |

* **Paramètres BI-GRAM optimaux** : Alpha (normalisation de longueur) = 0,60 ; Pos Weight = 0,00 ; Seuil de redondance = 0,20.
* **Paramètres 5-gram optimaux** : Alpha = 0,90 ; Pos Weight = 1,50 ; Seuil de redondance = 0,20.

#### Références
* Sushir, R. (2023). *Sensitization to bigram calculation in NLP with solved examples*. Medium.
* ResearchGate (2024). *Enhancing Bangla Language Next Word Prediction and Sentence Completion through Extended RNN with Bi-LSTM Model On N-gram Language*.

---

### 1.2 Algorithme génétique (GaSUM)

#### Description
La méthode GaSUM combine l'extraction de caractéristiques par plongements contextuels (BERT) et la sélection évolutive de phrases à l'aide d'un algorithme génétique. Les hyperparamètres ont été réglés par recherche sur grille (Grid Search) et via l'outil d'optimisation Optuna.

#### Résultats expérimentaux
Les performances globales de GaSUM demeurent en deçà des méthodes basées sur les graphes ou les statistiques pures.

| Version | Taille Pop. | Générations | Prob. Crossover | Prob. Mutation | Score de fitness |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **v1 (Grid Search)** | 40 | 10 | 0,6 | 0,2 | **F1 : 0,2396** |
| **v2 (Optuna)** | 25 | - | 0,8 | 0,01 | Moyenne globale : 0,2268 |

#### Référence
* *GaSUM Technique (BERT + GA)*. SciTePress, 2023.

---

### 1.3 Réduction de graphe sémantique (SGR)

#### Description
Cette approche se décompose en trois étapes : construction d'un graphe sémantique riche à partir du document source, réduction de ce graphe vers une structure hautement abstraite par élagage, puis génération du résumé final. L'optimisation a été menée par recherche sur grille.

#### Résultats expérimentaux
L'approche SGR affiche une robustesse marquée sur les métriques composites de cohérence.

| Seuil de similarité | Limite de repli | Taux d'élagage | Score composite |
| :--- | :---: | :---: | :---: |
| **0,125** | **1** | **39,5 %** | **0,8487** |
| 0,125 | 1 | 41,75 % | 0,8487 |
| 0,125 | 1,25 | 39,5 % | 0,8487 |
| 0,0875 | 1 | 39,5 % | 0,8438 |

#### Référence
* *Semantic graph reduction approach for abstractive Text Summarization*. Academia.edu.

---

### 1.4 Optimisation sémantique par graphe dynamique (DG-SGR)

#### Description
Il s'agit d'une extension de la méthode SGR qui intègre des réseaux de neurones sur graphes dynamiques (DGNN) afin de maximiser la cohérence de transition entre les propositions sélectionnées pour le résumé.

#### Résultats expérimentaux

| Variante du pipeline | Cohérence de phrase (normalisée) | Rang de fréquence des synonymes |
| :--- | :---: | :---: |
| **SGR de base** | 0,1701 | 12,2956 |
| **SGR optimisé par DGNN** | **1,0000** | **12,7543** |

#### Référence
* *Dynamic Graph Semantic Optimization for Text Summarization*. ScienceDirect / Egyptian Informatics Journal.

---

### Conclusion et choix de la méthode de résumé
L'analyse comparative montre que si les méthodes basées sur les N-grammes (BI-GRAM) offrent de bons scores ROUGE bruts, elles manquent de cohérence syntaxique globale. L'algorithme GaSUM présente un coût computationnel élevé pour des scores F1 modestes. 

La méthode **DG-SGR** est retenue. Elle combine la structure sémantique de l'approche SGR à une optimisation par graphe dynamique (DGNN), ce qui permet d'atteindre le score de cohérence le plus stable (noté à sa valeur maximale de 1,0 sur l'échelle d'évaluation normalisée) tout en limitant le bruit d'extraction.

---

## 2. Reconnaissance de thèmes (Topic Modeling)

L'évaluation s'appuie sur le jeu de données **Cornell-University/arxiv** (Kaggle), qui contient les métadonnées globales des publications scientifiques d'arXiv.

### 2.1 Latent Dirichlet Allocation (LDA)

#### Description
Le modèle LDA est un algorithme probabiliste génératif non supervisé. Il postule que chaque document est un mélange de plusieurs thèmes latents, et que chaque thème est défini par une distribution de probabilité sur un ensemble de mots. L'optimisation a été menée en évaluant l'évolution du score de cohérence sémantique ($C_V$).

#### Résultats expérimentaux
La configuration à 6 thèmes offre le meilleur compromis de cohérence.

| Nombre de thèmes ($k$) | Score de cohérence ($C_V$) | Statut |
| :--- | :---: | :--- |
| 2 | 0,2186 | Évalué |
| 3 | 0,2371 | Évalué |
| 5 | 0,2239 | Évalué |
| **6** | **0,2445** | **Sélectionné** |
| 7 | 0,2292 | Évalué |

* **Métriques finales du modèle retenu ($k=6$)** : Perplexité = -8,0533 ; Cohérence globale ($C_V$) = 0,2362.

#### Référence
* *Topic Modeling on Online NewsPortal Using Latent Dirichlet Allocation (LDA)*. ResearchGate.

---

### 2.2 TextRank

#### Description
TextRank est un algorithme non supervisé basé sur les graphes, dérivé de PageRank. Il détermine l'importance d'un mot ou d'une phrase en construisant un graphe de cooccurrences et en mesurant récursivement sa centralité.

#### Résultats expérimentaux
L'algorithme se montre peu adapté à l'extraction globale de thèmes sur des documents courts ou des résumés.

| Rang | Taille fenêtre | Facteur d'amortissement ($d$) | Seuil | Longueur max phrase | Top $N$ | F1-Score moyen |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | 5 | 0,85 | 0,25 | 2 | 5 | **0,0080** |
| **2** | 5 | 0,85 | 0,25 | 3 | 5 | 0,0080 |

---

### 2.3 Similarité Cosinus (Cosine Similarity)

#### Description et justification de l'exclusion
La similarité cosinus permet de comparer des vecteurs de documents à des vecteurs de catégories prédéfinies. Bien que cette approche présente de bons scores théoriques de confiance, elle nécessite un ensemble rigide de thèmes étiquetés en amont. Cette contrainte ne correspond pas au cas d'usage du projet, qui requiert une découverte dynamique et non supervisée des sujets de l'actualité technologique.

---

### Conclusion et choix de la méthode de modélisation de thèmes
L'algorithme TextRank s'avère insuffisant pour synthétiser des thématiques globales ($F1 < 0,01$). La similarité cosinus requiert quant à elle un cadre supervisé incompatible avec l'ambition d'automatisation du projet.

Le modèle **LDA ($k=6$)** est retenu. Malgré une complexité d'estimation des distributions a posteriori, il offre la meilleure aptitude à identifier de manière non supervisée des thématiques cohérentes et interprétables.

---

## 3. Analyse de sentiment (Sentiment Analysis)

Cette tâche est évaluée sur un jeu de données extrait de commentaires YouTube (Kaggle).

### 3.1 Classification par apprentissage automatique (Machine Learning)

#### Description
Trois algorithmes d'apprentissage supervisé classique ont été entraînés et comparés : Naive Bayes (MultinomialNB), SVM (LinearSVC) et les arbres de décision (Decision Tree).

#### Résultats expérimentaux
Le classifieur SVM (LinearSVC) surpasse les deux autres modèles en matière de précision et d'équilibre général entre les classes.

| Algorithme | Exactitude (Accuracy) | Précision (macro) | Rappel (macro) | F1-Score (macro) |
| :--- | :---: | :---: | :---: | :---: |
| **SVM (LinearSVC)** | **0,6855** | **0,6877** | **0,6854** | **0,6862** |
| **Naive Bayes** | 0,6450 | 0,6503 | 0,6448 | 0,6438 |
| **Decision Tree** | 0,5198 | 0,5740 | 0,5199 | 0,5194 |

##### Performance par classe (SVM LinearSVC) :
* **Négatif** : Précision = 0,6857 ; Rappel = 0,7006 ; F1 = 0,6931
* **Neutre** : Précision = 0,6281 ; Rappel = 0,6546 ; F1 = 0,6411
* **Positif** : Précision = 0,7493 ; Rappel = 0,7011 ; F1 = 0,7244

---

### 3.2 Méthode basée sur un lexique (SentiConAcron)

#### Description
L'algorithme SentiConAcron est une méthode basée sur des règles et des dictionnaires spécialisés. Elle vise à résoudre les limites des approches lexicales classiques en gérant les acronymes, les émoticônes et les glissements sémantiques contextuels, sans nécessiter d'entraînement préalable.

#### Résultats expérimentaux
La performance brute de cette méthode reste limitée sur ce jeu de données par rapport aux modèles supervisés.

| Algorithme | Exactitude (Accuracy) | Précision | Rappel | F1-Score |
| :--- | :---: | :---: | :---: | :---: |
| **SentiConAcron** | 47,74 % | 49,25 % | 47,85 % | 47,40 % |

#### Référence
* *Textual Sentiment Analysis using Lexicon Based Approaches*. ResearchGate, 2023.

---

### Conclusion et choix de la méthode d'analyse de sentiment
L'approche lexicale (SentiConAcron) souffre d'un manque de flexibilité face au langage familier des commentaires Web (exactitude de 47,74 %). Parmi les méthodes d'apprentissage supervisé, le modèle SVM démontre la meilleure stabilité générale.

Le modèle **SVM (LinearSVC)** est sélectionné pour l'analyse de sentiment, ayant obtenu une exactitude de 0,6855 et des performances de classification équilibrées sur les trois classes de polarité.

---

# Conclusion générale du chapitre

L'évaluation comparative des différentes architectures permet d'arrêter la configuration technique suivante pour le projet Tech News Fetcher :

1. **Résumé automatique** : Intégration du pipeline **DG-SGR** (Réduction de graphe sémantique optimisée par réseau de neurones sur graphe dynamique) afin de garantir des résumés concis et sémantiquement stables.
2. **Reconnaissance de thèmes** : Déploiement de l'algorithme **LDA ($k=6$)**, qui offre une classification thématique non supervisée robuste pour regrouper les flux d'actualités.
3. **Analyse de sentiment** : Utilisation du modèle **SVM (LinearSVC)** pour sa capacité à catégoriser efficacement la polarité des retours utilisateurs et des articles de presse.

Ces choix méthodologiques équilibrent la performance de classification, la cohérence sémantique des synthèses et la viabilité de l'exécution au sein de l'application.