
### **1. Structure d'un projet Airflow**
Pour maintenir un environnement Airflow propre et organisé, le projet doit inclure les dossiers suivants :
*   **`config/`** : Pour les fichiers de configuration.
*   **`dags/`** : Contient le code Python définissant les workflows.
*   **`logs/`** : Stocke les journaux d'exécution des tâches.
*   **`plugins/`** : Pour les extensions personnalisées.
*   **`docker-compose.yml`** : Fichier essentiel pour orchestrer le démarrage des conteneurs.

---

### **2. Création de DAGs et Tasks**
*   **DAG (Directed Acyclic Graph) :** C'est l'unité de base qui définit le workflow. Il nécessite des paramètres par défaut (`default_args`) comme le propriétaire, la date de début (`start_date`), et la fréquence d'exécution (`schedule_interval`).
*   **Tasks et Opérateurs :** Une tâche est une instance d'un opérateur. Les types d'opérateurs courants incluent :
    *   `BashOperator` : Exécute des commandes Shell.
    *   `PythonOperator` : Appelle des fonctions Python.
    *   `EmailOperator` : Gère l'envoi d'emails.
    *   *Note : Il existe une vaste bibliothèque de "community providers" (MySQL, Postgres, Docker, etc.) pour étendre les capacités d'Airflow.*
*   **Dépendances :** Elles définissent l'ordre d'exécution (par exemple : `first_task >> [second_task, third_task]`).

---

### **3. Gestion des données : Variables vs XComs**
Le cours établit une distinction cruciale entre deux mécanismes de gestion de données :

#### **A. Variables**
*   **Usage :** Stockage de valeurs globales et dynamiques (ex: chemins de fichiers, clés d'API, paramètres de configuration).
*   **Avantage :** Permet d'éviter le "hardcoding" (codage en dur) dans le code.
*   **Accès :** Via l'interface Web (`Admin -> Variables`), CLI, ou directement dans le code Python (`Variable.get("clé")`).
*   **Limitation :** Ne pas utiliser pour des mises à jour à haute fréquence.

#### **B. XComs (Cross-Communications)**
*   **Usage :** Échange de messages ou de petites quantités de données entre deux tâches au sein d'une même exécution de DAG.
*   **Fonctionnement :** Utilisation des méthodes `xcom_push` et `xcom_pull`.
*   **Bonnes pratiques :** 
    *   Destiné aux petits payloads uniquement (ne pas passer de DataFrames massifs).
    *   Ne pas stocker d'informations sensibles (préférer les *Airflow Connections*).
    *   Beaucoup d'opérateurs poussent automatiquement le résultat de la tâche sous la clé `return_value`.

---

### **Comparaison clé**
| Caractéristique | Variables | XComs |
| :--- | :--- | :--- |
| **Portée** | Globale (entre DAGs/tâches) | Par instance de tâche (au sein d'un DAG) |
| **Objectif** | Configuration globale | Communication entre tâches |

---

 **Sensor**: a specialized type of operator whose primary purpose is to wait for an event or condition to occur (such as a file appearing in an S3 bucket, a partition being created in a database, or a specific time of day). It continuously checks ("pokes") the target resource at defined intervals until the condition is met (returns True), allowing downstream dependent tasks to run.