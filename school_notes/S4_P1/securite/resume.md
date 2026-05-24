# Sécurité

- **Sécurité informatique** $\rightarrow$ **Sécurité des systèmes d'information (SSI)** $\rightarrow$ **Sécurité de l'information**

## Les trois services de sécurité clés (CIA)
- Confidentialité
- Intégrité(la garantie que les données n'ont pas été altérées, modifiées ou détruites)
- Disponibilité
*Note* : La norme ISO/IEC définit ces trois fondements mais reconnaît que d'autres peuvent s'ajouter selon le contexte.

## Protection des SI
L'accent est mis sur la protection, mais il ne faut pas négliger les mesures de **détection** et de **réaction**.

### Pourquoi les SI sont-ils importants ?
- Ils sont le cœur des organismes.
- Ils sont souvent connectés à Internet, sans frontière physique.

### Motivations pour attaquer un SI
- Profit financier
- Satisfaction personnelle
- Vengeance
- Conviction politique ou idéologique
- Sabotage

### Pourquoi sécuriser ?
- Les pertes peuvent être très graves.
- Les statistiques montrent que les attaques augmentent et deviennent plus efficaces.
- La cybercriminalité n'est pas encore totalement cernée juridiquement.

### Type de menaces:
- Selon l'action:
	- Menaces passives
	- Menaces actives
- Selon l'origine:
	- Menace interne
	- Menace externe
- Selon la cible:
	- menace sur la confidentialite
	- menace sur l'integrite
	- menace sur la disponibilite
## L'approche PDCA (plan, do, compare, act):
- Méthode de gestion de la sécurité
Le PDCA consiste à **"Planifier la sécurité, déployer les outils, vérifier qu'ils protègent bien, et corriger les failles"**, en répétant ce cycle indéfiniment.
1. **Planifier :** Déterminer les ressources, les procédures et les responsabilités pour mettre en place la politique de sécurité
2. **Faire :** mettre en œuvre ce qui a été planifié à l'étape précédente
3. **Comparer/Contrôler :** Une fois les mesures appliquées, il faut s'assurer qu'elles sont efficaces et conformes aux objectifs fixés.
4. **Agir :** Apprentissage à partir de l'action pour s'améliorer.

## Politique de sécurité (PS)
- La PS est la structure autour de laquelle tout le SI et le **document de référence** qui définit l'engagement d'une organisation envers la sécurité; elle doit être la base du projet.
- **Politique de contrôle d'accès :** Elle doit définir les règles d'accès acceptables au SI.

### Points à traiter (selon ISO 27001) :
1. Diagnostic de l'existant.
2. Analyse des risques.
3. Définition des rôles (qui est responsable de quoi).
4. Estimation du budget 
5. Procédures de protection.
6. Procédures de détection.
7. Procédures de réaction 
8. Procédures de redressement 
	->(réaction à court/moyen terme).

## Terminologie et Definition:
- **Vulnérabilité :** Faiblesse pouvant être exploitée pour entraîner un impact négatif sur un actif.
- **Menace :** Action pouvant causer un dommage à un actif ou violer la politique de sécurité (CIA).
- **Attaque :** Instance spécifique d'une menace.
- **Risque :** Estimation de la probabilité qu'une menace exploite une vulnérabilité.
  $$\text{Risque} = \text{Impact} \times \text{Probabilité}$$
- **Mesure de sécurité :** Action réduisant le risque ou l'impact d'une attaque.
- **Vecteur d'attaque :** point d'entrée d'une attaque.
- **Surface d'attaque :** Ensemble des points d'entrée/sortie qu'un attaquant peut exploiter.
- **Facteur de travail (Work factor) :** Temps et effort nécessaires pour exploiter un vecteur.

### Evaluation des menaces (STRIDE)(quoi)
- **S-Spoofing (Usurpation d’identité) :** Protégé par l'**Authentification**.
- **T-Tampering (Altération) :** Modification non autorisée de données
	->Protégé par le **Hashage/Signature**.
- **R-Répudiation :** Un utilisateur effectue une action et nie l'avoir faite, alors que le système ne possède pas de preuve suffisante pour prouver le contraire.
	->Protégé par les **Logs/Signatures**.
- **I-Information Disclosure :** Accès non autorisé à des données privées ou sensibles
	->Protégé par le **Cryptage**.
- **D-Denial of Service (Déni de service) :** Rendre un système indisponible pour ses utilisateurs légitimes
	->Protégé par le **Backup**.
- **E-Elevation of Privilege (Élévation de privilège) :** Un utilisateur normal réussit à obtenir les droits d'un administrateur ou d'un utilisateur de niveau supérieur.
	->Protégé par le **Contrôle d'accès (Least Privilege)**.

### Évaluation des risques : DREAD(combien)
- D-**damage**
- R-**Reproductibilité**
- E-**Exploitabilité**
- A-**Utilisateurs affectés**
- D-**Découvrabilité**
$$\text{Risque} = \frac{(\text{D}amage + \text{R}eproducibility + \text{E}xploitability + \text{A}ffected\_users + \text{D}iscoverability)}{5}$$

---

# Services et Mécanismes de sécurité

## Les services principaux
- <mark style="background: #FFB86CA6;">Confidentialité</mark> : propriété selon laquelle l’information n’est pas rendue accessible/divulguée.
	⇒ protéger les infos sensibles
-  <mark style="background: #FFB86CA6;">Intégrité</mark> : propriété de protection de l’exactitude des actifs
	⇒ aucune modification ou altération
-  <mark style="background: #FFB86CA6;">Authentification</mark> : assure que l’entité distante avec laquelle communique est bien celle qu’elle prétend représenter
	⇒ authenticité de l’identité
- <mark style="background: #FFB86CA6;">Non-répudiation</mark> : assurance que le message appartient à une personne par signature
- <mark style="background: #FFB86CA6;">Contrôle d’accès</mark> : protéger les ressources d’accès non autorisées
	⇒ intégrité + confidentialité
- <mark style="background: #FFB86CA6;">Disponibilité</mark> : être accessible et utilisable à la demande par une entité autorisée
	⇒ protège contre Denial of Service
- <mark style="background: #FFB86CA6;">Imputabilité</mark> : tentatives d’accès sont tracées
	⇒ protège contre répudiation

## Mécanismes cryptographiques
- **Cryptographie :** Transformation des données pour cacher leur contenu.
- **Cryptanalyse :** Étude des méthodes cryptographiques pour en trouver les faiblesses.
- **Principe de Kerckhoffs :** La sécurité repose sur le **secret de la clé** et non de l'algorithme.

### Cryptage Asymétrique (RSA)
- Utilise une paire de clés (**publique** pour chiffrer, **privée** pour déchiffrer).
- Impossible de retrouver la clé privée à partir de la publique.
- Pour assurer l’<mark style="background: #FFB86CA6;">authentification</mark>, <mark style="background: #FFB86CA6;">sécuriser les échanges
de clé</mark>
- permet d'eviter la nn-repudiation

| Si l'objectif est... | On utilise la clé... | De qui ? |
| :--- | :--- | :--- |
| **Confidentialité** (Cacher) | Publique | du **Destinataire** |
| **Signature** (Prouver) | Privée | de **l'Expéditeur** |

### Cryptage Symétrique (AES, DES)
- Utilise la même clé pour chiffrer et déchiffrer.
- Beaucoup plus <mark style="background: #FFB86CA6;">rapide</mark> que l'asymétrique (utilisé pour les gros volumes de données).
- <mark style="background: #FFB86CA6;">Grand volume</mark> de données car il est **rapide**.

### Fonction de Hashage
- Doit être à sens unique, de taille fixe, et résistant aux collisions.
- **Résistance à la pré-image 1:** Il est impossible de retrouver le message original à partir de son empreinte
- **la pré-image 1:** on ne peut pas trouver un autre message qui produit la même empreinte qu'un message donné
- **Résistance à la collision :** Impossible de trouver deux messages avec le même hash.

---

# Signature numérique et Certificats
C'est un <mark style="background: #FFF3A3A6;">mécanisme cryptographique</mark> qui permet de garantir l'authenticité d'un document numérique

- **Signature électronique (juridique) :** Tout procédé (clic, scan, etc.). <mark style="background: #FFF3A3A6;">Les 3 niveaux:</mark> signature électronique simple- signature électronique avancée - signature électronique qualifiée.   
- **Signature digitale (la technologie) :** Basée sur la cryptographie (hash + clé privée).
- **Certificat numérique :** "Carte d'identité" contenant la clé publique et signée par une autorité de certification (CA).
- **Chaîne de confiance :** Basée sur le "Trust Anchor" (Ancre de confiance).

---

# Mécanisme d'authentification
- **Niveaux :** (0) Identification, (1) Connaissance (mot de passe), (2) Possession (carte/token), (3) Caractéristique physique (biométrie).
- **Mots de passe :** Hachage avec **sel** et utilisation d'algorithmes comme **Argon2** ou **bcrypt**.
- **FIDO2 / WebAuthn :** Authentification sans mot de passe via cryptographie asymétrique (la clé privée reste sur l'appareil).

---

# Mécanisme de contrôle d'accès
- **Modèle de Lampson :** Sujet, Objet, Action.
- **ACL (Access Control List) :** Vue par colonne (qui accède à quel objet ?).
- **Capabilités :** Vue par ligne (à quels objets le sujet a-t-il accès ?).
- **Sandboxing :** Isolation de l'exécution pour limiter les droits.