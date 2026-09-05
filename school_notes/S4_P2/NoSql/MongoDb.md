### 1. Introduction et Caractéristiques Principales de MongoDB
MongoDB est un système de gestion de base de données écrit en **C++**, distribué sous licence **AGPL** (Licence publique générale Affero). 
*   **Orienté documents :** Il utilise un schéma flexible et est conçu pour être distribuable.
*   **Requêtes :** Il interprète les<mark style="background: #ABF7F7A6;"> requêtes JavaScript côté serveur</mark>.
*   **Gestion des fichiers :** La taille maximale d'un document est limitée à <mark style="background: #ABF7F7A6;">16 Mo</mark>. Pour stocker des documents plus volumineux, MongoDB utilise son propre système de découpage appelé <mark style="background: #ABF7F7A6;">GridFS</mark>. 
    * Au lieu de stocker un fichier dans un seul document, GridFS le divise en parties (chunks) stockées dans des documents distincts.

    * La taille de bloc par défaut de GridFS est de <mark style="background: #ABF7F7A6;">255 Ko</mark> (le dernier bloc est limité à la taille strictement nécessaire).
    * Les métadonnées de ces fichiers sont conservées dans une collection séparée nommée `files`.

### 2. Structure des Données
Les données sont structurées de manière <mark style="background: #BBFABBA6;">hiérarchique</mark> et flexible :
*   **Format JSON et BSON :** Pour l'utilisateur, la structure visible est le **JSON** (paires clé/valeur, dérivé de JavaScript). Cependant, MongoDB stocke les données sur le disque au format **BSON** (Binary JSON), plus pratique pour le stockage/traitement, et qui supporte plus de types de données que le JSON (incluant des documents imbriqués, des tableaux, et des tableaux de documents).
*   **Hiérarchie de stockage :** Les <mark style="background: #BBFABBA6;">documents</mark> sont contenus dans des <mark style="background: #BBFABBA6;">collections</mark> (équivalentes aux tables dans les SGBDR).<mark style="background: #BBFABBA6;"> Les documents d'une même collection partagent une structure similaire </mark>(homogénéité).
*   **Espaces de noms et Bases de données :** Les collections peuvent être regroupées dans des <mark style="background: #FFB86CA6;">espaces de noms</mark> (un <mark style="background: #BBFABBA6;">préfixe</mark> ajouté au nom de la collection, séparé par un point, similaire au concept de schémas SQL). Ces collections sont stockées dans des **bases de données (BD)**. Une <mark style="background: #BBFABBA6;">BD</mark> est considérée comme une <mark style="background: #BBFABBA6;">collection de collections.</mark>

### 3. Définition du Schéma de Données (Les Documents)
Un document équivaut à un enregistrement (tuple) en relationnel. 
*   **Le champ `_id` :** C'est le seul champ <mark style="background: #BBFABBA6;">obligatoire</mark>. Il sert de <mark style="background: #FFB86CA6;">clé primaire</mark> et est indexé. Il doit toujours figurer en <mark style="background: #BBFABBA6;">premier</mark> dans le document. Il est de type <mark style="background: #BBFABBA6;">ObjectId</mark> (taille de 12 octets). Sa valeur peut être fournie manuellement ou générée automatiquement par un algorithme garantissant une très faible probabilité de collision.
*   **Règles de nommage :** <mark style="background: #FFB86CA6;">Les noms des champs </mark>ne peuvent pas commencer par le caractère `$`, ni contenir le caractère `.` (point), ni le terme `null`.
*   **Ordre des champs :** MongoDB préserve l'ordre des champs tel que défini à la création, à l'exception du champ `_id` (toujours premier) et lors d'opérations de mise à jour (`update`) incluant un renommage qui peut modifier l'ordre.

### 4. Modélisation des Relations : Références vs Données Imbriquées
Il existe deux manières de lier les données :
##### Les Références (Modèles normalisés) :
Inclusion de <mark style="background: #ABF7F7A6;">liens d'un document à un autre</mark>. C'est à l'application de résoudre ces références.
*   *Quand l'utiliser ?* 
	* Pour éviter les données <mark style="background: #ABF7F7A6;">dupliquées</mark> sans grand avantage en lecture
	* pour représenter des relations <mark style="background: #ABF7F7A6;">complexes</mark> de type <mark style="background: #FFF3A3A6;">many-to-many</mark>
	* pour modéliser de très <mark style="background: #ABF7F7A6;">larges</mark> ensembles de données <mark style="background: #ABF7F7A6;">hiérarchiques</mark>.

##### Les Données Imbriquées (Modèles dénormalisés) :
Sauvegarde des <mark style="background: #ABF7F7A6;">données associées  directement dans la même structure</mark>. <mark style="background: #BBFABBA6;">Permet d'extraire et de manipuler plusieurs niveaux de hiérarchie en une seule instruction.</mark>
*   *Quand l'utiliser ?* 
	* Pour des relations d'appartenance de type <mark style="background: #FFF3A3A6;">one-to-one</mark> (ex: Personne et son adresse) ou <mark style="background: #FFF3A3A6;">one-to-many</mark> où les documents fils (many) apparaissent toujours dans le contexte du parent (one) (ex: Personne et ses emails).

### 5. L'instance MongoDB et ses Outils
Une instance MongoDB se caractérise par : un <mark style="background: #FFF3A3A6;">port d'écoute</mark> (par défaut **27017**), un <mark style="background: #FFF3A3A6;">processus serveur</mark>, un <mark style="background: #FFF3A3A6;">répertoire racine de stockage</mark>, un <mark style="background: #FFF3A3A6;">fichier de log</mark>, et un <mark style="background: #FFF3A3A6;">fichier de configuration (mongod.conf).</mark>
Les outils fournis sont :
*   `mongod` : Le processus daemon de base gérant les requêtes et l'accès aux données.
*   `mongosh` : Le shell interactif en ligne de commande JavaScript.
*   `mongos` : Le contrôleur de sharding (répartition).
*   `mongoimport` / `mongoexport` : Les outils d'importation et d'exportation.
*   `mongostat` : Outil de visualisation des statistiques de l'instance.

### 6. Administration et Manipulation via le Shell (`mongosh`)
Le shell permet de consulter/tester des requêtes, créer des index, et administrer la base. Les <mark style="background: #BBFABBA6;">commandes JS</mark> peuvent être exécutées depuis un fichier via la fonction `load('test.js')`.
*   **Gestion des bases de données :**
    *   Afficher la base en cours : `> db`
    *   Lister les bases : `> show dbs`
    *   Créer/Utiliser une base : `> use <db_name>`
    *   Supprimer une base : `> db.dropDatabase()`
    *   *Bases par défaut :* `local` (documents locaux jamais dupliqués), `test` (base vide), `admin` (commandes d'administration comme l'arrêt du serveur), `config` (utilisée en sharding pour les informations des nœuds).
    *   *Fichier namespace :* Pour chaque base,<mark style="background: #FFF3A3A6;"> le fichier avec ".ns" stocke les namespaces</mark>: L'emplacement physique des collections et des index associés dans la base.
*   **Gestion des collections :**
    *   Création automatique (lors de la première insertion) ou manuelle : `> db.createCollection("<nom_collection>")`
    *   Lister : `> show collections` ou `> db.getCollection()`
    *   Supprimer : `> db.<collection_name>.drop()`

### 7. Opérations CRUD (Création, Lecture, Mise à jour, Suppression)
*   **Insertion :** 
    *   Modes d'insertion via le shell (`insert()`, `save()`) ou via des drivers (PHP, Java, Python).
    *   Syntaxe : `> db.collection.insert(document)` (un seul document en format tableau clés/valeurs) ou 
    `> db.collection.insert(documents)` (liste de documents).
*   **Recherche / Consultation (`find` et `findOne`) :**
    *   `findOne()` : Retourne un document unique (le premier trouvé).
    *   `find()` : Retourne une liste de documents sous forme de **curseur** (pointer/objet itérable, ex: `c[0]`: premier document).
    *   Syntaxe : `> db.collection.find(requête, projection)`. Sans paramètres, renvoie tous les documents.
	    *   *Requête :* Tableau spécifiant les conditions. Utilisation d'opérateurs logiques et conditionnels : `$or` (OU logique), `$gt` (>), `$gte` (>=), `$lt` (<), `$lte` (<=), `$ne` (!=), et `$in` (dans une liste).
	    *   *Projection :* Permet de limiter les champs renvoyés (ex: `{prenom:0, nom:0}` pour exclure ces champs).
	    *   *Modificateurs :* `.sort({field: 1|-1})` pour trier (1 = ascendant, -1 = descendant), `.limit(N)` pour limiter le nombre de résultats, et `.skip(M)` pour ignorer les M premiers documents.
*   **Modification :**
    *   Syntaxe : `> db.collection.update(requête, modifications)`. La modification utilise des opérateurs spécifiques, comme `$set` pour modifier la valeur d'un champ.
*   **Suppression :**
    *   Syntaxe : `> db.collection.remove(requête)`. Attention : sans paramètre, tous les documents de la collection sont supprimés.
*   **Requêtes Agrégées :**
    *   MongoDB offre des fonctions d'agrégation similaires aux SGBDR.
    *   `countDocuments()` : Calcule le nombre de documents correspondant à un critère.
    *   `Distinct()` : Retourne les valeurs distinctes (sans doublon) d'un champ.

### 8. La Réplication (Haute Disponibilité)
La réplication assure la redondance et la haute disponibilité par la <mark style="background: #BBFABBA6;">synchronisation</mark> des données sur de <mark style="background: #BBFABBA6;">multiples serveurs</mark> (si un serveur crash, un autre prend le relais).
*   **Le Replica Set :** C'est un groupe de processus `mongod` conservant le même ensemble de données. Un Replica Set nécessite au minimum 2 nœuds (1 maître et 1 ou plusieurs esclaves) et est composé de différents types de membres :
##### Le membre Primaire (Maître) :
C'est l'<mark style="background: #FFB86CA6;">unique</mark> membre recevant les opérations de <mark style="background: #FFB86CA6;">lecture et d'écriture</mark>. Il enregistre tous les changements dans son journal appelé <mark style="background: #FFF3A3A6;">oplog</mark>. Une application dirige ses requêtes vers lui par défaut.
##### Les membres Secondaires :
ils se basent sur cet <mark style="background: #FFF3A3A6;">oplog</mark> afin de <mark style="background: #FFF3A3A6;">appliquer</mark> les mêmes opérations du maitre sur leur propre ensemble de données de facon **asynchrone**. Ils acceptent <mark style="background: #FFB86CA6;">uniquement</mark> des opérations de <mark style="background: #FFB86CA6;">lecture</mark>. Un secondaire peut être configuré pour : ne jamais être élu primaire (rester un simple backup), bloquer les lectures des applications pour réserver son trafic, ou exécuter des snapshots (historique).
##### L'Arbitre :
Il ne conserve <mark style="background: #ABF7F7A6;">aucune copie des données et ne peut devenir primaire</mark>. Son seul rôle est de voter lors des élections (défini pour les Replica Sets ayant un nombre pair d'esclaves).
##### Les Élections :
*   Elles ont lieu à la création du Replica Set ou lors de la panne du primaire.
*   Pendant le processus (qui prend du temps), le système passe en **readonly**.
*   L'élection est basée sur <mark style="background: #FFB86CA6;">une priorité définie pour chaque membre </mark>(modifiable par position géographique, etc. ; par défaut tous ont la même priorité). <mark style="background: #FFB86CA6;">Le membre avec la plus haute priorité est élu.</mark>
*   Chaque membre ne peut voter que pour **un seul autre membre**. Celui recevant le plus de votes devient le nouveau primaire.

### 9. Le Sharding (Partitionnement et Scalabilité Horizontale)
Le sharding permet de <mark style="background: #BBFABBA6;">distribuer le stockage</mark> des données sur différentes instances MongoDB (en pré-requis, chaque fragment/shard doit être un Replica Set).

##### Un shard 
C'est une instance MongoDB (un serveur ou un groupe de serveurs) qui stocke **une partie** (les chunks) de la quantité totale de vos données.

##### Balancer
Dans MongoDB, le **Balancer** (que l'on pourrait traduire par "l'équilibreur") est un **processus automatique qui tourne en arrière-plan** dans votre cluster.

Son rôle unique et fondamental est de **garantir que vos données sont toujours réparties de manière équitable** entre tous vos serveurs (les shards).

##### Mongos:
C'est le point d'<mark style="background: #ABF7F7A6;">entrée</mark> pour les applications clientes. Il fait l'<mark style="background: #ABF7F7A6;">interface</mark> entre l'application et le cluster de sharding. Il ne stocke pas lui-même les données, il agit comme un **intermédiaire intelligent**.

- **Il interroge les Config Servers :** Il consulte les serveurs de configuration pour savoir où se trouvent les données (le "mapping" entre les données et les différents shards).
    
- **Il redirige la requête :** Il sait exactement sur quel shard (fragment de base de données) se trouve la donnée demandée. Il envoie donc la requête vers le shard approprié.
    
- **Il agrège le résultat :** Si la requête concerne des données réparties sur plusieurs shards, mongos récupère les réponses de chaque shard, les combine et renvoie le résultat final à l'application cliente comme s'il s'agissait d'une seule source de données.
##### Architecture du Sharding :
*   **Shards :** Stockent les données, qui y sont distribuées et répliquées.
*   **Query Routers (instances `mongos`: le controleur du shrading) :** L'interface avec les applications clientes. Ils <mark style="background: #ABF7F7A6;">redirigent les opérations</mark> vers le shard approprié et retournent le résultat. Il y en a plusieurs pour répartir les tâches.
*   **Config Servers :** <mark style="background: #ABF7F7A6;">Stockent les métadonnées du cluster et le mapping données/shards</mark>. En production, <mark style="background: #FFF3A3A6;">il en faut 3</mark>.
##### Partitionnement des données :
*   S'effectue au niveau des collections à l'aide d'une **Shard key** (clé de fragmentation : champ simple ou composé, indexé, présent dans chaque document).
*   Les valeurs de cette clé sont divisées en morceaux appelés **chunks**, distribués équitablement.
*   *Stratégie 1 : Basée sur le rang.* Définit des intervalles sans chevauchement. <mark style="background: #BBFABBA6;">Documents proches = même shard</mark>. Recherche <mark style="background: #FFB86CA6;">facile</mark>, mais risque de <mark style="background: #FFB86CA6;">mauvaise distribution</mark> (ex: requêtes temporelles surchargeant un seul serveur à une heure donnée).
*   *Stratégie 2 : Basée sur le hash.* Utilise le hash d'un champ pour créer des partitions. Deux documents ayant des clés proches ont très peu de chance de se retrouver dans le même shard. Cela assure une distribution <mark style="background: #BBFABBA6;">aléatoire et plus équitable</mark> de la collection. Cependant, c'est <mark style="background: #FFB86CA6;">moins efficace</mark> en termes de performances, car pour une <mark style="background: #FFB86CA6;">recherche</mark> sur un intervalle défini, le système devra parcourir plusieurs shards au lieu d'un seul.

- *Stratégie 3 : Basée sur les tags (étiquettes)*. <mark style="background: #BBFABBA6;">Les administrateurs définissent des tags</mark> qu'ils associent à des intervalles de clés, puis associent ces tags aux différents shards en visant une distribution équitable. Le <mark style="background: #ABF7F7A6;">balancer</mark> migre ensuite les données taggées vers les shards adéquats. C'est le <mark style="background: #BBFABBA6;">meilleur moyen</mark> d'assurer une bonne répartition maîtrisée des données.
	- C'est le **meilleur moyen pour assurer une bonne répartition** car c'est la seule méthode qui offre un **contrôle total, intelligent et sur-mesure** aux administrateurs.
    
### Maintien d’une distribution équitable
L'ajout de nouvelles données ou de serveurs peut rendre la distribution déséquilibrée. MongoDB utilise deux mécanismes pour y remédier :

- **Le Splitting (fragmentation) :** Son but est d'éviter d'avoir des <mark style="background: #BBFABBA6;">chunks trop larges</mark>. Quand la taille d'un chunk dépasse une valeur prédéfinie (chunk size), MongoDB divise cet ensemble de données en deux <mark style="background: #FFF3A3A6;">sur le même shard</mark>. Ce processus est déclenché automatiquement par les insertions et les modifications. Un split modifie uniquement les métadonnées : il <mark style="background: #ABF7F7A6;">ne fait pas migrer les données et n'affecte pas le contenu des shards.</mark>
    
- **Le Balancing (équilibrage) :** C'est un processus en arrière-plan gérant les migrations de chunks (il peut être lancé à partir de n'importe quel query router). Si la distribution est déséquilibrée, <mark style="background: #BBFABBA6;">le balancer fait migrer des chunks du shard en ayant le plus vers celui en ayant le moin</mark>s, jusqu'à ce que la répartition soit équitable.
    
    - Étapes de migration : 1) Le shard de destination reçoit tous les documents du chunk à migrer. 2) Il applique tous les changements faits aux données durant le processus de migration. 3) <mark style="background: #FFF3A3A6;">Finalement, les métadonnées concernant l'emplacement du chunk sont mises à jour sur le config server</mark>.
#### NB:
- <mark style="background: #FFB8EBA6;">Le Splitting modifie uniquement les métadonnées, tandis que le Balancing migre physiquement les données</mark>
##### Ajout d'un shard :
- **Intégration :** L'administrateur connecte le nouveau shard (le nouveau Replica Set) au cluster via le routeur (mongos).
    
- **Le Déséquilibre initial :** Au moment précis où le shard est ajouté, il est **totalement vide**. Un énorme <mark style="background: #ABF7F7A6;">déséquilibre</mark> se crée dans le cluster (par exemple : Shard A a 500 chunks, Shard B a 500 chunks, et<mark style="background: #FFF3A3A6;"> le nouveau Shard C a 0 chunk</mark>).
    
- **Intervention du Balancer :** Le système de MongoDB détecte ce déséquilibre. Le **Balancer** (le processus en arrière-plan chargé de l'équité) se réveille.
    
- **La Migration :** Le Balancer commence immédiatement à copier des chunks depuis les Shards surchargés (A et B) vers le nouveau Shard (C).
    
- **Le Temps d'attente :** Comme le précise votre cours, bien que la migration commence tout de suite, **cela prend du temps**. Déplacer des gigaoctets de données sur le réseau ne se fait pas en une seconde. Le cluster fonctionnera parfaitement pendant ce temps, mais il ne sera considéré comme "équilibré" qu'une fois la migration de tous les chunks terminée.

##### Suppression d'un shard :
- <mark style="background: #FFB86CA6;">Le balancer se charge d'abord de migrer tous les chunks de ce shard vers les autres shards restants</mark>. Ce n'est qu'une fois toutes les données migrées et les méta-données mises à jour que la suppression physique du shard peut avoir lieu.