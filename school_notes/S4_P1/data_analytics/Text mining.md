# definitions
- **Data mining:** exploitation  ou analyse par des methode automatique ou semi-automatique dans une grande masse de données pour trouver des tendances ou des règles.
- **Text mining:** data mining sur les data textuelles. 

# Traitement automatique du langage(TAL):
##### TAL base languistique:
- Des règles grammaticales strictes.
- On crée des graphes où les mots sont reliés entre eux

==> **Pas suffisant car:** gros volume de données. Bruit dans les données. Besoin d'expertise de données.   

##### TAL base statistique et machine learning:
- l'ordinateur utilise des vecteurs numériques
- en fournissant d'immenses quantités de données, l'ordinateur comprend le contexte et la probabilité des mots
---
# Pre-traitement du text
##### **Nettoyage:** 
supprimer les annonces, traiter les tableaux, supprimer les liens et les images...
##### **Normalisation:** 
supprimer la ponctuation, les majuscules, chiffres, caractères spéciaux...
##### **Tokenization:**
Découper la phrase en des petits morceaux (mots ou alphabets). exemple:["Le", "chat", "dort", "."]
##### **Indexation:**
permet de passer d'un text brut a  une **table ou une matrice** 
###### Bag of words:
- On considère le document comme une simple liste de mots, sans ordre. On compte juste combien de fois chaque mot apparaît.
- <mark style="background: #FFF3A3A6;">Inconvenient: </mark> Perte de context. Explosion de la dimension car différentes forment d'un mots se comptent comme différents mots. 
###### N-grammes en mots
- On prend des séquences de **n** mots adjacents
- <mark style="background: #FFF3A3A6;">Avantages:</mark> Capturer plus de sens
- <mark style="background: #FFF3A3A6;">Problemes:</mark> 
	- Plus `n` est grand plus la complexite augmente.
	- Des combinaisons de 3 ou 4 mots n'apportent pas de sens

###### N-grammes en lettres:
- On prend des fenêtres glissantes de n caractères
- <mark style="background: #FFF3A3A6;">Avantages:</mark> 
	- Robustes contre les fautes d'orthographe. 
	- indépendante de la langue.
	- Utiliser pour les sequences d'ADN
- <mark style="background: #FFF3A3A6;">Problemes:</mark> Résultat illisible par l'human.


##### **Ponderation**
<mark style="background: #FFB86CA6;">Objectif: </mark>Quelle valeur numérique donner à chaque mot pour représenter son importance dans un document ?
###### Ponderation binaire
- Le mot est-il présent dans le document ? Oui (1) ou Non (0). On ignore combien de fois il apparaît.
- <mark style="background: #FFF3A3A6;">Problemes:</mark> Les ecarts entre les documents sont très grands si on utilise une distance euclidienne.
###### Fréquence des termes(TF)
-  On compte le nombre d'apparitions du mot dans le document.
- <mark style="background: #FFF3A3A6;">Avantage : </mark> La repetition d'un mot traduit son importance. 
- <mark style="background: #FFF3A3A6;">Limite :</mark>  Un long document aura mécaniquement des scores TF plus élevés qu'un court, ce qui fausse les comparaisons.
###### Fréquence Inverse de Document(IDF):
- évalue l'**importance** ou la **rareté** d'un mot dans l'ensemble de vos documents
- Si un mot est présent dans **tous** les documents, il n'a aucune valeur discriminante

###### TF-IDF (Le standard)
- Mesure **l'importance relative** d'un mot dans un document par rapport à une collection de documents
- C'est la combinaison de l'importance d'un mot dans le document(frequence) et dans tous les documents(IDF).
- Le TF-IDF permet d'extraire les "mots-clés" caractéristiques d'un document. 

###### Normalisation (Amortir les écarts)
- Pour corriger le biais de la longueur des documents, on "normalise" les scores.
- <mark style="background: #FFF3A3A6;">Avantage : </mark> Réduit l'impact des très hautes fréquences(le passage de 100 à 101 répétitions est moins important que de 1 à 2).

## Reduction de la dimensionnalité:
 
##### Retrait des stopwords(mot vide):
- Leurs fréquence est la meme dans tous les documents, donc ils sont inutiles en text mining.
##### Lemmatization
- Analyser les termes afin d'extraire leur forme canonique(am, is, was -> be)
- Éliminer les differences de formes: féminin,masculin, pluriels, passer, present...
- <mark style="background: #FFF3A3A6;">Problèmes: </mark>
	- Spécifique a chaque langue
	- Des erreurs possibles

##### Racination (Stemming):
- Le but est de réduire la taille du vocabulaire en supprimant les suffixes des mots pour ne garder que la racine.
- <mark style="background: #FFF3A3A6;">Problèmes: </mark>
	- N'authorise plus d’après traitements sur le mot
	- Peut conduire a de fausse groupement
	- Il peut couper deux mots qui n'ont rien à voir et les transformer en une racine identique

##### Filtrage par la fréquence
- <mark style="background: #ABF7F7A6;">Fréquence:</mark> Nombre de documents ou le mot apparaît au moins une fois par rapport au nombre total des documents.
- **très élevée:** mot present dans tous les documents. permet de cerner le domain mais ne différencier pas entre les documents.
- **Trop faible:** terme rare. N'apporte pas une difference significatif entre les documents
- **Le seuil reste arbitraire**

## Mesure de similarité:
###### Distance euclidienne:
- Elle est sensible a la taille du text. 
- deux documents qui parlent du même sujet mais dont l'un est très court et l'autre très long seront vus comme très éloignés, simplement à cause de la différence de nombre de mots.

###### La Similarité Cosinus
- Si deux documents traitent du <mark style="background: #FFB86CA6;">même sujet, les vecteurs pointent dans la même direction</mark>, quel que soit le nombre de mots utilisés.z
- Le résultat est **normalisé** (toujours entre 0 et 1), ce qui facilite grandement les comparaisons.

###### L'Indice de Jaccard
- **Le principe :** On ne s'intéresse plus aux valeurs (fréquences), mais uniquement à la présence ou l'absence des mots (pondération binaire).
- **Usage :** Idéal pour comparer des ensembles de caractéristiques (ex: quels mots sont partagés ?), mais moins fin que le cosinus car il ignore la répétition des termes.

---
# Categorization du text:
## Étapes:
1.  La préparation des données
2.  Le fractionnement(test and training data)
3. La vectorisation : transformation des documents en matrice
4. La construction du classifieur
5. L'évaluation

## Défis:
- <mark style="background: #BBFABBA6;">Grandes dimensions:</mark> le tableau "termes x textes" peut avoir des milliers de colonnes et des lignes avec la majorité des cellules vides.
- <mark style="background: #BBFABBA6;">Complexité des algorithmes:</mark>
	- La plupart des algorithmes d'apprentissage automatique sont sensibles au nombre des variables.
	- Il faut absolument fair une reduction de la dimension. Ce qui est critique car on peut supprimer des termes importants
- <mark style="background: #BBFABBA6;">Desequilibrage:</mark>
	- classes peu nombreuses mal représentees
	- **solution:** technique de redressement

## Reduction de la dimentionalite
- Selection des attributs et features les plus informatifs pour but d'optimization pour le model
-> Reduit le bruit
-> Moins de complexite de temps
->ameliorer la performance du classificateur finale

### method "Wrapper":
- C'est une méthode de type <mark style="background: #FF5582A6;">"trial and error"</mark>. Le modèle propose un sous-ensemble de mots, on entraîne le classifieur(SVM, arbre de decision...), on regarde la performance. On recommence pour trouver la combinaison qui donne le meilleur score.
- Efficace pour <mark style="background: #FF5582A6;">trouver les interactions complexes</mark> des mots
- Tres coûteux: <mark style="background: #FF5582A6;">explosion des combinaisons</mark>.

### method "Filter":
- On calcule une **mesure statistique** pour chaque mot afin de voir s'il est "utile" pour distinguer les classes
- n'est pas couteux
- <mark style="background: #FF5582A6;">mesures de filtrage:</mark>
	1. Information gain
	2. khi-deux: compare les frequences observees
	3. mutual information: Mesure la dépendance statistique entre le mot et la classe.
	4. Odds ratio: la proba qu'un mot appartient a une classe
- <mark style="background: #FF5582A6;">Avantage: </mark>
	- **Indépendance du classificateur :** La méthode Filter évalue les caractéristiques (features) intrinsèquement, sans avoir besoin d'entraîner un modèle de classification spécifique
	- La méthode Filter est réalisable sur de **très grands ensembles de caractéristiques**.
	- Il n'est pas couteuse en terme de temps.
	

## Model adaptes:
- SVM
- KNN
- Decision trees
- Naive bais

---
# Opinion mining
## Deux approches
### Approche statistique(machine learning):
- On entraîne un model sur du texte deja note(+1,-1) par des humains.
- <mark style="background: #BBFABBA6;">avantage:</mark> Precis.
- <mark style="background: #BBFABBA6;">problem:</mark> Demande beaucoup de données étiquetées par des humains.
### Approche Lexicale:
- On utilise des dictionnaires spécialisés (comme **SentiWordNet**) qui listent des mots avec leur score de "positivité" ou "négativité".
- Le score final du text est la somme des scores des mots
- <mark style="background: #BBFABBA6;">avantage:</mark> facile, pas besoin d'entrainement
- <mark style="background: #BBFABBA6;">problem:</mark> ne gère pas l'ironie et la satire

---
# Evaluation:
### Stratégies de validation
*   **Test set (méthode de maintien) :** Réserver une partie des données pour le test. *Inconvénients :* Gaspillage de données (qui manquent pour l'apprentissage) et coût potentiel de l'étiquetage.
*   **Validation croisée (Cross-validation) :** Diviser les données en $n$ partitions (généralement 10). On effectue $n$ tests : à chaque itération, une partition sert de test et les $n-1$ autres servent à l'apprentissage. On calcule ensuite la moyenne des résultats.

###  Métriques pour la classification binaire
L'évaluation repose sur la **matrice de confusion** qui confronte la classe observée à la classe prédite :
*   **Taux d'erreur :** $\epsilon = \frac{b+c}{n}$
*   **Rappel (Recall) :** $R = \frac{a}{a+b}$ (capacité à détecter les éléments positifs).
*   **Précision :** $P = \frac{a}{a+c}$ (fiabilité des prédictions positives).
*   **F-Mesure (F1-score) :** Moyenne harmonique entre précision et rappel, donnant une importance égale aux deux : $F_1 = 2 \frac{P \times R}{P+R}$
*   **Fß-Mesure :** Variante permettant de moduler l'importance relative donnée à la précision par rapport au rappel.

### Classification multi-classes
Pour les problèmes avec plus de deux classes, on peut calculer une moyenne globale des performances :
*   **Micro-average :** On additionne toutes les tables de contingence des différentes classes avant de calculer le rappel et la précision. <mark style="background: #FFB86CA6;">Cette méthode favorise les classes les plus larges.</mark>
*   **Macro-average :** On calcule le rappel et la précision <mark style="background: #FFB86CA6;">pour chaque classe individuellement, puis on en fait la moyenne arithmétique</mark>. Cette méthode donne le **même poids** à chaque classe, quelle que soit sa taille.