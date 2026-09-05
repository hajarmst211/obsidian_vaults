
### **1. Construction d'un Pipeline de Données (Airflow)**
Le cours présente un cas pratique d'ingestion de données avec Airflow utilisant un DAG nommé `data_ingestion` composé de deux tâches principales :

*   **`transform_data` (PythonOperator) :**
    *   **Actions :** Fusion des fichiers CSV (`booking.csv`, `client.csv`, `hotel.csv`), transformation et nettoyage des données.
    *   **Logique :** Utilisation de la bibliothèque `pandas` pour effectuer des jointures (merges) sur les clés de référence (ex: `client_id`, `hotel_id`) et renommage des colonnes.
*   **`load_data` (PythonOperator) :**
    *   **Actions :** Connexion à une base de données **SQLite** (`/usr/local/airflow/db/datascience.db`), création de la table `booking_record` et insertion des données traitées provenant du fichier `/processed_data/processed_data.csv`.

**Configuration technique :** 
L'automatisation est gérée par un fichier `docker-compose.yml` incluant des volumes pour les données brutes, les données traitées, les logs et la base de données. Les commandes clés sont `docker-compose up airflow-init` pour l'initialisation et `docker-compose up` pour le déploiement.

---

### **2. Automatisation ETL (Lab 4-2)**
Le cours propose une architecture complète d'automatisation ETL incluant :
*   **Source :** SQL Server.
*   **Processus (Extract, Transform, Load) :** Orchestré par Python et Airflow.
*   **Destination :** PostgreSQL.
*   **Cas d'usage avancé :** Un pipeline intégrant YouTube (Scraping de commentaires), Kafka (flux de messages), un modèle de traitement de langage naturel via **Hugging Face** (Sentiment Analysis), et une étape finale de visualisation.

---

### **3. Introduction à Apache NiFi**
Le cours introduit Apache NiFi comme un outil complémentaire ou alternatif pour la gestion des flux de données.

*   **Définition :** Logiciel open source développé à l'origine par la NSA (transféré à la fondation Apache en 2014) permettant d'automatiser et de visualiser le mouvement de données entre systèmes en temps réel.
*   **Architecture et Composants clés :**
    *   **FlowFile :** Représente l'objet de donnée original avec ses métadonnées attachées.
    *   **Processor :** L'élément actif qui effectue le traitement sur les données.
    *   **Connector :** Le lien entre les processeurs qui définit les files d'attente (queues) et le routage des données.
*   **Avantages :** Contrôle granulaire du mouvement des données, visualisation intuitive du DataFlow et maintenance régulière par la communauté Apache.

---

### **Résumé des points d'évaluation (Lab)**
Pour réussir les travaux pratiques, l'étudiant doit démontrer sa capacité à :
1.  Créer un **conteneur Docker** le plus léger possible.
2.  Automatiser un **pipeline ETL complet**.
3.  Utiliser des modèles pertinents (ex: via **Hugging Face** pour l'analyse de sentiment).

Ce cours fait la transition entre l'orchestration de tâches basées sur le code (Airflow) et la gestion visuelle de flux de données (NiFi).