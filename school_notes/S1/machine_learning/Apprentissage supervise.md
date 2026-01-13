#  Apprentissage Automatique — Chapitre III : Apprentissage Supervisé (Régression)

Un modèle de Machine Learning supervisé  est un type d’algorithme qui apprend à partir de données étiquetées — <mark style="background: #BBFABBA6;">c’est-à-dire des données pour lesquelles on connaît déjà la réponse attendue</mark> (la sortie)

## 1. Introduction

### Formulation du problème

- On dispose d’un ensemble fini de paires (entrée, sortie désirée).
- Objectif : créer automatiquement un modèle capable de prédire la sortie d’une nouvelle entrée.
    

### Types d’apprentissage supervisé

| Type                   | Nature de la sortie                 | Exemple                                    |
| ---------------------- | ----------------------------------- | ------------------------------------------ |
| **Classification**     | Nominale (catégories)               | Malade/sain, accord/refus de crédit        |
| **Régression**         | Continue (valeurs réelles)          | Prix d’une maison, consommation électrique |
| **Séries temporelles** | Valeurs futures basées sur le passé | Rendement d’une action, météo              |

---

## 2. Exemples d’applications

### Régression
- Prédiction du montant des ventes d’une entreprise.
- Estimation du prix d’un bien immobilier.
- Prévision de la consommation électrique d’une ville.

### Classification
- Diagnostic médical (sain/malade).
- Décision d’octroi de crédit.
- Reconnaissance d’écriture manuscrite (chiffres).
    
---

## 3. Méthodes de Régression

### Liste des principales méthodes
- Régression Linéaire Simple (RLS)
- Régression Linéaire Multiple (RLM)
- Régression Polynomiale (RP)
- Régression Ridge et Lasso (RR–RL)
- Arbres de Régression (AD–R)
- Support Vector Machines pour la Régression (SVM–R)

---

## 4. Régression Linéaire

### Modèle général
Forme du modèle :  
**Y = β₀ + β₁X + ε**

- β₀ : ordonnée à l’origine    
- β₁ : pente (impact de X sur Y)  
- ε : erreur (bruit aléatoire) 

### Principe

Trouver la meilleure droite qui **minimise l’erreur entre valeurs réelles et prédites**.
### *Méthodes d’estimation
- **Méthode des moindres carrés:** <mark style="background: #FFF3A3A6;">minimiser la somme des erreurs au carré</mark>
- **Méthode de la vraisemblance:** on cherche <mark style="background: #FFF3A3A6;">les paramètres </mark>qui rendent les données observées <mark style="background: #FFF3A3B6;">les plus probables</mark> selon une distribution statistique hypothétique des erreurs.

### Optimisation

- Recherche analytique des coefficients (β₀, β₁)
- Descente de gradient (pour un ou plusieurs paramètres)

---

## 5. Régression Linéaire Multiple

### Modèle

**Y = β₀ + β₁X₁ + β₂X₂ + … + βₙXₙ + ε**

- Plusieurs variables explicatives X₁, X₂, …, Xₙ
- Représentation matricielle pour simplifier le calcul

### Apprentissage

- Descente de gradient pour estimer les coefficients
- Évaluation par erreur quadratique moyenne (MSE)

---

## 6. Types de relations et solutions

|Type de relation|Solution|
|---|---|
|Linéaire|Régression linéaire|
|Non linéaire|Linéarisation du modèle ou Régression polynomiale|

---

## 7. Régression Polynomiale

- Extension de la régression linéaire : ajout de termes non linéaires (X², X³, …).
- Permet d’ajuster des courbes plutôt que des droites.

- Si le <mark style="background: #FF5582A6;">degré</mark> du polynôme est <mark style="background: #FF5582A6;">trop petit</mark>, le modèle sous-ajuste (underfitting) 
- Si le <mark style="background: #FF5582A6;">degré</mark> est trop grand, il <mark style="background: #FF5582A6;">sur-ajuste</mark> (overfitting) : il s’adapte trop au bruit des données.

---

## 8. Arbres de Régression

### Principe

- Division récursive de l’espace des données en sous-ensembles homogènes.
- À chaque nœud : choix d’une variable et d’un seuil de coupure (segmentation).

### Critère de segmentation

- Minimisation de la variance intra-feuille (ou de l’erreur quadratique).

### Résultat

- Prédiction = moyenne des valeurs de la feuille où tombe l’exemple.

---

## 9. Support Vector Regression (SVR)

### Principe

- Variante de SVM pour la régression.
- Objectif : trouver une fonction **aussi plate que possible** qui reste dans une **marge de tolérance ε** autour des points d’apprentissage.

### Idée clé

- Les points à l’extérieur de la marge sont les **vecteurs de support**, qui influencent le modèle.
    
---

## 10. Évaluation de la Régression

### Jeux de données
- **Ensemble d’entraînement** : sert à construire le modèle.
- **Ensemble de test / validation** : sert à évaluer la performance.

### Mesures d’évaluation

- Erreur quadratique moyenne (MSE)
- Erreur absolue moyenne (MAE)
- Coefficient de détermination (R²)

---

## 11. Conclusion

- La **régression supervisée** vise à prédire des valeurs continues à partir de données d’entrée.
- Les méthodes diffèrent selon la nature des relations entre variables (linéaire, non linéaire, complexe).
- L’évaluation rigoureuse (train/test) est essentielle pour garantir la capacité de **généralisation** du modèle.
    
---

Souhaites-tu que je t’ajoute un petit **schéma récapitulatif** (texte ou ASCII) pour visualiser les types de régression et leur logique d’apprentissage ?