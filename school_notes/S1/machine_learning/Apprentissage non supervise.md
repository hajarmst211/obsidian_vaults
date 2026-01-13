
### Principe général

Deux approches existent :
##### Méthodes ascendantes (agglomératives)
Départ : chaque objet est un cluster individuel.  
Étapes :
1. Calculer la matrice de distances entre tous les objets.
2. Fusionner les deux clusters les plus proches.   
3. Recalculer les distances entre le nouveau cluster et les autres.     
4. Répéter jusqu’à obtenir un seul cluster (ou un critère d’arrêt).
        
##### Méthodes descendantes (divisives)
Départ : un seul cluster contenant tous les objets.  
Étapes :
1. Diviser le cluster le plus hétérogène.    
2. Répéter la division jusqu’à obtenir les k clusters souhaités.
        
### Mesure de distance entre clusters

Le résultat dépend de la manière dont on définit la **distance inter-cluster** :
- **Single Link (liaison simple)** : distance minimale entre un élément du cluster A et un élément du cluster B.  
    → Tendance à former des chaînes allongées (effet “chaining”).
    
- **Complete Link (liaison complète)** : distance maximale entre deux éléments de clusters différents.  
    → Produit des clusters plus compacts et sphériques.

- **Average Link** : distance moyenne entre tous les couples d’éléments de deux clusters.
- **Centroid Link** : distance entre les centroïdes (moyennes) des clusters.
    
### Représentation : Dendrogramme
- Axe vertical = distance de fusion.
- On peut choisir un seuil de coupure pour déterminer le nombre de clusters.

### Avantages
- Interprétation intuitive (visualisation hiérarchique).
- Pas besoin de spécifier k.
- Méthodes bien théorisées et robustes pour petits jeux de données.

### Inconvénients
- Décisions irréversibles (erreur de fusion non corrigeable).
- Complexité élevée (O(n²) en mémoire et en calcul).
- Peu adaptées aux grands ensembles de données.

---

## Section IV : Méthodes de partitionnement

Ici, on suppose **k** fixé à l’avance (ou déterminé empiriquement).  
Chaque objet est affecté **exactement à un seul cluster**.  
Les clusters sont disjoints.
### 1. Méthode des K-means

Principe : minimiser la **somme des distances intra-cluster** (variance interne).

**Algorithme :**
1. Choisir k centroïdes initiaux.
2. Affecter chaque point au cluster le plus proche (selon une distance, souvent euclidienne). 
3. Recalculer les centroïdes comme moyenne des points assignés.
4. Répéter jusqu’à stabilisation (centroïdes inchangés ou convergence du critère).  

**Complexité** : O(t × k × n), avec n = nombre de points, k = clusters, t = itérations.

**Choix du k :**

- **Connaissance du domaine** (ex: k=10 pour reconnaissance de chiffres).
- **Méthode du coude (elbow method)** : on trace la courbe “inertie intra-cluster” vs k et on choisit le point où la décroissance ralentit.
- **Coefficient de silhouette** : mesure la compacité et la séparation des clusters (valeur entre -1 et +1).
    

**Avantages :**
- Simple et rapide    
- Efficace sur grands jeux de données numériques.

**Inconvénients :**
- Nécessite de connaître k à l’avance.
- Sensible aux valeurs initiales    
- Suppose des clusters convexes et isotropes.
- Peu robuste au bruit et aux outliers.

**Variantes :**

- **K-medoids** : utilise des objets réels comme centroïdes (plus robuste aux outliers).
- **K-modes** : pour données catégorielles.
- **K-prototypes** : pour données mixtes.    
- **GMM (Gaussian Mixture Models)** : version probabiliste.

---

### **2. Méthodes basées sur la densité (DBSCAN, OPTICS, etc.)**

Principe : définir les clusters comme **zones de forte densité** séparées par des zones plus creuses.

**Paramètres :**

- ε (epsilon) : rayon du voisinage.
- MinPts : nombre minimum de points pour être considéré dense.
    

**Règles :**

- Si un point P a au moins MinPts voisins dans son rayon ε → **point dense**.
- Si P est dense et Q est dans son voisinage → Q appartient au même cluster. 
- Points non denses :
    - **bordure** : voisins d’un point dense.    
    - **bruit** : isolés, hors de tout cluster.
        

**Avantages :**
- Pas besoin de k. 
- Détecte les formes arbitraires.  
- Résiste au bruit.
    

**Inconvénients :**

- Sensible au choix de ε et MinPts.
- Difficile à utiliser avec des densités très variables.
---

## Section V : Évaluation du clustering
### Clustering:
•Regrouper des objets en <mark style="background: #ABF7F7A6;">groupes, ou classes, ou familles, ou segments, ou
clusters,</mark> de sorte que :
	✓2 objets d’un même groupe se ressemblent le + possible
	✓2 objets de groupes distincts diffèrent le + possible

•Une bonne méthode de clustering produira  avec :
	1. Similarité intra-classe importante
	2. Similarité inter-classe faible
•La qualité d’un clustering dépend de :
	3. La mesure de similarité utilisée
	4. L’implémentation de la mesure de similarité

On distingue deux grandes catégories : **interne** et **externe**.
### 1. Évaluation interne
Aucune vérité de terrain disponible.  
On mesure la qualité intrinsèque du partitionnement :
- **Intra-cluster** : compacité (les points d’un même cluster sont proches).
- **Inter-cluster** : séparation (les clusters sont éloignés).

**Indices courants :**

- ***Indice de Dunn : 
	- Il sert à quantifier **à quel point les clusters sont bien séparés et compacts**.
	=><mark style="background: #FF5582A6;">Rapport entre la plus petite distance entre deux clusters différents,  et la plus grande taille de cluster </mark>.
	
    **D = (distance minimale inter-cluster) / (distance maximale intra-cluster).**  
    → Plus D est grand, meilleur est le clustering. 
- **Silhouette coefficient** : moyenne normalisée entre cohésion et séparation.

### 2. Évaluation externe

On dispose d’un **label réel** pour comparer les clusters trouvés.
- **F-measure**, **accuracy**, etc.
- **Purity** : proportion d’objets du cluster appartenant à la classe dominante.  
    Purity(Cj) = (1 / n) × Σ max_i (|ω_i ∩ Cj|).  
    → Biaisé : avoir n clusters maximise la purity.
    
---
### Pour résumer

|Type de méthode|Principe|Paramètres|Avantages|Inconvénients|
|---|---|---|---|---|
|**Hiérarchique**|Fusion ou division progressive|Aucun (k déterminé après)|Interprétable, visuel|Complexe, irréversible|
|**K-means**|Minimisation de la variance intra-cluster|k|Simple, rapide|Nécessite k, sensible au bruit|
|**Densité (DBSCAN)**|Groupes de points denses|ε, MinPts|Détecte formes complexes, robuste|Sensible aux paramètres|
|**Évaluation interne**|Cohésion/séparation|-|Aucune vérité nécessaire|Pas toujours fiable|
|**Évaluation externe**|Comparaison à des labels|-|Interprétable|Nécessite vérité de référence|
