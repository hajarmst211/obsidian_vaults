
### 1. Le rôle des Hooks
Un pipeline de données n'est jamais isolé ; il doit communiquer avec des bases de données, des API, du stockage objet (S3, GCS), etc.
*   **Définition :** Un Hook est une interface Python qui encapsule la connexion à un système externe.
*   **Avantages :** Il gère pour vous l'authentification, la connexion réseau (sessions, pools) et fournit des méthodes métier (`run`, `get`, `insert`) pour interagir avec la cible.

---

### 2. Architecture en trois couches
Pour rendre les DAGs maintenables, portables et sécurisés, le cours préconise une séparation nette :
1.  **Operator (Le "Quoi") :** Définit la logique métier du DAG. Il utilise un Hook pour exécuter l'action.
2.  **Hook (Le "Comment") :** L'interface technique qui permet de communiquer avec le système externe. Ils sont réutilisables.
3.  **Connection (Le "Où") :** Stocke la configuration (host, login, password) de manière centralisée. Elle est gérée soit via l'UI, soit via le CLI, soit via des variables d'environnement.

---

### 3. Gestion des configurations
*   **Connections :** Trois méthodes de création sont possibles :
    *   **UI :** Idéal pour la découverte.
    *   **CLI :** Idéal pour le scripting/CI-CD.
    *   **Variable d'environnement :** Recommandé pour la production (ex: `AIRFLOW_CONN_MY_POSTGRES`).
*   **Variables :** Permettent de stocker des paramètres de configuration globaux (clés/valeurs). *Note : Pour les secrets en production, utilisez des Secret Backends (Vault, AWS Secrets Manager, etc.).*

---

### 4. Partage de données : XCom
*   **Usage :** Les XComs (Cross-Communication) permettent de faire circuler de petites quantités de données entre les tâches.
*   **Intégration TaskFlow :** La communication est implicite avec l'API TaskFlow ; tout `return` d'une fonction `@task` crée automatiquement un XCom.
*   **Limites :** XCom est conçu pour les métadonnées (Ko). Pour des volumes > 1 Mo, il faut passer par un **Custom Backend** (S3, GCS) pour stocker le fichier et ne transmettre que le chemin.

---

### 5. Bonnes pratiques (Do's & Don'ts)
*   **À faire :**
    *   Centraliser les credentials dans les Connections (jamais en dur dans le code).
    *   Utiliser des `conn_id` paramétrés.
    *   Préférer les méthodes spécialisées (`get_pandas_df`, `get_records`) plutôt que des requêtes brutes.
    *   Logger les volumes manipulés.
*   **À éviter :**
    *   Stocker des mots de passe en clair dans une Variable.
    *   Passer des DataFrames massifs (ex: 500 Mo) via XCom.
    *   Hardcoder des hostnames (utiliser les connexions).
    *   Ignorer la gestion des retries sur les appels réseau.

---

### 6. Atelier pratique (Résumé)
Le cours propose de construire un pipeline complet en 4 étapes :
1.  **Récupération :** API publique (`fetch_users` via `HttpHook`).
2.  **Stockage :** Upload du JSON sur MinIO (`S3Hook`).
3.  **Chargement :** Insertion dans PostgreSQL (`PostgresHook.insert_rows`).
4.  **Notification :** Envoi d'une alerte via Slack (`SlackWebhookHook`).

**Conclusion :** La maîtrise de la séparation *Operator/Hook/Connection* est la clé pour concevoir des pipelines professionnels, portables et robustes dans Airflow.