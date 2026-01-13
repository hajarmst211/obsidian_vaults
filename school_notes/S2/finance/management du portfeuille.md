- Cette partie est basée sur la théorie financière classique (notamment le modèle de <span style="color:rgb(233, 149, 149)">Markowitz</span> et le <span style="color:rgb(233, 149, 149)">CAPM</span>)
 - **Portefeuille efficace :** C'est un portefeuille qui, pour un niveau de risque donné, offre le rendement le plus élevé. Sur le graphique, cela correspond à la **partie haute de la courbe**.
 - Si on nous demande de trouver des proportions a fin d'avoir un <span style="color:rgb(233, 149, 149)">rendement maximum</span>, on donne une proportion égale a 1 au titre qui a l’espérance la plus élevée.
 - Si on veut un<span style="color:rgb(233, 149, 149)"> risque minimum</span>, si la correlation est égale a 1, on met 100% des proportions dans le titre le moins risque( ayant l’espérance minimale) 
 - <span style="color:rgb(233, 149, 149)">Une action</span> est un titre de propriété
# Le Portefeuille de 2 titres
Ici, on mélange deux titres (Titre 1 et Titre 2) avec des poids spécifiques ($\lambda$). Dans l'exemple, on met **40%** d'argent dans le Titre 1 ($\lambda = 0,4$) et **60%** dans le Titre 2 ($1-\lambda = 0,6$).

#### Le rendement espéré du portefeuille ($E(r_{pt})$) :
Règle de calcules:
$$E(r_{pt}) = \lambda E(r_{xt}) + (1 - \lambda) E(r_{yt})$$

C'est une simple moyenne pondérée :
$$E(r_p) = (0,4 \times 3,53\%) + (0,6 \times 4,59\%) = 4,17\%$$
<span style="color:rgb(233, 149, 149)">Interprétation : </span>Si vous investissez ainsi, vous espérez gagner 4,17% par mois.

####  La correlation:
Le calcul de la corrélation est :  $$ρ_{xy}=\dfrac{Covariance}{σ1×σ2}$$

#### Le risque (Variance) du portefeuille :
Pour un portefeuille composé de deux titres $x$ et $y$.
La formule générale de la variance dont nous partons est :
$$\sigma_p^2 = \lambda_x^2\sigma_x^2 + \lambda_y^2\sigma_y^2 + 2\lambda_x\lambda_y \rho_{xy}\sigma_x\sigma_y$$
La formule de la variance change selon la valeur de la correlation:
##### 1. Corrélation positive parfaite ($\rho_{xy} = 1$)
Les titres évoluent exactement dans le même sens.
*   **Relation :** $r_{xt} = a + b \cdot r_{yt}$ avec $b > 0$
*   **Calcul du risque :** La formule devient une identité remarquable $(\lambda_x\sigma_x + \lambda_y\sigma_y)^2$.
$$\sigma_p = \lambda_x\sigma_x + \lambda_y\sigma_y$$
![[Pasted image 20260105185757.png|600]]
> **Note :** Ici, le risque est une simple moyenne pondérée. Il n'y a **aucun effet de diversification**. Le risque est maximum.

---

##### 2. Corrélation nulle ($\rho_{xy} = 0$)
Les titres sont indépendants (ou non reliés linéairement).
*   **Relation :** $r_x$ et $r_y$ ne sont pas reliés.
*   **Calcul du risque :** Le terme de covariance s'annule.
$$\sigma_p = \sqrt{\lambda_x^2\sigma_x^2 + \lambda_y^2\sigma_y^2}$$
![[Pasted image 20260105185725.png|600]]
> **Note :** Le risque du portefeuille est **inférieur** à la moyenne pondérée des risques. La diversification commence à être efficace.

---
##### 3. Corrélation négative parfaite ($\rho_{xy} = -1$)
Les titres évoluent en sens opposés.
*   **Relation :** $r_{xt} = a + b \cdot r_{yt}$ avec $b < 0$
*   **Calcul du risque :** La formule devient l'identité remarquable $(\lambda_x\sigma_x - \lambda_y\sigma_y)^2$.
$$\sigma_p = |\lambda_x\sigma_x - \lambda_y\sigma_y|$$
![[Pasted image 20260105185627.png|600]]

> **Note :** C'est le cas optimal. Il est possible d'annuler totalement le risque ($\sigma_p = 0$) en choisissant les proportions telles que $\lambda_x\sigma_x = \lambda_y\sigma_y$.


---

####  calculs avancee: 
**1. Calcul des Rendements (Slide 40) :**
Le cours utilise le **rendement continu** (Log-rendement) : $R_c = \ln(P_t / P_{t-1})$.
*   *Exemple (Janv-97) :* Prix de déc-96 = 740, prix de janv-97 = 870.
*   Calcul : $\ln(870 / 740) \approx 0,1618$ soit **16,18%**.

**2. Annualisation  :**
Comme les données sont mensuelles, il faut les transformer pour une année :
*   **Rendement annuel** $\approx$ Rendement mensuel $\times 12$.
*   **Risque annuel (Écart-type)** = Écart-type mensuel $\times \sqrt{12}$.
*(On multiplie par la racine carrée du temps, c'est une règle standard en finance).*

**3. Corrélation et Covariance :**
Le tableau montre comment on calcule la **Covariance**. On regarde pour chaque mois si les deux titres ont dévié de leur moyenne dans le même sens :
*   Si le Titre 1 monte plus que sa moyenne ET le Titre 2 aussi $\rightarrow$ Produit positif.
*   La moyenne de ces produits donne la **Covariance (0,00464)**.
*   En divisant cette covariance par les écarts-types des deux titres, on obtient la **Corrélation (0,2709)**.

# Portefeuille efficient
> le meilleur mélange possible entre deux actifs.

- **Objectif :**
	- min⁡ var(rp): On veut trouver les proportions (`λ`) qui donnent le **risque le plus bas**.
    
- **Sous contraintes (S/C) :**
    1. On veut un rendement précis (`μ`).
    2. La somme des proportions doit être égale à 1 (100% de votre argent).

