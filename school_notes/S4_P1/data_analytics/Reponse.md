### Chapitre I : CRISP-DM et Méthodologie
1. **Cycle itératif :** Car il nécessite des ajustements constants (ex: si la modélisation échoue, on retourne à la préparation). Ch I, p. 10-12.
2. **Outliers :** La régression linéaire est très sensible (le carré des erreurs amplifie l'impact), tandis que les arbres de décision sont plus robustes car basés sur des seuils. Ch I, p. 29.
3. **Imputation :** K-NN est plus précis mais coûteux. Préférable quand les données sont corrélées localement. Ch I, p. 32.
4. **Normalisation :** Pour éviter qu'une variable à grande échelle ne domine la distance (biais). Ch I, p. 40.
5. **Extraction/Sélection :** Sélection = garder un sous-ensemble existant. Extraction = créer de nouvelles variables via transformation (ex: ACP). Ch I, p. 46-49.
6. **Filter :** Évalue la pertinence seule (statistiques univariées). Critères : Gain d'info, Khi-2. Ch I, p. 47-56.
7. **Wrapper :** Il teste des combinaisons de variables avec l'algorithme final (plus précis mais boucle très lente). Ch I, p. 47-48.
8. **ACP :** Réduit les dimensions en projetant sur les axes de variance maximale, éliminant le "bruit" des axes à faible variance. Ch I, p. 49-50.
9. **Binning :** Lisse le bruit par zone. Moyenne = valeur centrale ; Frontières = valeurs extrêmes du groupe. Ch I, p. 35-36.
10. **Rôles :** Engineer = infrastructure, pipelines (ETL) ; Scientist = modélisation, analyse, algorithmes. Ch I, p. 53-54.

### Chapitre II : Règles d'Association
11. **Support/Confiance :** Support = fréquence de l'itemset dans tout le corpus. Confiance = probabilité conditionnelle $P(B|A)$. Ch II, p. 3.
12. **Lift :** Si Lift = 1, A et B sont indépendants (aucune corrélation). Ch II, p. 7.
13. **Apriori :** Si un ensemble est rare, ses sur-ensembles le seront aussi. On ignore donc tout ce qui contient cet item rare. Ch II, p. 10.
14. **Complexité :** Le nombre de combinaisons possibles croît de façon exponentielle ($2^m$). Ch II, p. 8.
15. **Redondance :** Via des mesures comme l'Amélioration (Lift > 1) ou le test de Khi-2. Ch II, p. 7.
16. **Seuils :** Support trop haut = on manque les produits rares mais corrélés. Ch II, p. 5.
17. **Données réelles :** Apriori traite des listes d'items binaires (présence/absence), pas des valeurs numériques continues. Ch II, p. 3.
18. **Génération :** En testant les sous-ensembles et en vérifiant si Confiance $\ge$ MinConf. Ch II, p. 13.
19. **Variantes :** DHP, DIC, Partition, Eclat. Améliorent la vitesse de scan. Ch II, p. 16.
20. **Applications :** Détecter des mouvements suspects (ex: si retrait A, alors achat B). Ch II, p. 2.

### Chapitre III : Text Mining
21. **Normalisation :** Stemming (troncature, rapide) vs Lemmatisation (base lexicale, précis). Lemmatisation est plus précis. Ch III, p. 30-31.
22. **Tokenization :** Langues sans espaces (japonais) ou avec morphologie complexe (arabe). Ch III, p. 19.
23. **Bag of Words :** Perd l'ordre des mots et donc le sens (ex: "chien mord homme" vs "homme mord chien"). Ch III, p. 19.
24. **TF-IDF :** Formule $tf \times \log(N/df)$. Le log diminue le poids des mots trop fréquents (ex: "le", "de"). Ch III, p. 25.
25. **N-grammes :** Capturent la dépendance locale (ex: "ne pas"). Ch III, p. 20.
26. **Stopwords :** Utiles pour le contexte syntaxique (ex: négation dans l'analyse de sentiment). Ch III, p. 29.
27. **Similarité :** Cosinus. Elle ignore la longueur des vecteurs, elle est donc plus robuste que l'Euclidienne. Ch III, p. 37.
28. **Jaccard :** Utile quand on s'intéresse uniquement à la présence/absence d'items (pondération binaire). Ch III, p. 38.
29. **Text Mining :** Les données textes sont non-structurées, bruitées et multidimensionnelles. Ch III, p. 11.
30. **Multidisciplinarité :** Réunit statistiques, linguistique (TAL) et apprentissage automatique. Ch III, p. 6.

### Chapitre IV : Web Mining
31. **Piliers :** Content (le texte), Structure (hyperliens), Usage (logs/clics). Ch IV, p. 4.
32. **Web Content Mining :** Via similarité vectorielle (TF-IDF, cosinus) pour classer les résultats selon la requête. Ch IV, p. 9.
33. **Structure du Web :** Hubs = bons répertoires, Authorities = pages faisant autorité (liées par des hubs). Ch IV, p. 12.
34. **PageRank :** Probabilité qu'un surfeur aléatoire arrive sur une page donnée. Ch IV, p. 12.
35. **Logs :** IP, heure, URL demandée, agent, cookie, referrer. Ch IV, p. 18-19.
36. **Web Analytics vs Mining :** Analytics = stat descriptive (vues) ; Mining = prédiction/patterns (comportement). Ch IV, p. 15.
37. **Clickstream :** Vie privée, traçage, revente de données sans consentement. Ch IV, p. 16-17.
38. **Sessions :** HTTP est stateless, donc il faut reconstruire la session via cookies ou ID. Ch IV, p. 17.
39. **Personnalisation :** En utilisant le profil utilisateur (Usage mining) pour suggérer du contenu (Content mining). Ch IV, p. 4.
40. **Challenges :** Hétérogénéité des formats, nettoyage, synchronisation des sources. Ch IV, p. 21.

### Chapitre V : Topic Modeling
41. **LDA (Overview) :** Génératif : le modèle suppose un processus de création du texte. Ch V, p. 15.
42. **Latent :** Ils ne sont pas étiquetés explicitement, ils émergent des corrélations de mots. Ch V, p. 5.
43. **Apprentissage :** L'utilisateur choisit K. Trop grand = sujets redondants, trop petit = mélange de sujets. Ch V, p. 4.
44. **Dirichlet :** $\alpha$ contrôle la densité des sujets dans les docs, $\beta$ la densité des mots par sujet. Ch V, p. 20.
45. **Gibbs Sampling :** Ré-assigne itérativement le sujet d'un mot en fonction des probabilités actuelles. Ch V, p. 21-22.
46. **Interprétabilité :** En regardant les mots les plus probables pour chaque sujet. Ch V, p. 22.
47. **Evaluation :** Perplexité = mesure de "surprise" du modèle face aux données. Ch V, p. 23.
48. **Cohérence :** Elle mesure la similarité sémantique entre les mots (plus proche de l'intuition humaine). Ch V, p. 23.
49. **LSA vs LDA :** LSA est algébrique (SVD), LDA est probabiliste (Dirichlet/Bayésien). Ch V, p. 6.
50. **Supervision :** Seeded LDA permet d'orienter le modèle vers des thèmes voulus par l'expert. Ch V, p. 24-25.