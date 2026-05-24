# Web content mining:
- Il repose sur la capacité a determiner si deux documents se ressemblent 
- Pour comparer des textes, on transforme les mots en **vecteurs** (des listes de nombres). Une fois sous forme mathématique, on peut utiliser des calculs comme le **cosinus de l'angle** entre deux vecteurs pour mesurer leur proximité sémantique.

# web structure mining
- on s'intéresse ici à **l'organisation et aux liens** entre les pages
### PageRank algorithm
- La valeur (le "score") d'une page dépend de qui pointe vers elle. Si des pages importantes pointent vers une page, cette page devient importante à son tour.
- Le score représente la probabilité qu'un utilisateur qui clique au hasard sur des liens finisse par arriver sur cette page
- Le paramètre **"d"** (damping factor) représente le moment où l'utilisateur arrête de suivre des liens pour taper une nouvelle adresse directement.
### Autorités/Hubs
- **Les Autorités (Authorities) :** Ce sont les pages qui contiennent l'information de référence (ex: une page officielle Wikipédia sur un sujet). Elles ont beaucoup de liens entrants.
- **Les Hubs :** Ce sont les pages qui servent de "répertoires" ou de listes de liens vers les meilleures autorités (ex: une page de ressources ou un annuaire). Elles ont beaucoup de liens sortants.
- **Relation mutuelle :** Un bon hub pointe vers de bonnes autorités, et une bonne autorité est pointée par de bons hubs. C'est un cercle vertueux.

### communautés
- Une communauté est un groupe de pages qui sont beaucoup plus connectées entre elles qu'avec le reste du Web
---
# web usage mining
### 1. Qu'est-ce que le Web Usage Mining ?

C'est le processus consistant à découvrir <mark style="background: #BBFABBA6;">des modèles de comportement</mark> à partir des journaux (logs) des serveurs Web ou des services de suivi (tracking).

- **Pourquoi ?** Pour comprendre comment un site est utilisé, prédire les besoins des utilisateurs, personnaliser le contenu, améliorer la structure du site et augmenter les ventes/fidélité.
    
- **Différence avec l'Analytics :** Alors que l'Analytics se contente souvent de statistiques descriptives , le Web Usage Mining va plus loin en créant des modèles prédictifs.
    

### 2. Les fondements techniques (Le protocole HTTP et les Cookies)

- **Le problème du "Stateless" :** Le protocole HTTP est "sans état". Cela signifie que le serveur Web ne se souvient pas de qui vous êtes d'une requête à l'autre.
    
- **La solution (Cookies) :** Pour résoudre cela, on utilise des **cookies**. Le serveur envoie une petite information au navigateur que celui-ci renvoie à chaque nouvelle requête, ce qui permet au serveur d'identifier qu'il s'agit du même utilisateur (et de suivre sa session).
    

### 3. La source des données : Les fichiers "Server Log"

Les logs sont des fichiers textuels bruts enregistrés par le serveur à chaque clic. Ils contiennent des informations précieuses :

- **Adresse IP** du visiteur.
    
- **Timestamp** (date et heure).
    
- **Requête (GET/POST)** : la page ou le fichier demandé.
    
- **Code de statut** (ex: 200 = succès, 404 = page non trouvée).
    
- **Referrer** : la page d'où vient l'utilisateur.
    
- **User Agent** : quel navigateur et quel système d'exploitation sont utilisés.
    

### 4. Le processus complexe de préparation (Le travail de "nettoyage")

Le "Mining" ne peut pas se faire sur des logs bruts, car ils sont "sales". Il faut donc suivre ces étapes :

1. **Nettoyage des données :** Supprimer les lignes inutiles (ex: robots d'indexation/spiders), supprimer les erreurs techniques, et combler les manques (ex: si le contenu a été chargé depuis le cache local du navigateur et n'a pas laissé de trace sur le serveur).
    
2. **Intégration :** Combiner les logs avec d'autres sources (ventes, profils utilisateurs).
    
3. **Transformation et "Sessionization" :** C'est l'étape cruciale. Il faut regrouper les clics isolés pour reconstituer des **sessions utilisateur** (ex: définir qu'une série de clics sur 10 minutes correspond à une visite unique).
    

### 5. Méthodes d'analyse avancée

Une fois les données préparées, on utilise des algorithmes pour :

- **Prédiction (Markov) :** Prédire quel sera le prochain clic d'un utilisateur.
    
- **Association :** Découvrir quels produits/pages sont souvent consultés ensemble
    
- **Clustering :** Regrouper les utilisateurs ayant des comportements similaires 
    
- **Classification :** Catégoriser le visiteur pour adapter l'affichage en temps réel (personnalisation).
    