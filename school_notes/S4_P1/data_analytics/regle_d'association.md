# definition:
- Technique de data mining, utilisée pour découvrir des relations cachées et intéressantes entre des variables au sein  bases de données.
- Le concept de base : "Si -> Alors"

# les indices:
#### Support
- <mark style="background: #FFB8EBA6;">Le support: </mark>la fréquence d'apparition du consequent et de antecedent en meme temps
- **Si élevé:** Selection élevée ==> peu d'item set + très robuste statiquement  (ex: 20% des clients).
- **Si réduit:** Vous cherchez des relations plus rares
- on ignore les **combinaisons d'articles** qui représentent moins que le support
- car une combinaison qui n'a pas a support de 5% n'est pas fréquente.   
#### confiance
- <mark style="background: #FFB8EBA6;">La confiance:</mark> Indique la probabilité que le "conséquent" soit présent sachant que "l'antécédent" l'est aussi
- **Élevé:** Vous ne gardez que les règles "quasi-certaines". très "sévère" sur la validité de la règle
- **Réduite:** La règle n'est pas fiable. On accepte des relations plus floues
- Si une confiance d'une combinaison est inférieur au seuil `γ` (ex: 80%), la règle est rejetée car elle n'est pas assez "sûre"

#### Amelioration (lift):
- Parfois, une règle semble avoir une bonne "Confiance", mais en réalité, le produit résultant est **tellement populaire** que tout le monde l'achète de toute façon, même sans la règle.
- Amelioration = Confiance / Fréquence du résultat(la frequence total d'acheter l'item antecedent)

- **Amélioration = 1 :** La règle est inutile. Le fait d'acheter A ne change absolument rien à la probabilité d'acheter B. 
- **Amélioration < 1 :** La règle est "négative". Si un client achète A, il a **moins** de chances d'acheter B que n'importe quel client pris au hasard. Il vaut mieux éviter cette règle.
- **Amélioration > 1 :** La règle est **intéressante**. Cela signifie que l'achat de A "booste" (améliore) la probabilité d'acheter B. Plus le chiffre est élevé, plus la règle est forte.

# Recherche de regles:
Soient une liste de n articles et de m achats.
1. Calculer le nombre d'occurrences de chaque article.
2. Calculer le tableau des co-occurrences pour les paires d'articles. 
3. Déterminer les règles de niveau 2 en utilisant les valeurs de support, confiance et amélioration.
4. Calculer le tableau des co-occurrences pour les triplets d'articles.
5. Déterminer les règles de niveau 3 en utilisant les valeurs de support, confiance et amélioration

# Algorithm Apriori
### Etapes(exemple page 10):
- principe: **Si un ensemble d'articles est fréquent, alors tous ses sous-ensembles sont aussi fréquents.**
- <mark style="background: #FFB86CA6;">pros:</mark> Grâce au filtre du **Support**, Apriori réduit drastiquement l'espace de recherche dès la première étape en éliminant tous les articles impopulaires et leurs futures combinaisons
- <mark style="background: #FFB86CA6;">cons: </mark>
	- Si le dataset est gigantesque et le support minimum est très bas, l'algorithme peut encore être lent car il doit scanner la base de données à chaque étape.
	- Il ne prend pas en compte l'ordre des achats (contrairement aux algorithmes de séquences).
- <mark style="background: #FFB86CA6;">Etapes: </mark>
1. Rechercher des k-itemset les plus frequent(support > MINSUPPORT)
	 - Les sous-itemset d'un k-itemset frequent sont forcement frequent
2. Construction des règles a partir de ces k-itemset (confiance > MINCONF)

##### Explication etape 2:
- **Tu prends ton k-itemset :** Disons que tu as trouvé que {A, B, C} est un itemset fréquent 
- **Tu explores toutes les combinaisons de règles possibles** à partir de cet ensemble :
    - Si {A, B, C} est fréquent, tu peux tester :
        - Si A et B alors C
        - Si A et C alors B
        - Si B et C alors A
        - Si A alors B et C
        - ...et ainsi de suite.
- **Tu filtres avec la Confiance (MINCONF)**


# Complexité de l'algorithme:
- explosion combinatoire
##### Astuces:
- Supprimer les transactions qui ne contient pas k-items pour les parcours suivants
- <mark style="background: #ABF7F7A6;">Partitionnement: </mark> Si un item set est frequent dans la DB, il est forcement frequent aussi dans toute partition de la DB
- <mark style="background: #ABF7F7A6;">Échantillonnage:</mark>  travailler sur un échantillon de le DB. **RISK:** rater des combinaison rare. **La parade :** On baisse le seuil de support . Si dans 10% des données tu cherches un support de 0,5% au lieu de 5%, tu augmentes tes chances de trouver les mêmes règles que sur la base complète.

# Avantages

- Règle facile a interpreter.
- Simple
- Apprentissage non supervise (pas d'hypotheses)

# Inconvenients
- Complexite temporelle et spacialle elevee
- Des regles inutiles parfois
- Non efficase pour les cas rares

# Algorithm de sequence
- GSP
- SPRADE
- PrefixSpan
a