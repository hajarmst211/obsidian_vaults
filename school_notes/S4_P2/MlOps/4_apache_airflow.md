### 1. Synthèse : Quel mécanisme pour quel usage ?
Le choix repose sur une question simple : **« Qu'est-ce qui provoque le changement de la donnée ? »**

| Mécanisme     | Rôle principal             | Portée              | Quand l'utiliser ?                                                                        |
| :------------ | :------------------------- | :------------------ | :---------------------------------------------------------------------------------------- |
| **Assets**    | Lien de données entre DAGs | Inter-DAGs          | Lorsqu'un DAG produit une donnée et qu'un autre doit s'exécuter après sa matérialisation. |
| **XCom**      | Communication entre tâches | Intra-DAG run       | Pour transmettre de petites valeurs (JSON) entre tâches d'une même exécution.             |
| **Variables** | Configuration globale      | Globale (tous DAGs) | Pour des paramètres qui changent selon l'environnement (prod/dev/staging).                |

---

### 2. Focus sur les trois piliers

#### **A. Les Assets (Le chaînage automatique)**
*   **Concept :** Un jeu de données identifié par un URI unique (ex: `s3://bucket/data.parquet`).
*   **Fonctionnement :** 
    *   **Producteur :** Déclare `outlets=[asset]` pour enregistrer la matérialisation.
    *   **Consommateur :** Déclare `schedule=[asset]` pour se déclencher automatiquement.
*   **Avantage :** Supprime le besoin de `Sensors` ou de `TriggerDagRunOperator` et génère un lignage (lineage) automatique dans l'UI.

#### **B. XCom (Le transfert d'état entre tâches)**
*   **Concept :** "Cross-communication", paire clé/valeur stockée dans la base de métadonnées.
*   **Usage :** Push via `return` (TaskFlow) ou `xcom_push` ; Pull via paramètre ou `xcom_pull`.
*   **Contrainte critique :** XCom est fait pour les **métadonnées (Ko)**, jamais pour les données lourdes. Si le volume dépasse ~100 Ko (ex: DataFrame), il faut stocker le fichier sur un disque ou un S3 et ne passer que le chemin via XCom.

#### **C. Variables (La configuration externe)**
*   **Concept :** Paires clé/valeur globales, modifiables à chaud via l'UI, le CLI, le code Python ou Jinja.
*   **Anti-pattern important :** Ne **JAMAIS** appeler `Variable.get()` au niveau du module (en haut du fichier DAG), car cela est exécuté à chaque parsing (environ toutes les 30 secondes). Il faut toujours appeler la lecture **à l'intérieur d'une tâche**.

---

### 3. Autres mécanismes mentionnés
Pour compléter le spectre de gestion d'état, le cours rappelle deux outils supplémentaires :
*   **Connections :** Pour les accès systèmes (host, port, login, password).
*   **Secrets Backend :** Pour la gestion sécurisée des secrets (Vault, AWS Secrets Manager, GCP Secret Manager) permettant une rotation périodique des clés.

### **La règle d'or pour décider :**
1.  Si la valeur dépend de l'**environnement** → **Variable**.
2.  Si elle dépend de l'**exécution** (run) → **XCom**.
3.  Si elle est produite par un **DAG** et consommée par un autre → **Asset**.