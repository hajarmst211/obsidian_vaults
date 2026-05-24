
### Chapitre I : CRISP-DM et Méthodologie
1.  **Cycle itératif :** Pourquoi le modèle CRISP-DM est-il considéré comme un processus dynamique plutôt que séquentiel ? Donnez un exemple de retour en arrière fréquent entre deux étapes.
2.  **Qualité des données :** Comment la présence de données aberrantes (outliers) peut-elle fausser les résultats d'une régression linéaire simple par rapport à un arbre de décision ?
3.  **Nettoyage :** Comparez l'imputation par la moyenne et l'imputation par K-NN (K-plus proches voisins). Dans quel scénario l'imputation par K-NN est-elle préférable malgré sa complexité ?
4.  **Transformation :** Pourquoi est-il indispensable de normaliser les variables (Scaling) avant d'appliquer un algorithme basé sur les distances, comme le K-Means ?
5.  **Réduction de dimension :** Expliquez la différence entre *Feature Selection* (sélection de variables) et *Feature Extraction* (extraction de caractéristiques).
6.  **Sélection :** Décrivez le fonctionnement de la méthode *Filter*. Quels sont les critères statistiques (ex: Gain d'information, Khi-2) utilisés pour juger de la pertinence d'une variable ?
7.  **Wrapper :** Pourquoi les méthodes *Wrapper* (comme le Forward Selection) sont-elles généralement plus performantes mais beaucoup plus coûteuses en temps de calcul que les méthodes *Filter* ?
8.  **ACP :** En quoi l'Analyse en Composantes Principales (ACP) permet-elle de réduire le bruit tout en conservant l'information essentielle du jeu de données ?
9.  **Gestion du bruit :** Comment le lissage par "Binning" (mise en bacs) peut-il aider à corriger des données bruitées ? Expliquez la différence entre le lissage par les moyennes et le lissage par les frontières.
10. **Data Engineering :** Quel est le rôle d'un ingénieur de données (*Data Engineer*) dans un projet d'analyse comparativement à celui d'un *Data Scientist* ?

---

### Chapitre II : Règles d'Association
11. **Support vs Confiance :** Quelle est la différence conceptuelle fondamentale entre le *Support* et la *Confiance* dans une règle d'association $A \Rightarrow B$ ?
12. **Lift (Amélioration) :** Si le *Lift* d'une règle est égal à 1, que pouvez-vous conclure sur la relation entre les items A et B ?
13. **Apriori :** Expliquez la propriété "Apriori" (tous les sous-ensembles d'un itemset fréquent sont fréquents). Comment cette propriété permet-elle d'élaguer l'espace de recherche ?
14. **Complexité :** Pourquoi l'algorithme Apriori devient-il inefficace lorsque le nombre d'items différents ($m$) augmente exponentiellement ?
15. **Redondance :** Comment identifier et éliminer des règles d'association "triviales" (ex: règles qui ne sont pas statistiquement significatives) ?
16. **Seuils :** Comment le choix d'un support minimum trop élevé impacte-t-il la découverte de règles intéressantes sur des produits de niche ?
17. **Données réelles :** Pourquoi l'algorithme Apriori est-il plus adapté à des données transactionnelles (panier) qu'à des données continues ?
18. **Génération de règles :** Une fois les itemsets fréquents extraits, comment génère-t-on les règles de confiance ?
19. **Variantes :** Citez deux variantes de l'algorithme Apriori et expliquez brièvement en quoi elles améliorent les performances.
20. **Applications :** Donnez un exemple d'application des règles d'association dans le secteur bancaire (détection de fraude, par exemple).

---

### Chapitre III : Text Mining
21. **Normalisation :** Expliquez la différence entre *Stemming* et *Lemmatisation*. Lequel est le plus précis linguistiquement ?
22. **Tokenization :** Quels sont les défis de la tokenization pour des langues comme l'arabe ou le japonais comparé à l'anglais ?
23. **Bag of Words :** Pourquoi la représentation "Bag of Words" (sac de mots) perd-elle des informations sémantiques importantes ?
24. **TF-IDF :** Formulez mathématiquement le TF-IDF et expliquez pourquoi le terme "IDF" (Inverse Document Frequency) pénalise les mots très fréquents dans tout le corpus.
25. **N-grammes :** Comment l'utilisation de bi-grammes peut-elle aider à capturer des expressions comme "machine learning" qui perdraient leur sens avec un modèle unigramme ?
26. **Stopwords :** Pourquoi est-il déconseillé de supprimer les mots vides (stopwords) dans certaines tâches comme l'analyse de sentiments ou la reconnaissance d'entités nommées ?
27. **Similarité :** Comparez la distance Euclidienne et la similarité Cosinus pour comparer deux documents textuels. Laquelle est la plus robuste aux variations de longueur de document ?
28. **Distance de Jaccard :** Dans quel cas l'indice de Jaccard est-il plus pertinent que la similarité Cosinus ?
29. **Text Mining vs Data Mining :** Pourquoi les algorithmes de Data Mining classiques ont-ils du mal à traiter les données textuelles brutes ?
30. **Multidisciplinarité :** En quoi le Text Mining est-il à l'intersection entre les statistiques, le Web Mining et le traitement automatique du langage (TAL/NLP) ?

---

### Chapitre IV : Web Mining
31. **Typologie :** Définissez les trois piliers du Web Mining : *Content*, *Structure* et *Usage*.
32. **Web Content Mining :** Comment le moteur de recherche Google utilise-t-il le contenu textuel des pages pour classer les résultats ?
33. **Structure du Web :** Expliquez la notion de Hubs et d'Authorities dans l'algorithme HITS. Comment cette structure en graphe est-elle exploitée pour la découverte de communautés ?
34. **PageRank :** Expliquez l'intuition derrière le modèle du "random surfer" (surfeur aléatoire) dans l'algorithme PageRank.
35. **Logs Serveurs :** Quelles informations cruciales pouvez-vous extraire d'un log serveur pour mieux comprendre le comportement des utilisateurs ?
36. **Web Analytics vs Web Mining :** Quelle est la différence entre une analyse descriptive (Google Analytics) et le Web Usage Mining ?
37. **Clickstream :** Quelles sont les problématiques de protection de la vie privée liées à la collecte des données *clickstream* via des cookies ?
38. **Sessions :** Pourquoi est-il difficile de définir une "session utilisateur" dans un environnement HTTP sans état (stateless) ?
39. **Personnalisation :** Comment le Web Mining permet-il de servir des publicités ciblées dynamiquement aux utilisateurs ?
40. **Challenges :** Quels sont les principaux défis techniques liés à l'intégration de données provenant de sources disparates (logs serveurs, réseaux sociaux, bases e-commerce) ?

---

### Chapitre V : Topic Modeling
41. **LDA (Overview) :** Pourquoi LDA est-il considéré comme un modèle génératif ? Que signifie "modéliser les statistiques du corpus" ?
42. **Latent :** Pourquoi les sujets (topics) extraits par LDA sont-ils qualifiés de "latents" ou "cachés" ?
43. **Apprentissage :** LDA est un apprentissage non supervisé. Comment l'utilisateur influence-t-il le modèle, et quel est le risque de choisir un $K$ (nombre de sujets) inadapté ?
44. **Distribution de Dirichlet :** Expliquez intuitivement le rôle des paramètres $\alpha$ (alpha) et $\beta$ (beta). Que se passe-t-il si $\alpha$ est très faible ?
45. **Gibbs Sampling :** Expliquez le processus itératif du *Gibbs Sampling* dans l'inférence de LDA. Comment converge-t-il vers une solution ?
46. **Interprétabilité :** Comment passe-t-on de la matrice *Topic-Word* à l'interprétation humaine des sujets ?
47. **Evaluation :** Qu'est-ce que la "Perplexité" et pourquoi est-elle une mesure importante pour comparer deux modèles LDA ?
48. **Cohérence :** Pourquoi la cohérence thématique (*Topic Coherence*) est-elle souvent jugée plus proche du ressenti humain que la perplexité ?
49. **LSA :** Quelle est la différence majeure dans l'approche mathématique entre LSA (basé sur la SVD) et LDA (basé sur les probabilités) ?
50. **Supervision :** Présentez le concept de *Seeded LDA*. Pourquoi intégrer des mots-graines (seeds) peut-il rendre le modèle plus utile pour un expert métier ?