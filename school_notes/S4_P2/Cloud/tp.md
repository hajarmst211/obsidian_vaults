
## Module 1 : Architecture Découplée (Amazon SQS & Amazon SNS)

### 1. Couplage Fort (Tight Coupling) vs. Découplage (Decoupling)
*   **Couplage Fort (Phase 1) :** Le serveur web communique directement et de manière synchrone avec le serveur d'application (ex: via une requête HTTP directe sur le port `8009`). Si le serveur d'application subit une panne, l'ensemble du processus échoue immédiatement et l'utilisateur rencontre une erreur.
*   **Découplage (Phase 2) :** Les composants ne communiquent plus directement. Le serveur web dépose l'image dans S3 et met à jour DynamoDB. Un message contenant l'événement est envoyé à une file d'attente (**Amazon SQS**) via un service de notification (**Amazon SNS**). Le serveur d'application traite les messages de manière asynchrone à son propre rythme. Si le serveur d'application est indisponible, les messages s'accumulent en toute sécurité dans SQS jusqu'à sa remise en service.

### 2. Flux de l'Architecture Découplée (Phase 2)
1.  **Upload :** L'utilisateur charge une image sur le serveur web.
2.  **Stockage & Métadonnées :** Le serveur web écrit l'image directement dans le compartiment Amazon S3 (`Phase2bucket`) et crée un enregistrement d'état initial dans Amazon DynamoDB.
3.  **Déclenchement d'Événement :** S3 détecte la création de l'objet et envoie une notification d'événement à un sujet **Amazon SNS** (`uploadnotification`).
4.  **Fan-out (Multi-diffusion) :** Le sujet SNS distribue le message à deux abonnés simultanément :
    *   Une file d'attente **Amazon SQS** (`ImageApp`).
    *   Une adresse email (Notification directe à l'administrateur/utilisateur).
5.  **Consommation (Polling) :** Le serveur d'application interroge la file d'attente SQS (*polling*), récupère le message de notification, télécharge l'image depuis S3, applique le traitement (teinte/redimensionnement), met à jour DynamoDB et replace l'image finale dans S3.

### 3. Éléments Clés de Configuration
*   **Access Policy SNS pour S3 :** Pour permettre à S3 d'écrire dans un topic SNS, la politique d'accès du topic SNS (*Access Policy*) doit autoriser le principal de service `s3.amazonaws.com` avec une condition restreignant la source au bucket S3 spécifique :
    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "S3 SNS topic policy",
          "Effect": "Allow",
          "Principal": { "Service": "s3.amazonaws.com" },
          "Action": "SNS:Publish",
          "Resource": "arn:aws:sns:us-east-1:ACCOUNT_ID:uploadnotification",
          "Condition": {
            "ArnLike": { "aws:SourceArn": "arn:aws:s3:::nom-du-bucket-phase2" }
          }
        }
      ]
    }
    ```
*   **S3 Event Notifications :** Configuré dans l'onglet *Properties* du bucket S3. L'événement déclencheur est `All object create events` (ou `s3:ObjectCreated:*`) et la destination est le topic SNS.

---

## Module 2 : Conteneurisation avec Docker et Amazon ECR

### 1. Concepts Docker Clés
*   **Dockerfile :** Fichier texte contenant les instructions nécessaires pour assembler une image Docker.
*   **Image Docker :** Modèle en lecture seule utilisé pour créer des conteneurs.
*   **Conteneur Docker :** Instance active et isolée d'une image.

### 2. Analyse d'un Dockerfile (Node.js)
```dockerfile
FROM node:11-alpine          # Image de base légère sous Alpine Linux avec Node.js
RUN mkdir -p /usr/src/app    # Crée le répertoire de travail dans le conteneur
WORKDIR /usr/src/app         # Définit le répertoire de travail par défaut
COPY . .                     # Copie le code source local dans le conteneur
RUN npm install              # Installe les dépendances définies dans package.json
EXPOSE 3000                  # Indique que le conteneur écoute sur le port 3000
CMD ["npm", "run", "start"]  # Commande exécutée au démarrage du conteneur
```

### 3. Commandes Docker Essentielles
*   **Build de l'image :** 
    ```bash
    docker build --tag node_app .
    ```
*   **Lancement du conteneur en arrière-plan (detached mode) avec variables d'environnement et redirection de port :**
    ```bash
    docker run -d --name node_app_1 -p 3000:3000 -e APP_DB_HOST="172.17.0.3" node_app
    ```
    *   `-d` : Mode détaché (s'exécute en arrière-plan).
    *   `-p 3000:3000` : Redirige le port `3000` de l'hôte vers le port `3000` du conteneur.
    *   `-e APP_DB_HOST="..."` : Injecte une variable d'environnement pour écraser la configuration par défaut de la base de données.
*   **Inspection et administration :**
    *   `docker ps` ou `docker container ls` : Liste les conteneurs actifs.
    *   `docker exec -ti <container-id> sh` : Ouvre un terminal interactif dans le conteneur en cours d'exécution.
    *   `docker inspect <container-id>` : Récupère les métadonnées détaillées (comme l'adresse IP interne du conteneur dans le sous-réseau Docker par défaut `bridge`).
    *   `docker stop <name> && docker rm <name>` : Arrête et supprime un conteneur.

### 4. Publication sur Amazon Elastic Container Registry (ECR)
1.  **Authentification du client Docker auprès d'ECR :**
    ```bash
    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
    ```
2.  **Création du dépôt sur ECR :**
    ```bash
    aws ecr create-repository --repository-name node-app
    ```
3.  **Étiquetage (Tag) de l'image locale :** Relie l'image locale au dépôt distant ECR.
    ```bash
    docker tag node_app:latest <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/node-app:latest
    ```
4.  **Push de l'image vers ECR :**
    ```bash
    docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/node-app:latest
    ```

---

## Module 3 : Hébergement Web Statique et Protection des Données (Amazon S3)

### 1. Hébergement de site web statique
*   Un bucket S3 peut héberger un site statique (HTML, CSS, JS, images).
*   **Prérequis :**
    1.  Activer l'option **Static website hosting** dans les propriétés du bucket et définir le document d'index (ex : `index.html`).
    2.  Désactiver l'option **Block all public access** (Bloquer tout l'accès public) au niveau du bucket.
    3.  Activer les listes de contrôle d'accès (**ACLs**) si nécessaire pour la gestion individuelle des objets.
    4.  Appliquer une **Bucket Policy** publique pour permettre l'accès en lecture aux utilisateurs anonymes :
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicRead",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::votre-nom-de-bucket/*"
            }
        ]
    }
    ```

### 2. Protection des données : Versioning
*   **Object Versioning :** Permet de conserver plusieurs versions d'un même objet dans un bucket.
*   Protège contre les écrasements accidentels et les suppressions involontaires.
*   Si vous supprimez un objet sans spécifier d'ID de version, S3 ajoute un **Delete Marker** (marqueur de suppression). L'objet semble supprimé, mais les anciennes versions restent accessibles et restaurables. Pour supprimer définitivement un objet, vous devez supprimer explicitement l'ID de la version ou le marqueur de suppression.

### 3. Optimisation des coûts : Cycle de vie (Lifecycle Policies)
Permet de définir des règles automatiques pour gérer le stockage des objets au fil du temps :
*   **Transition Rules (Règles de transition) :** Déplacent les anciennes versions des objets vers des classes de stockage moins coûteuses (ex : `S3 Standard-IA` après 30 jours, puis vers `S3 Glacier` si nécessaire).
*   **Expiration Rules (Règles d'expiration) :** Suppriment définitivement les anciennes versions des objets après une période définie (ex : 365 jours).

### 4. Plan de reprise d'activité (DR) : Réplication inter-régions (CRR)
*   **Cross-Region Replication (CRR) :** Réplication automatique et asynchrone des objets d'un bucket S3 source vers un bucket de destination situé dans une autre région AWS.
*   **Prérequis :**
    1.  Le versioning doit être activé sur le bucket source **ET** sur le bucket de destination.
    2.  Un rôle IAM (**CafeRole**) doit être associé à la règle de réplication pour donner à S3 l'autorisation de lire dans le bucket source et d'écrire dans le bucket de destination (`s3:GetObject`, `s3:ReplicateObject`, `s3:ReplicateDelete`, etc.).
*   *Note sur la suppression :* Par défaut, la suppression d'une version spécifique ou d'un objet dans le bucket source ne supprime pas automatiquement les versions répliquées historiques dans le bucket de destination afin d'éviter la propagation d'accidents de suppression.

---

## Module 4 : Stockage Partagé avec Amazon Elastic File System (EFS)

### 1. Concepts et Architecture
*   **Amazon EFS :** Fournit un stockage de fichiers NFSv4 partagé, élastique et scalable, utilisable par plusieurs instances EC2 simultanément à travers différentes zones de disponibilité (AZ).
*   **Mount Targets (Cibles de montage) :** Pour monter un système de fichiers EFS, vous devez créer une cible de montage dans chaque sous-réseau (AZ) de votre VPC. Chaque cible de montage reçoit une adresse IP du sous-réseau associé.
*   **Sécurité réseau (Security Groups) :**
    *   **EFS Mount Target Security Group :** Doit autoriser le trafic entrant sur le port **TCP 2049** (protocole NFS) en provenance des instances EC2 clientes.
    *   **Configuration recommandée :** Définir la source de la règle de sécurité de la cible de montage comme étant l'ID du groupe de sécurité attaché aux instances EC2 clientes (ex : `sg-037279...`).

### 2. Procédure de Montage sur EC2 (Linux)
1.  Connexion à l'instance EC2.
2.  Installation des utilitaires de support EFS :
    ```bash
    sudo yum install -y amazon-efs-utils
    ```
3.  Création du point de montage local :
    ```bash
    mkdir efs
    ```
4.  Montage du système de fichiers via le client NFSv4.1 :
    ```bash
    sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport <DNS_DU_SYSTEME_DE_FICHIERS>:/ efs
    ```
5.  Vérification de l'espace disque disponible :
    ```bash
    df -hT
    ```
    *(Le type doit être `nfs4` et la taille logique affichée s'élève à plusieurs exaoctets `8.0E` en raison de la nature élastique d'EFS).*

### 3. Performance et Surveillance (CloudWatch)
*   **Outil de Benchmarking (Flexible IO / `fio`) :** Permet d'analyser les performances d'écriture en simulant des charges de travail de fichiers de grande taille.
*   **Métriques CloudWatch Essentielles :**
    *   `PermittedThroughput` : Le débit maximal autorisé pour le système de fichiers, qui évolue proportionnellement à la quantité de données stockées (mode de débit par défaut *Bursting*).
    *   `DataWriteIOBytes` / `DataReadIOBytes` : Volume de données lues ou écrites. En configurant la statistique sur `Sum` et la période sur `1 Minute`, on peut calculer le débit effectif en divisant la somme par 60 secondes.

---

## Vocabulaire et Concepts Clés pour l'Examen

| Terme Anglais                | Équivalent / Explication en Français | Rôle Principal                                                                                    |
| :--------------------------- | :----------------------------------- | :------------------------------------------------------------------------------------------------ |
| **Decoupling**               | Découplage                           | Séparer les composants d'une application pour améliorer la tolérance aux pannes.                  |
| **Polling**                  | Interrogation active / Sondage       | Action pour un consommateur (App Server) de demander régulièrement des messages à SQS.            |
| **Fan-out**                  | Multi-diffusion                      | Scénario où un message envoyé à un topic SNS est poussé vers plusieurs destinations (SQS, Email). |
| **Port Forwarding**          | Redirection de port                  | Mapper un port de la machine hôte vers un port spécifique du conteneur (ex: `-p 3000:3000`).      |
| **Lifecycle Policy**         | Politique de cycle de vie            | Automatiser le transfert ou la suppression des objets S3 pour réduire les coûts.                  |
| **Cross-Region Replication** | Réplication inter-régions            | Copier automatiquement les données S3 dans une autre région pour la reprise d'activité (DR).      |
| **Mount Target**             | Cible de montage                     | Point d'accès réseau (adresse IP) créé dans une AZ pour connecter EC2 à EFS.                      |
| **NFS (Port 2049)**          | Network File System                  | Protocole réseau utilisé par Amazon EFS pour le partage de fichiers.                              |
