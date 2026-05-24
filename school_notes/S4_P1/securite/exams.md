# Exam 2024
### **Exercice 1 : Questions de compréhension du cours:

La direction générale d'une entreprise travaillant sur des projets sensibles a l'habitude de communiquer par email à ses fournisseurs et partenaires (et vice-versa) des documents critiques sur les commandes à passer et les projets à mener. Il est donc primordial d'assurer la sécurité de tels échanges.

**a-** Quels sont les 2 services de sécurité que vous considérez les plus essentiels à assurer pour cette entreprise ? (Justifiez votre réponse)

**b-** Identifiez les 3 principales menaces (liées à la communication par email de documents sensibles) impactant ces 2 services (de votre réponse à la question -a-). Pour chaque menace (selon le modèle STRIDE), proposez une mesure de sécurité adaptée (utilisez ce tableau pour votre réponse) :

| Menace | Mesure de sécurité adaptée | Justification |
| :--- | :--- | :--- |
| | | |
| | | |
| | | |

**c-** En cas d'utilisation de clés cryptographiques, quels types de clés cette entreprise devrait-elle utiliser et quel mécanisme supplémentaire peut-elle utiliser pour renforcer davantage la sécurité de ces échanges ?

### Exercice 2: QCM

**Q1) Quel(s) mécanisme(s) de sécurité ne participent pas à assurer le service de non-répudiation :**
*   **Réponse : a) et b)**
*   *Justification :* La non-répudiation nécessite de lier une action à une identité unique (via signature/certificat). Le chiffrement symétrique et le simple hachage ne permettent pas de prouver l'identité de l'auteur de façon irréfutable.

**Q2) Selon la loi 43-20, la signature électronique avancée est une signature électronique qualifiée qui repose en plus sur un certificat qualifié :**
*   **Réponse : b) Faux**
*   *Justification :* Dans la hiérarchie juridique, la signature électronique **qualifiée** est le niveau supérieur qui repose sur un certificat qualifié. La signature **avancée** est un niveau intermédiaire, elle ne nécessite pas obligatoirement un certificat qualifié.

**Q3) Lorsque « Trouver X et Y, tels que H(Y)=H(X) et Y≠X » est très difficile, on parle de :**
*   **Réponse : b) Résistance à la seconde pré-image**
*   *Justification :* Comme expliqué précédemment, on part d'une valeur X connue (imposée) et on cherche une autre valeur Y pour créer une collision.

**Q4) Le chiffrement post-quantique :**
*   **Réponse : b) Vise à résister aux attaques utilisant l'ordinateur quantique**
*   *Justification :* Le chiffrement post-quantique utilise des algorithmes classiques (mathématiques) conçus pour être résistants même face à la puissance de calcul d'un ordinateur quantique futur. Il n'utilise pas la physique quantique lui-même.

**Q5) Etablir et mettre en place une bonne politique de sécurité nécessite toujours :**
*   **Réponse : a) Une analyse de risque**
*   *Justification :* L'analyse de risque est le socle indispensable (l'étape "Plan" du PDCA). Sans elle, on ne sait pas quoi protéger ni quel niveau de sécurité appliquer. Les autres options ne sont pas "toujours" nécessaires ou sont trop spécifiques.

**Q6) ISO 27002 (la nouvelle version de Février 2022) est un guide de bonnes pratiques déclinées en plusieurs thèmes :**
*   **Réponse : a) Mesures organisationnelles / Mesures sur les personnes / Mesures physiques / Mesures techniques**
*   *Justification :* C'est la structure officielle des 93 contrôles de l'ISO 27002:2022, qui sont répartis dans ces quatre thématiques (auparavant, c'était une structure différente en 14 domaines).

---
### 1. Exercice 1 (Sujet : Echanges d'emails sensibles)
*   **Services de sécurité (a) :** Le cours consacre une section entière aux "Services de Sécurité" (page 19-23), incluant la Confidentialité, l'Intégrité, etc. Vous pouvez justifier le choix de la **Confidentialité** et de l'**Intégrité** (ou Authentification/Non-répudiation) en utilisant les définitions présentes aux pages 20 et 21.
*   **Menaces STRIDE (b) :** La page 16 explique en détail le modèle **STRIDE** (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege). Pour une communication par email, vous pouvez sélectionner :
    *   *Information Disclosure* (divulgation) -> Chiffrement.
    *   *Tampering* (modification) -> Signature/Hash.
    *   *Spoofing* (usurpation) -> Signature/Authentification.
*   **Clés cryptographiques (c) :** Le cours explique la différence entre clés symétriques et asymétriques (page 27 et 29).

### 2. Exercice 2 (QCM sur la sécurité)
*   **Q1 (Non-répudiation) :** Le cours (page 22, 24) indique que le chiffrement (symétrique ou asymétrique seul) ne garantit pas la non-répudiation contrairement à la signature.
*   **Q2 (Loi 43-20) :** La réponse est explicitée à la page 34 sur les niveaux de signature électronique.
*   **Q3 (Résistance aux collisions) :** La définition exacte se trouve à la page 33 ("Résistance aux collisions : X et Y tel que Y≠X et H(Y)=H(X)").
*   **Q4 (Chiffrement post-quantique) :** La définition est présente aux pages 27 et 28.
*   **Q5 (Politique de sécurité) :** Le cours insiste sur l'importance de l'analyse de risque (page 14, 60).
*   **Q6 (ISO 27002) :** La structure des contrôles (catégories) est détaillée dans le schéma de la page 59.
---
### 3. Exercice 1 (Suite - Modèle de sécurité)
*   **Q1 (Cube du NSTISSC) :** Le schéma de la page 55 (Cube du CNSS/NSTISSC) permet de compléter les axes (Confidentialité/Intégrité/Disponibilité, Stockage/Transmission/Traitement, etc.).
*   **Q2 (Intégrité des données vs flux) :** La distinction est clairement faite aux pages 20 et 21 du cours.
*   **Q3 (Protocole A -> B) :** Cette question porte sur l'authentification. Les pages 39-42 expliquent le principe des challenges (principe de challenge/réponse) et les pages 29-30 traitent des algorithmes symétriques et de la gestion des clés.

### 4. Exercice 2 (Suite - QCM)
*   **Q1 (Chiffrement asymétrique) :** Le mécanisme est expliqué en détail page 29 (Alice doit utiliser la clé publique de Bob pour que seul Bob puisse décrypter avec sa clé privée).
*   **Q4 (Capabilité) :** La définition est donnée à la page 45 ("Chaque sujet possède une liste de capabilités").
*   **Q5 (TOE) :** La définition se trouve page 51 ("Target of Evaluation (TOE) qui peut être un software ou hardware...").
*   **Q6 (Résistance aux collisions) :** Identique à la Q3 du premier exercice.

**Conseil :** Pour rédiger vos réponses, appuyez-vous sur les définitions textuelles des pages citées. Le cours est très structuré et contient les réponses littérales aux définitions demandées (par exemple, pour le "Modèle STRIDE" ou les "Fonctions de Hachage").