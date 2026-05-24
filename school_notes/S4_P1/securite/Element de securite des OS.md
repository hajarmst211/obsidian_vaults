Une capabilité représente: Un certain type d'accès à un objet

### 2. Fonctions de base et de sécurité
*   **Fonctions de base :** Gestion de la mémoire, du processeur (CPU), des périphériques et des services de communication.
*   **Fonctions de sécurité :** L'OS doit assurer le contrôle d'accès sur toutes ces ressources pour éviter qu'un processus ne compromette la sécurité globale (ex: empêcher un processus autorisé de transmettre des données en clair vers le réseau).

### 3. Concepts fondamentaux (CC, TCB et Moniteur)
*   **Critères Communs (CC) :** Standard international permettant d’évaluer <mark style="background: #FFB86CA6;">l’efficacité des contrôles de sécurité</mark> d’un produit (ex: OS). Les produits sont évalués selon les 7 niveaux d'assurance (EAL).
* <mark style="background: #BBFABBA6;">Concepts :</mark>
	• CEM : méthodologie évaluation système
	• TCB (Trusted computing Base) : réduire sa taille améliore la sécurité
	• PP : document des exigences pour une communauté d'utilisateurs dans un but particulier
	• ST (Security Target) : doc des exigences qu’on veut évaluer
	
*   **TCB (Trusted Computing Base) :** L'ensemble des composants critiques (noyau, matériel, fichiers de configuration) sur lesquels repose la sécurité. Plus sa "taille" est réduite, plus le système est sécurisé.
#### Moniteur de Référence 
C'est le composant qui contrôle <mark style="background: #FFB8EBA6;">les accès aux objet SE</mark>. Il doit être :
*   **Inviolable** (tamper-proof) ;
*   **Incontournable** (médiation complète) ;
*   **Vérifiable**.

- **Sandboxing (bac à sable) :**
- Le **Sandboxing**  est une technique de sécurité informatique qui consiste à isoler un programme ou un processus dans un environnement restreint et fermé.
- Objectifs:
	• Limiter l’environnement d’exécution d’un code suspect
	• Limiter les accès aux ressources système
	• Limiter les accès aux données des autres utilisateurs

### 4. Exigences de sécurité de base pour un OS
Pour qu'un OS soit considéré comme sécurisé, il doit remplir les conditions suivantes :
*   **Identification et authentification** robuste des utilisateurs.
*   **Gestion des accès (ACL)** pour appliquer une politique de sécurité (DAC, MAC).
*   **Protection de la mémoire** (via des mécanismes matériels comme la segmentation ou la pagination).
*   **Gestion du partage** (contrôle d’intégrité pour éviter les conflits).
*   **Contrôle des communications inter-processus**.

### 5. Solutions complémentaires
*   **Audit et traçabilité :** Enregistrer les activités importantes (logs) comme les échecs de connexion ou les modifications de droits, tout en protégeant l'intégrité de ces journaux (stockage sur support non réinscriptible).
*   **Sandboxing (Isolation) :** Isoler l’exécution d'un code suspect dans un environnement restreint ("bac à sable") pour limiter ses privilèges et empêcher tout impact sur le système ou les données utilisateur.