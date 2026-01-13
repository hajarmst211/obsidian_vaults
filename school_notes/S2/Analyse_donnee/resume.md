- Correlation:
	- **`r=1`**  : Perfect positive correlation (they increase together).
	- **`r=−1`** : Perfect negative correlation (as one goes up, the other goes down).
	- **`r=0`**: No linear relationship at all.
- ACP normée: variables centrées réduites $E_{Xi}$ = 0 et $V_{Xi}=1$
#### Tester globalement le model de la regression linéaire:
we look at three lines of the `Summary(reg)`
1. `F-statistic: 3.167 on 1 and 8 DF, p-value: 0.113` 
if p-value > 0.05: le model n'est pas significatif
2. `Multiple R-squared:`
explique seulement sa valeur de la variation des notes
the higher the better

# après la suppression de la valeur atypique:
#### Tester la significativité des paramétres du model
- comparer les valeurs de la p_value note Pr(>|t|) pour chaque parametre et aussi pour l'intercept
![[Pasted image 20260106145133.png|400]]

Pour valider ton modèle de régression linéaire après la suppression de la valeur atypique

### 1. La Normalité des résidus
*   **Graphique à regarder :** **Figure (1,2) "Normal Q-Q"**.
*   **Interprétation :** Sur ce graphique, les points représentent les résidus de ton modèle. Pour que l'hypothèse de normalité soit validée, les points doivent être alignés le long de la droite pointillée diagonale.

### 2. L’Homoscédasticité (dispersion constante)
*   **Graphique à regarder :** **Figure (2,1) "Scale-Location"** (et aussi le graphique 1,1).
*   **Interprétation :** 
	* On vérifie si la dispersion des points est constante le long de la ligne rouge. Si la ligne rouge est à peu près horizontale et que les points ne forment pas un "entonnoir" ou Il n'y a pas de motif de "vague" (une dispersion qui s'élargit), la variance est constante.
	* le graphe de la figure (1,1) Le nuage des résidus ne forme aucune forme régulière dons l’homoscédasticité est vérifiée

### 3. L’Autocorrélation (Indépendance des résidus)
*   **Graphique à regarder :** **Figure (1,1) "Residuals vs Fitted"**.
*   **Interprétation :** On cherche à voir s'il existe une structure ou un "motif" (une forme de U, une courbe, etc.) dans la répartition des points. Si les points sont répartis de manière aléatoire autour de la ligne horizontale zéro, les résidus sont indépendants.

