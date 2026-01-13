
#### KNN: 
- If your data points have _n_ attributes (features), you can start by setting  
**k = n + 1** 
- Vote majoritaire: The class that appears **most frequently** among the _k_ neighbors is assigned to _y_
- Vote majoritaire pondere: a neighbor that is _very close_ to _y_ has a higher weight

| Avantages                                                             | Inconvénients                                             |
| :-------------------------------------------------------------------- | :-------------------------------------------------------- |
| Pas d’apprentissage                                                   | Temps élevé                                               |
| Nouvelles données ajoutées sans reconstruire le modèle                | Besoin de stocker toutes les données (gros stockage)      |
| Résultats faciles à interpréter                                       | Sensible au choix de la distance et du nombre de voisins  |
| Donne souvent de bons résultats par rapport à d’autres algorithmes ML | Dépend fortement du mode de combinaison et des paramètres |
#### Classification Bayésienne
- Calcul explicite de probabilités sur des hypothèses
- Utiliser la théorie des probabilités bayésiennes pour estimer la probabilité
qu'une nouvelle observation appartienne à chaque catégorie.

|  Avantages                                        | Inconvénients                                        |
| ------------------------------------------------- | ---------------------------------------------------- |
| Simple et rapide à entraîner                      | Suppose l’indépendance des attributs                 |
| Fonctionne bien avec peu de données               | Performances limitées si les attributs sont corrélés |
| Donne des probabilités claires pour chaque classe | Sensible aux probabilités très faibles (zéros)       |
#### Arbre de decision
- Un arbre de décision est un arbre où
	▪ Noeud interne = un attribut
	▪ Branche d’un nœud = un test sur un attribut
	▪ Feuilles = classe donnée
- Les mesures pour selectioner les attributs du split:
    - Gain d’Information (ID3, C4.5)
	- Indice Gini (CART)
	- Table de contingence statistique χ2 (CHAID)
	- G-statistic
##### Sélection de l’attribut
- On calcule le **Gain(S, A)** pour **chaque attribut A**.
- L’attribut qui a le **plus grand gain d’information** est choisi pour **diviser le nœud courant**.
- $$
Gain() = Entropie(S) - Entropie_{\text{après}}(\text{division\_météo})
$$
##### Critiques:
• Compréhensible 
• Tous types de données
• Robuste au bruit et aux valeurs manquantes
• Attributs apparaissent dans l’ordre de pertinence ! sélection d’attributs
• Classification rapide (parcours d’un chemin dans un arbre)
• Sensibles au nombre de classes: performances se dégradent
• Evolutivité dans le temps: si les données évoluent dans le temps
-> relancer la phase d’apprentissage

##### Variants:
• CART 
• C5
• SLIQSLIQ 

---
#### Support vector machine
- 1995,
--> Trouvez un hyperplan optimal qui sépare les données.

![[Pasted image 20251104161705.png]]
###### si les donnees ne sont pas linearement separables?
•Autoriser quelques erreurs dans la classification
•Mappez les données dans un espace de dimension supérieure où
elles peuvent être linéairement séparables.
*** noyau***
- Le noyau (kernel) est une fonction mathématique utilisée pour mesurer la similarité entre deux points de données.
- Le noyau (kernel) sert à projeter les données d’un espace initial (souvent 2D) vers un espace de dimension supérieure (par exemple 3D), où elles deviennent linéairement séparables.

| forces                       | faiblesse               |
| ---------------------------- | ----------------------- |
| Apprentissage Facile         | bonne fonction du noyau |
| Bonne adaptation aux donnees |                         |

---
### Mesure de qualite d'un classificateur:
#### Cas de classification bidimentionnelle
##### Cas de classes déséquilibres
- Accuracy treats all samples equally, but in an imbalanced dataset, that hides the poor performance on minority classes.
- High accuracy, but completely useless for C−, since the model never identifies it correctly.

---> Use a better evaluation mettric like F-mesure
---> Balance the data
---> adjust class weights; `class_weight='balanced'`
##### Precision
- Précision = <mark style="background: #D2B3FFA6;">TP / (TP + FP)  </mark>
→ parmi tout ce que ton modèle **a prédit comme positif**, quelle proportion est correcte ?
##### Le rappel:
- Mesurer la proportion d'observations positive des donnees.
- Mesure la capacité d’un modèle à identifier correctement les exemples positifs parmi tous les positifs réels.

Sa formule est :

$$
\text{Rappel} = \frac{TP}{TP + FN}
$$

où :

- \( TP \) (*True Positives*) : nombre de vrais positifs — le modèle a prédit « positif » et c’était correct.  
- \( FN \) (*False Negatives*) : nombre de faux négatifs — le modèle a prédit « négatif » alors que c’était en réalité « positif ».



##### F-mesure (F1)
- On se focalise sur l’identification des « + ».
- Elle donne la même importance aux deux critères.
- F1 mesure is good on imblanced data
- when you need a **single number** summarizing classifier quality beyond accuracy
Le **score F1** combine la précision et le rappel :

$$
F_1 = 2 \times \frac{\text{Précision} \times \text{Rappel}}{\text{Précision} + \text{Rappel}}
$$
##### ROC classification
- The **ROC curve** visualizes the **trade-off** between sensitivity (true positive rate) and fallout (false positive rate) across all possible thresholds of classification
- The closer the curve is to the top-left corner , the better the classifier.
![[Pasted image 20251104163143.png]]
- L’<mark style="background: #FFB8EBA6;">AUC (Area Under the Curve)</mark> est l’aire sous la courbe ROC.  
C’est un nombre compris entre 0 et 1 qui résume la performance globale du modèle.
- <mark style="background: #ADCCFFA6;">AUC = 1 </mark>→ modèle parfait
- <mark style="background: #ADCCFFA6;">AUC = 0.5 </mark>→ modèle aléatoire
- AUC < 0.5 → modèle pire que le hasard (il inverse les classes)
![[Pasted image 20251104163157.png]]

#### Cas de classification mutlidimentionnelle
######  Micro-average:
• additionner les tables de contingence 2x2 pour chaque classe
• calculer le rappel et la précision à partir du tableau récapitulatif
![[Pasted image 20251104163508.png|700]]

###### Macro-average:
• calculer le rappel et la précision de chaque table de contingence
• calculer la moyenne des estimations de rappel et de précision
![[Pasted image 20251104163426.png|700]]
###### Différence de base:
• La micro-average ==> <mark style="background: #FFF3A3A6;">les grandes classes-domine les classes</mark>
• La macro-average donne <mark style="background: #FFF3A3A6;">un poids égal à chaque classe</mark>
---> r / p sur les classes plus petites compte autant que sur les classes plus
grandes

---
#### Tuning des hyper-paramètres des algorithmes d’apprentissage supervisés

- Un **hyperparamètre** est une variable **qui contrôle le comportement de l’algorithme d’apprentissage**, mais **qui n’est pas apprise à partir des données**.

| **Algorithme supervisé**        | **Exemples d’hyperparamètres**                                       |
| ------------------------------- | -------------------------------------------------------------------- |
| SVM (Support Vector Machine)    | `C`, `kernel`, `gamma`                                               |
| Arbre de décision               | `max_depth`, `min_samples_split`, `min_samples_leaf`                 |
| Forêt aléatoire (Random Forest) | `n_estimators`, `max_features`, `max_depth`                          |
| k-Nearest Neighbors (k-NN)      | `n_neighbors`, `metric`, `weights`                                   |
| Réseau de neurones              | `learning_rate`, `n_layers`, `n_neurons`, `batch_size`, `activation` |
| Régression logistique           | `C`, `penalty`, `solver`                                             |

## SVM

- <mark style="background: #ADCCFFA6;">SVM: </mark> trouver un <mark style="background: #ADCCFFA6;">hyperplan optimal qui sépare</mark> de manière maximale les données dans un espace de caractéristiques, permettant ainsi une <mark style="background: #ADCCFFA6;">classification précise</mark>
- <mark style="background: #BBFABBA6;">SI les donnees ne sont pas linearement separables??</mark>
	1. **Kernel Trick:** on les projette dans un espace de <mark style="background: #BBFABBA6;">dimension plus élevée </mark>où elles deviennent séparables par un hyperplan linéaire.
	2. **Soft margin:**  on <mark style="background: #BBFABBA6;">autorise certaines erreurs</mark> (points mal classés)

##### Methodes du turing (adjusting) des hyper-parametres
###### Grid search
- On définit un ensemble fini de valeurs possibles pour chaque hyperparamètre, on teste <mark style="background: #D2B3FFA6;">toutes les combinaisons</mark>, et on choisit celle qui donne la meilleure performance.

###### Random Search
- Au lieu d’explorer toute la grille, on fixe le nombre des combinaisons dans l’espace des hyperparamètres.
---> Beaucoup plus efficace si certains paramètres n’ont pas une grande influence.
---> Permet d’explorer des intervalles continus
--->Plus efficace que Grid Search pour <mark style="background: #D2B3FFA6;">les grands espaces</mark>

###### cross-validation
Quelle que soit la méthode de recherche, il faut **évaluer correctement** chaque combinaison de paramètres.  
Au lieu d’un simple split _train/test_, on utilise la **k-fold cross-validation** :
- On divise les données en k parties,
- <mark style="background: #D2B3FFA6;">On entraîne sur k−1 parties et teste sur la partie restante,</mark>
- <mark style="background: #D2B3FFA6;">On répète k fois et on moyenne les performances.</mark>
---> une mesure plus fiable de la qualité du modèle pour chaque combinaison d’hyperparamètres.

---

#### Biais et Variance
- **Biais (erreur de biais)**
    - Résulte d’hypothèses incorrectes (ex : penser que les données sont linéaires alors qu’elles sont quadratiques).
    - Modèles à biais élevé : ne capturent pas les tendances, trop simplifiés, taux d’erreur élevé.
    - Modèles à biais faible : correspondent mieux aux données d’apprentissage.
        
- **Variance (erreur de variance)**
    - Résulte de la sensibilité du modèle aux petites variations des données d’apprentissage.
    - Modèles à variance élevée : bruit dans les données, potentiel de sur-apprentissage (overfitting), modèles complexes.
        
- **Compromis biais-variance**
    - Modèle simple : biais élevé, variance faible.
    - Modèle complexe : biais faible, variance élevée.
    - Objectif : minimiser l’erreur totale pour trouver un équilibre entre underfitting et overfitting.
        

#### Overfitting
- Se produit lorsque le modèle mémorise le bruit et les valeurs incorrectes du dataset.
- Caractéristiques : faible biais, forte variance.
- Conséquence : dégradation du modèle, mauvaise généralisation.

##### Techniques pour éviter l’overfitting

- <mark style="background: #ADCCFFA6;">Plus de données :</mark> Ajouter plus d’exemples réduit l’impact du bruit    
- <mark style="background: #ADCCFFA6;">Arrêt précoce (Early stopping) :</mark> arrêter l’apprentissage avant que le modèle ne commence à sur-ajuster.
- <mark style="background: #ADCCFFA6;">Réduction de features : </mark>supprimer les caractéristiques non pertinentes pour améliorer la précision.
- Régularisation : ajouter une pénalité aux coefficients du modèle. 
    - **L1 (Lasso)** : somme des valeurs absolues, peut réduire certains coefficients à zéro (sélection de features).  
    - **L2 (Ridge)** : somme des carrés, réduit tous les coefficients sans les éliminer.
- <mark style="background: #ADCCFFA6;">Ensemble learning :</mark> combiner plusieurs modèles.
    - **Bagging** : modèles indépendants sur des échantillons bootstrap, combine par vote ou moyenne, réduit la variance.
    - **Boosting** : modèles faibles construits séquentiellement, corrigent les erreurs des modèles précédents, réduit le biais.
        
#### Underfitting
- Le modèle est trop simple ou sous-entraîné et <mark style="background: #ABF7F7A6;">ne capture pas les relations complexes.</mark>
- Conséquence : mauvaise performance sur les données d’entraînement et de test.

##### Techniques pour éviter l’underfitting
- Utiliser un modèle plus complexe.
- Augmenter le temps d’apprentissage et ajuster l’arrêt précoces
- Éliminer le bruit des données.
- Ajuster les paramètres de régularisation.
- Essayer un modèle différent.
- Augmenter le nombre de features et appliquer du feature engineering.