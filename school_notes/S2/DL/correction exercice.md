### 1-a : Recodage binaire des données

Pour qu'un réseau de neurones puisse traiter ces données, nous devons transformer les attributs catégoriels en valeurs numériques, idéalement dans l'intervalle $[0, 1]$.

*   **Âge :** C'est une variable à 3 modalités. On utilise le **One-Hot Encoding** (codage disjonctif complet) pour éviter d'induire un ordre artificiel.
    *   $x_1$ (Âgé) : 1 si Âgé, 0 sinon.
    *   $x_2$ (Moyen) : 1 si Moyen, 0 sinon.
    *   $x_3$ (Récent) : 1 si Récent, 0 sinon.
*   **Concurrence :** Variable binaire.
    *   $x_4$ : 1 si "Oui", 0 si "Non".
*   **Type :** Variable binaire.
    *   $x_5$ : 1 si "Hardware", 0 si "Software".
*   **Profit (Cible/Classe $c$) :**
    *   $c = 1$ si "Hausse", $c = 0$ si "Baisse".

**Justification :** Le codage binaire 0/1 est adapté aux fonctions d'activation courantes et permet de représenter clairement la présence ou l'absence d'une caractéristique sans biais d'échelle.

---

### 1-b : Architecture du réseau de neurones

L'architecture adéquate pour un perceptron simple dans ce cas est la suivante :

*   **Couche d'entrée :** 5 neurones ($x_1$ à $x_5$) correspondant aux variables recodées, plus un neurone de biais ($x_0 = 1$).
*   **Poids :** Un vecteur de poids $W = [w_0, w_1, w_2, w_3, w_4, w_5]$.
*   **Fonction d'agrégation :** Somme pondérée $net = \sum_{i=0}^{5} w_i \cdot x_i$.
*   **Fonction d'activation :** Fonction échelon (Heaviside) : $o = 1$ si $net > 0$, sinon $o = 0$.
*   **Couche de sortie :** 1 neurone représentant la prédiction du Profit.

**Dessin :**
```text
(Biais 1) --w0--> [ Neurone ]
(x1:Âgé)  --w1--> [    de    ]
(x2:Moy)  --w2--> [  Sortie  ] --> Sortie (o)
(x3:Réc)  --w3--> [     f    ]
(x4:Conc) --w4--> [          ]
(x5:Type) --w5--> [          ]
```

---

### 1-c : Table d'apprentissage (Échantillons 1 à 4)

Initialisation : Tous les poids $w_i = 1$. Règle : $w_i \leftarrow w_i + (c-o)x_i$.
On définit le vecteur d'entrée comme $[x_0, x_1, x_2, x_3, x_4, x_5]$.

| It  | Entrep. | Entrée $[x_0..x_5]$  | Cible $c$ | $net = \sum w_i x_i$ | Sortie $o$ | Erreur $(c-o)$ | Nouveaux poids $w$     |
| :-: | :-----: | :------------------- | :-------: | :------------------: | :--------: | :------------: | :--------------------- |
|  0  |    -    | -                    |     -     |          -           |     -      |       -        | $[1, 1, 1, 1, 1, 1]$   |
|  1  |    1    | $[1, 1, 0, 0, 0, 0]$ |     0     |    $1(1)+1(1)=2$     |     1      |       -1       | $[0, 0, 1, 1, 1, 1]$   |
|  1  |    2    | $[1, 0, 1, 0, 1, 0]$ |     0     |  $0(1)+1(1)+1(1)=2$  |     1      |       -1       | $[-1, 0, 0, 1, 0, 1]$  |
|  1  |    3    | $[1, 0, 1, 0, 0, 1]$ |     1     | $-1(1)+0(1)+1(1)=0$  |     0      |       +1       | $[0, 0, 1, 1, 0, 2]$   |
|  1  |    4    | $[1, 1, 0, 0, 0, 1]$ |     0     |  $0(1)+0(1)+2(1)=2$  |     1      |       -1       | $[-1, -1, 1, 1, 0, 1]$ |

Les poids après cette itération partielle sont : $w_0=-1, w_1=-1, w_2=1, w_3=1, w_4=0, w_5=1$.

---

### 1-d : Calcul du taux d'erreur sur les données de test (7 à 10)

On utilise le modèle final de la question précédente : $W = [-1, -1, 1, 1, 0, 1]$.

1.  **Entreprise 7** (Moyen, Non, Software) : $x = [1, 0, 1, 0, 0, 0]$, $c=1$.
    *   $net = -1(1) + 1(1) = 0 \rightarrow o = 0$. (Erreur : $0 \neq 1$)
2.  **Entreprise 8** (Récent, Oui, Software) : $x = [1, 0, 0, 1, 1, 0]$, $c=1$.
    *   $net = -1(1) + 1(1) + 0(1) = 0 \rightarrow o = 0$. (Erreur : $0 \neq 1$)
3.  **Entreprise 9** (Moyen, Oui, Hardware) : $x = [1, 0, 1, 0, 1, 1]$, $c=0$.
    *   $net = -1(1) + 1(1) + 0(1) + 1(1) = 1 \rightarrow o = 1$. (Erreur : $1 \neq 0$)
4.  **Entreprise 10** (Âgé, Oui, Software) : $x = [1, 1, 0, 0, 1, 0]$, $c=0$.
    *   $net = -1(1) - 1(1) + 0(1) = -2 \rightarrow o = 0$. (Correct : $0 = 0$)

**Taux d'erreur :** Il y a 3 erreurs sur 4 exemples.
$Taux = \frac{3}{4} = 0,75$ soit **75% d'erreur**.1

---

### 1-e : Amélioration de la performance (4 points)

Pour améliorer les performances de ce modèle :
1.  **Augmenter le nombre d'itérations (époques) :** Le perceptron n'a fait qu'un passage partiel sur les données ; il faut boucler jusqu'à stabilisation des poids.
2.  **Augmenter la taille de la base d'apprentissage :** 6 exemples sont insuffisants pour généraliser correctement sur des données complexes.
3.  **Utiliser un Perceptron Multicouche (MLP) :** Ajouter des couches cachées pour permettre au réseau de capturer des relations non-linéaires entre les attributs.
4.  **Ajuster le taux d'apprentissage ($\eta$) :** Utiliser une valeur plus petite que 1 pour éviter des oscillations brutales des poids et assurer une convergence plus stable.