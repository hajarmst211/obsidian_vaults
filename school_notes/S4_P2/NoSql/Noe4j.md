### 1. Introduction et Caractéristiques Principales de Neo4j
Neo4j est l'une des bases de données <mark style="background: #BBFABBA6;">orientées graphe </mark>natives les plus évoluées et robustes.
*   **Historique et Licence :** Projet Java initié en 2000 (version 1.0 sortie en 2010), distribué en tant que logiciel libre sous licence GLPv3.
*   **Transactionnel :** C'est une base de données qui respecte strictement les principes <mark style="background: #FFB86CA6;">ACID</mark>
*   **Schemaless :** Elle ne possède pas de schéma pré-établi (flexibilité totale du modèle).
*   **Haute disponibilité et Volumétrie :** Supporte la mise en place de clusters et est capable de stocker et requêter des milliards de nœuds et de relations.
*   **Langage d'interrogation :** Utilise <mark style="background: #FFB86CA6;">Cypher</mark>, un langage de requête graphe déclaratif, simple et efficace.

### 2. Concepts Fondamentaux des Bases de Données Graphe
Le modèle de données s'articule autour de quatre concepts clés :
*   **Nœud (Node) :** Représente une entité (ex: un client, un produit).
*   **Label :** Étiquette permettant de regrouper et de catégoriser des nœuds entre eux (ex: Client, Commande).
*   **Relation :** Matérialise un lien obligatoirement **dirigé** entre deux nœuds. Chaque relation possède un **type** spécifique (ex: "A COMMANDÉ").
*   **Propriétés :** Les nœuds comme les relations peuvent stocker des données sous forme de <mark style="background: #FFB86CA6;">propriétés stockes sous forme de key:value</mark>. Celles-ci peuvent être <mark style="background: #BBFABBA6;">des valeurs numériques, des chaînes de caractères, des booléens ou des listes contenant ces types.</mark>

### 3. Gestion des Transactions et Cohérence
Pour garantir la fiabilité des données, Neo4j assure :
*   **L'isolation :** Les opérations concurrentes sont isolées jusqu'à leur complétion.
*   **L'ordonnancement :** Les opérations d'<mark style="background: #FFF3A3A6;">écriture</mark> sont <mark style="background: #FFF3A3A6;">triées</mark> pour garantir un <mark style="background: #FFB86CA6;">ordre de mise à jour</mark> prévisible. Elles sont stockées de manière ordonnée dans un <mark style="background: #FFB86CA6;">log de transaction</mark>, puis appliquées aux fichiers de données.
*   **La validation :** Lors d'une transaction, Neo4j ne modifie pas immédiatement la base de données principale et globale. il « stocke » ces changements dans une <mark style="background: #FFB86CA6;">memoire temporaire</mark> (en mémoire) qui vous est propre(les autres utilisateurs ne le voie pas). Si la transaction est validee, les changements temporaires <mark style="background: #FFB86CA6;">s'appliquent a la base principale</mark>.

### 4. Disponibilité, Réplication et Performances
#### Definitions:
**Replication**: copie des donnees

**Cluster**: un groupe de plusieurs ordinateurs (serveurs) distincts qui sont connectés entre eux et qui travaillent ensemble pour héberger la même base de données

**Redondance:** C'est le fait de prendre votre base de données parfaitement bien conçue (sans mauvaise duplication interne), et d'en faire des **copies conformes et complètes sur des ordinateurs (serveurs) physiquement différents**.


Neo4j utilise un <mark style="background: #BBFABBA6;">cluster</mark> pour assurer <mark style="background: #BBFABBA6;">la disponibilité et les performances</mark>.
*   **Architecture Maîtres/Esclaves :** Assure la redondance des données et la tolérance aux pannes via la réplication (le graphe entier est copié sur chaque serveur, sans limite de taille). Un protocole se charge d'élire le maître.
*   **Processus d'écriture :** Une majorité de serveurs doit être opérationnelle. <mark style="background: #BBFABBA6;">Les transactions s'exécutent d'abord sur le maître </mark>(génération d'un identifiant), puis sont appliquées aux esclaves (ce délai implique une **cohérence éventuelle**). Sur les esclaves, les verrous sont coordonnés par le maître et partagent le même identifiant.
*   **Performances en lecture :** Les lectures sont possibles sur tous les nœuds du cluster. La capacité de lecture augmente linéairement avec l'ajout de serveurs. Contrairement aux bases relationnelles (BDR), <mark style="background: #BBFABBA6;">la taille globale du graphe n'impacte pas les performances</mark>.
*   **Cache-based sharing :** Les requêtes sont routées de manière consistante pour optimiser l'utilisation de la RAM (ex: toutes les requêtes concernant un "Nœud A" sont systématiquement envoyées au "Serveur 1").

##### Pourquoi la lecture est performante n'importe la taille du graph?

1.  **Indexation des relations (Index-free adjacency) :** Les pointeurs physiques. Chaque nœud contient directement l'adresse mémoire des nœuds auxquels il est lié. <mark style="background: #FFF3A3A6;">Le temps nécessaire pour traverser une relation est donc constant, quelle que soit la taille totale de la base de données.</mark>
2.  **Routage consistant et Cache-based sharing :** <mark style="background: #FFF3A3A6;">Cache based sharing permet d'optimiser l'utilisation de la RAM</mark>. Comme le système peut diriger des requêtes spécifiques vers les serveurs possédant les données pertinentes dans leur cache, la performance de lecture reste stable et efficace.
3.  **Montée en charge linéaire :** La capacité de lecture augmente linéairement avec la taille du cluster. Puisque le graphe est répliqué entièrement sur chaque serveur du cluster, l'ajout de serveurs supplémentaires augmente proportionnellement la capacité de traitement des requêtes sans que la structure globale du graphe ne ralentisse le processus de recherche locale.


### 5. Le Langage Cypher : Principes Généraux
Cypher est un langage déclaratif: une approche de programmation où vous décrivez **ce que vous voulez obtenir**, plutôt que la manière détaillée de le faire.
- Une requette doit etre <mark style="background: #FFB86CA6;">soit ecriture soit lecture</mark>.
*   Une <mark style="background: #FFB86CA6;">requête: plusieurs clauses</mark>.
*   Il gère les transactions (<mark style="background: #FFB86CA6;">plusieurs requêtes possibles par transaction</mark>).
*   Il supporte les variables, les expressions, les opérateurs, les commentaires, et les collections (listes, dictionnaires).
*   <mark style="background: #FFB86CA6;">Il offre un ensemble de fonctions natives </mark>(mathématiques, chaînes de caractères, collections, agrégation).

### 6. Syntaxe Cypher : Recherche et Lecture
Les requêtes se construisent en chaînant des clauses :
*   **Syntaxe de base (Motifs) :**
    *   *Nœud :* Anonyme `()`, nommé `(x)`, avec un label `(:label)`. On peut spécifier des propriétés via des accolades : `(x {city: "Chicago"})`.
    *   *Relation :* Anonyme `-[]->`, nommée `-[r]->`, avec un type/label `-[r:nom_relation]->`.
    *   *Liaison :* `(a)-[r]->(b)`. Les motifs peuvent s'enchaîner pour parcourir plusieurs nœuds et relations.
*   **Clauses de lecture :**
    *   **MATCH :** <mark style="background: #FFB86CA6;">Recherche un motif et retourne une table ou un sous-graphe</mark>. L'option **OPTIONAL MATCH** agit comme une jointure externe en SQL (relation optionnelle). L'option **DISTINCT** élimine les doublons.
    *   **WHERE :** <mark style="background: #FFB86CA6;">Applique des filtres et conditions</mark> (supporte notamment les expressions régulières, ex: `=~ ".*in$"`).
    *   **RETURN :** <mark style="background: #FFB86CA6;">Retourne le résultat final</mark>. Peut être combiné avec **ORDER BY** pour le tri.
*   **Fonctions d'agrégation :** <mark style="background: #FFB86CA6;">Le regroupement se fait automatiquement</mark> sur toutes les colonnes non concernées par l'agrégation.
    *   `SUM()`, `AVG()`, `COUNT(*)` ou `COUNT(DISTINCT X)`.
    *   `COLLECT(X)` : Renvoie une liste contenant les valeurs retournées par une expression.
*   **Fonctions générales :**
    *   `id()` : Retourne l'identifiant interne du nœud.
    *   `labels()` : Renvoie une liste (chaînes de caractères) de toutes les étiquettes d'un nœud.
    *   `type()` : Renvoie le type (label) d'une relation.
    *   Exploration de chemins : `length(path)`, `relationships(path)`, `nodes(path)`, et `reverse(path)` pour inverser l'ordre des éléments.
    *   `range(l,u)` : Génère une liste de nombres entre *l* et *u*.

### 7. Syntaxe Cypher : Écriture, Modification et Suppression
Cypher dispose de clauses dédiées à la manipulation de données :
*   **CREATE :** <mark style="background: #FFB86CA6;">Crée explicitement des nœuds, des propriétés ou des relations</mark>.
*   **MERGE :** Équivalent à un "MATCH-ou-CREATE". <mark style="background: #FFB86CA6;">Crée le nœud ou la relation uniquement s'il n'existe pas déjà</mark>.
*   **SET :** Modifie ou ajoute des données (propriétés) ou des labels à un élément existant.
*   **REMOVE :** <mark style="background: #FFB86CA6;">Supprime des labels ou des propriétés spécifiques</mark>.
*   **DELETE :** <mark style="background: #FFB86CA6;">Supprime des relations</mark>. Pour supprimer un nœud ayant des relations, il faut utiliser **DETACH DELETE** (<mark style="background: #FFB86CA6;">qui supprime le nœud ET toutes ses relations entrantes/sortantes</mark>). Cela permet aussi de vider entièrement la base (`MATCH (n) DETACH DELETE n`).
*   **Importation en masse :** Possibilité de charger des données depuis un fichier <mark style="background: #FFB86CA6;">CSV volumineux à l'aide de la commande</mark> `LOAD CSV WITH HEADERS FROM 'url' AS line` couplée à `USING PERIODIC COMMIT` pour optimiser l'insertion (ex: convertir une année via `toInt()`).

### 8. Critères de Choix d'une Base de Données
*   **Les familles de bases de données disponibles :**
    *   *Relationnelles :* PostgreSQL, MySQL, Microsoft SQL Server.
    *   *Clé-valeur :* Redis, Riak, Oracle BerkeleyDB.
    *   *Documentaires :* MongoDB, CouchDB, CouchBase.
    *   *Familles de colonnes :* Cassandra, HBase.
    *   *Graphes :* Neo4j, Titan.
*   **Les facteurs déterminants :** Le choix (et la conception du modèle) est d'abord guidé par <mark style="background: #FFB86CA6;">les requêtes à effectuer</mark>. D'autres facteurs essentiels doivent être pris en compte :
    *   Le volume de lectures et d'écritures.
    *   La tolérance de l'application vis-à-vis des données incohérentes dans les réplicas.
    *   La nature des relations entre les entités et leur impact direct sur les requêtes.
    *   Les exigences en matière de disponibilité et de reprise après sinistre.
    *   Le besoin de flexibilité du modèle de données (schéma strict vs schemaless).
    *   Les contraintes et exigences de latence.