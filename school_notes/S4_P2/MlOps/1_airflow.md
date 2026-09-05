
### **1. Qu'est-ce qu'Airflow et pourquoi l'utiliser ?**
*   **Définition :** C'est une plateforme d'orchestration qui permet d'exécuter des séries de tâches de<mark style="background: #FFF3A3A6;"> manière séquentielle ou parallèle </mark>pour atteindre un objectif spécifique.
*   **Pourquoi l'utiliser :**
    *   **Orchestration des flux de données :** Connecter différents outils (extraction, transformation, chargement, analyse).
    *   **Intégration :** Système de plugins riche pour interagir avec des outils comme Spark, AWS, Google Cloud, etc.
    *   **Workflows dynamiques :** Possibilité de générer des tâches dynamiquement en fonction des résultats de tâches précédentes.
    *   **Monitoring et Data Lineage :** Suivi précis de l'origine et des transformations des données dans le temps.
    *   **Communauté :** Support actif, nombreuses bibliothèques et connecteurs disponibles.

---

### **2. Architecture et Composants**
Airflow repose sur plusieurs composants fondamentaux :
*   **Scheduler :** Le cerveau qui planifie et orchestre les tâches (DAGs).
*   **Web Server :** L'interface utilisateur pour gérer, suivre les états d'exécution et consulter les logs.
*   **Meta Database :** Base de données stockant l'état des tâches et les métadonnées.
*   **Executor & Workers :** Les composants responsables de l'exécution réelle des tâches.

---

### **3. Évolution vers Airflow 3.2.0**
Le cours souligne l'évolution vers la version 3.2.0 (sortie mentionnée : 7 avril 2026), avec des nouveautés majeures :
*   **Asset Partitioning (AIP-76) :** Permet une orchestration ultra-granulaire basée sur des partitions de données.
*   **Multi-Team Deployments :** Fonctionnalité expérimentale permettant une isolation complète par équipe au sein d'un même cluster.
*   **Deadline Alerts :** Alertes synchrones pour la gestion des délais.
*   **Progression Task SDK :** Meilleure séparation entre les auteurs de DAGs et les opérations (Ops).
*   **Nouvelle Interface UI :** Refonte avec de nouvelles fonctionnalités de monitoring.

---

### **4. Installation et Configuration**
*   **Recommandation :** Utilisation de **Docker** pour le déploiement.
*   **Points clés :** Configuration du fichier `airflow.cfg` et gestion des variables d'environnement.
*   **Multi-Team :** Activation via `multi_team = True`.
*   **Déploiement type :** Stack incluant PostgreSQL (base de données), Redis (pour le queuing) et le conteneur Airflow officiel (`apache/airflow:3.2.0`).

---

### **5. Concepts de Développement (TP)**
*   **TaskFlow API :** Utilisation de la syntaxe moderne `@task` recommandée en 3.x.
*   **Structure :** Distinction entre **DAG** (le workflow global), **TaskGroup** (groupement visuel de tâches) et **Asset** (dépendances basées sur les données).
*   **Cas pratique :** Scraping de données YouTube et traitement associé.

---

### **6. Quand utiliser Airflow ?**
*   Automatisation de listes de tâches récurrentes.
*   Enchaînement de modèles d'IA/ML (traduction, extraction d'entités, recommandations).
*   Déploiement et orchestration de modèles de Machine Learning.
*   Intégration dans des architectures **Event-Driven** (Microservices, Kafka).

---

### **7. Interface et Monitoring**
L'interface Web permet de visualiser :
*   L'état de santé du système (Base de données, Planificateur, Déclencheur).
*   Le statut des DAGs (Échoués, en cours, actifs).
*   L'historique des exécutions et la gestion des Assets/Partitions.
*   L'utilisation de la **CLI** et des nouveaux **endpoints API** d'Airflow 3.

--- 
*Note : Le cours se termine par un TP pratique axé sur l'installation de Docker Desktop, l'intégration avec VS Code et la création complète d'une image/conteneur Airflow.*