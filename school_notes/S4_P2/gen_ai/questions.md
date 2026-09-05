Voici un ensemble de questions à choix multiples (QCM) basées sur l'intégralité des documents fournis (Airflow : Design Patterns, Monitoring, Intégrations, et n8n).

---

### **Partie 1 : Airflow - Principes et Design Patterns**

**1. Qu'est-ce qu'une tâche idempotente dans Airflow ?**
A) Une tâche qui s'exécute toujours en moins d'une minute.
B) Une tâche qui produit le même résultat quel que soit le nombre de fois qu'elle est jouée.
C) Une tâche qui ne nécessite aucune connexion externe.
D) Une tâche qui est automatiquement relancée en cas d'erreur.
*Réponse : B*

**2. Quel est l'intérêt de diviser un pipeline en plusieurs petites tâches plutôt qu'un monolithe ?**
A) Pour réduire le nombre de fichiers dans le dossier `/dags`.
B) Pour augmenter la consommation de ressources.
C) Pour permettre un retry ciblé, le parallélisme et un meilleur debug.
D) Pour supprimer le besoin d'utiliser XCom.
*Réponse : C*

**3. Quel "Design Pattern" Airflow est le plus approprié pour traiter N items indépendants en parallèle puis agréger leurs résultats ?**
A) ETL linéaire.
B) Branching.
C) TaskGroup.
D) Fan-out / Fan-in.
*Réponse : D*

**4. Où devez-vous placer vos imports de bibliothèques lourdes (ex: `pandas`) pour respecter les bonnes pratiques ?**
A) Au niveau du module (haut du fichier DAG).
B) Uniquement dans les fonctions `@task`.
C) Dans le fichier `docker-compose.yaml`.
D) Dans la base de données de métadonnées.
*Réponse : B*

---

### **Partie 2 : Monitoring et Debugging**

**5. Que signifie l'état `upstream_failed` d'une tâche dans Airflow ?**
A) La tâche a échoué à cause d'une erreur de syntaxe interne.
B) La tâche est en attente de retry.
C) La tâche a été bloquée car une tâche dont elle dépend a échoué.
D) La base de données est saturée.
*Réponse : C*

**6. Quelle est la méthode recommandée pour déboguer une tâche spécifique en local ?**
A) Cliquer sur "Clear" dans l'UI.
B) Utiliser la commande `airflow tasks test <dag_id> <task_id> <logical_date>`.
C) Consulter uniquement le calendrier.
D) Redémarrer tout le conteneur Docker.
*Réponse : B*

**7. Pourquoi est-il déconseillé d'appeler `Variable.get()` au niveau du module (top-level) ?**
A) Parce que cela consomme trop de mémoire.
B) Parce que c'est exécuté à chaque parsing du DAG (~30 sec), ralentissant le système.
C) Parce que les variables ne sont accessibles que dans les fonctions `@task`.
D) Parce que c'est une pratique non sécurisée.
*Réponse : B*

---

### **Partie 3 : Intégrations (Hooks & XComs)**

**8. Quel est le rôle principal d'un "Hook" dans Airflow ?**
A) Orchestrer le DAG.
B) Encapsuler la connexion technique à un système externe (ex: Postgres, S3).
C) Visualiser le lignage des données.
D) Remplacer l'opérateur.
*Réponse : B*

**9. Quelle est la limite principale de l'utilisation des XComs ?**
A) Ils ne fonctionnent qu'avec PythonOperator.
B) Ils sont destinés aux petits volumes de données ; pour les gros objets, il faut utiliser un stockage externe.
C) Ils ne sont pas sérialisés en JSON.
D) Ils sont trop rapides et peuvent surcharger le scheduler.
*Réponse : B*

**10. Quelle est la meilleure pratique pour stocker des secrets en production ?**
A) Les stocker en clair dans une Variable.
B) Les écrire en dur dans le code Python.
C) Utiliser un Secret Backend (Vault, AWS Secrets Manager, etc.).
D) Les stocker dans le fichier `airflow.cfg`.
*Réponse : C*

---

### **Partie 4 : n8n et IA Agentique**

**11. Qu'est-ce que le "Vibe Coding" selon le cours ?**
A) Une méthode pour créer des interfaces graphiques complexes avec du code C++.
B) L'utilisation de serveurs dédiés pour héberger des modèles LLM.
C) Une approche où l'utilisateur décrit ses besoins en langage naturel à l'IA qui génère le code.
D) Le fait de programmer uniquement avec de la musique.
*Réponse : C*

**12. Quel est le but premier de n8n ?**
A) Remplacer complètement Apache Airflow pour le Big Data.
B) Connecter différents systèmes pour automatiser des flux de travail (No-Code/Low-Code).
C) Compiler des fichiers binaires pour le déploiement sur serveur.
D) Gérer uniquement les bases de données SQL.
*Réponse : B*

**13. À quoi sert le MCP (Model Context Protocol) ?**
A) À créer des sites web statiques.
B) À standardiser la communication entre les modèles d'IA et les outils/données externes.
C) À chiffrer les communications entre conteneurs Docker.
D) À automatiser les tests unitaires.
*Réponse : B*

**14. Que se passe-t-il si vous avez un pipeline qui tourne depuis 2 heures sans aucune sortie dans les logs ?**
A) C'est normal pour un traitement ETL lourd.
B) Vous devriez utiliser des "heartbeats" (logging.info réguliers) pour suivre la progression.
C) Le scheduler est en train de mettre à jour la base de données.
D) Le pipeline est en mode "idempotent".
*Réponse : B*