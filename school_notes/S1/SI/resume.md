
1. MFD: matrice de flux de donnees
2. DFD: diagramme de flux de donnees
3. GOE: graphe organisationnelle des evenements
4. GDD: graphe de dependence des documents/donnees
5. MCT: modele conceptuelle des traitements
6. VED: vue externe des donnees
7. MCD: modele conceptuelle de donnees
8.  MOT: modele organisationnel de traitement
---
- **GDE** : dépendances entre les événements (logique et ordre).
- **GOE** : ajoute les acteurs à ces événements (qui est concerné).
- **MCT** : décrit les traitements déclenchés par les événements
---

### MFD: matrice de flux de donnees
| Source / Destination | Processus A | Processus B     | Base X      | Client |
| -------------------- | ----------- | --------------- | ----------- | ------ |
| **Processus A**      | —           | Bon de commande | —           | —      |
| **Processus B**      | Réponse     | —               | Mise à jour | —      |
| **Base X**           | Lecture     | Écriture        | —           | —      |

---
#### GFD: graphe de flux des documents
- Ressources mémorisées : toute information qui existait avant la transaction susceptible de
changer mais qui est obligatoire au bon déroulement de la transaction.
- Il faut éliminer les flèches de CATA et FCLI.( les ressources memorises)
![[Pasted image 20251028160922.png]]

DCD:

---
#### MOT
### 1. Sens de “toujours” dans le MOT

- **Toujours** signifie que **l’action ou le traitement se produit systématiquement**, à **chaque occurrence de la transaction ou de l’événement déclencheur**.