- valorisation: quelle est lea valeur actuelle de ce stock?
la meme quantité ms pas la meme valeur(FIFO...)
- gestion les coûts de stock: la quantité minimale a avoir dans un stock...
- les coûts de stock: 
	1. Coût de passation de la commande(coût indirecte): le coût total des operations indirection liées/quantité maximal
	2. coût de possession(direct): mdov directement lie a la production/quantité moyenne du stock
	`quantite_moyenne du stock= quantite_max/2`
	3. coût de rupture(des aléas): perte d'un client fidèle- retard du fournisseur... imprévues
	**solution:** stock de sécurité qui va rendre le coût des alias a zero donc le coût total va se diviser en deux coûts: de passation et de possession.
	$$Stock moyen=quantite commande/2$$
- Le nombre optimal de commandes a faire est:
$$
\sqrt{\frac{\left(quantiteAnnuelle\ \cdot\ prixUnitaire\cdot\tau aux_{possession}\right)}{2\cdot cout_{passsation}}}
$$
- La quantite optimal a commander:
$$
\frac{quantiteAnnuelle}{nombreCommande}
$$

---

#### 1ere approche: Achat & approvisionnement a quantité cts
- Q*: lot économique: quantité optimal a commande
optimal<=> minimise les coûts
$$
Q*=\sqrt(\frac{2cf}{pt})$$
$$C: demande annuelle(consommation annuelle)les besoins annuelles de l'entreprise; annuelle demande
f:Co: coût de passation
p: prix unitaire
t: taux de possession
p*t= coût de possession(carrying cost)
$$

=> elle est optimal a commande car:
![[Pasted image 20250926092208.png]]


#### 2eme approche: Achat & approvisionnement a intervalle constant
$$
\text{cadence}=\frac{12 \text{mois}}{\text{nombre de commandes}}
$$
$$
k=\sqrt{2cfpt} \quad \text{ou} \quad k=\sqrt{2DC_oC_h}
$$
- estimation du stock de sécurité:
	- Si les conditions sont stables, je revient au jour avec le plus de commandes de l'année dernière et c'est ca le stock min
- Lorsque la livraison est variable:
$$
SS= ZD\sigma_\text{livraison}
$$
SS=Z*\Ect*\sqrt(L)
Z=1,65
Ect: ecart type de l'historique
L: délai de livraison
NB: homogénéise les 

- Si les demandes et le délai du fournisseur varient:
$$
SS= Z \frac{\sqrt{(D \sigma_L)^2 + \sigma_D}}{2}
$$
NB: SS: stock de securite
---
- Si la quantité restante en stock < stock de sécurité => reception de la commande