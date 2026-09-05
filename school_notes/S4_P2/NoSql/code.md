Voici un récapitulatif structuré de l'ensemble des commandes, requêtes et exemples de code présents dans les différents cours (MongoDB et Neo4j / Cypher), avec des explications sur leur fonctionnement.

---

# PARTIE 1 : MONGODB (Shell & requêtes)

Les commandes suivantes s'exécutent généralement dans l'invite interactive de MongoDB (`mongosh`).

### 1. Commandes d'administration de base
*   **Afficher la base de données actuellement utilisée :**
    ```javascript
    db
    ```
*   **Afficher la liste des bases de données disponibles :**
    ```javascript
    show dbs
    ```
*   **Se positionner sur une base de données (ou la créer si elle n'existe pas) :**
    ```javascript
    use <db_name>
    ```
*   **Supprimer la base de données courante :**
    ```javascript
    db.dropDatabase()
    ```
*   **Charger et exécuter un fichier de script JavaScript externe :**
    ```javascript
    load('test.js')
    ```

### 2. Gestion des collections
*   **Créer explicitement une collection :**
    ```javascript
    db.createCollection("<nom_collection>")
    ```
*   **Lister les collections de la base courante :**
    ```javascript
    show collections
    ```
*   **Supprimer une collection :**
    ```javascript
    db.<nom_collection>.drop()
    ```

### 3. Insertion de documents
*   **Insertion simple (un seul document) :**
    ```javascript
    db.etudiants.insert({'prenom': 'Ahmed', 'nom': 'Hamidi'})
    ```
    *(Note : Si vous insérez sans spécifier de champ `_id`, MongoDB génère automatiquement un `ObjectId` unique).*

*   **Insertion avec un seul champ :**
    ```javascript
    db.etudiants.insert({'prenom': 'Jalal'})
    ```

*   **Insertion multiple (tableau de documents) :**
    ```javascript
    db.etudiants.insert([
      {'prenom': 'Hamza', 'nom': 'Alami'}, 
      {'prenom': 'Abdelkader', 'nom': 'Mahdaoui'}
    ])
    ```

### 4. Recherche et Consultation (`find` et `findOne`)
*   **Récupérer tous les documents d'une collection :**
    ```javascript
    db.etudiants.find()
    ```
*   **Recherche avec filtre (égalité) :**
    ```javascript
    db.etudiants.find({'prenom': 'Ahmed'})
    ```
*   **Manipulation du curseur de résultat en JavaScript :**
    ```javascript
    var c = db.etudiants.find({'prenom': 'Ahmed'})
    c[0]       // Accède au premier document du curseur
    c[0].nom   // Accède à la propriété "nom" du premier document (affiche "Fathi" par exemple)
    ```
*   **Recherche d'un seul document (renvoie directement le document plutôt qu'un curseur) :**
    ```javascript
    var ahmed = db.etudiants.findOne({'_id': ObjectId('532d40c72d510b4635b8cfc9')})
    ahmed.nom  // Accède directement à la propriété "nom"
    ```

### 5. Opérateurs de comparaison et logiques dans `find`
*   **Opérateur logique OU (`$or`) :**
    ```javascript
    db.collection.find({$or : [{field1: value1}, {field2: value2}]})
    ```
*   **Opérateurs de comparaison :**
    *   `$gt` (strictement supérieur)
    *   `$gte` (supérieur ou égal)
    *   `$lt` (strictement inférieur)
    *   `$lte` (inférieur ou égal)
    *   `$ne` (différent de)
*   **Opérateur d'appartenance (`$in`) :**
    ```javascript
    { field: { $in: [value1, value2, value3] } }
    ```

### 6. Projection, Tri, Limite et Saut
*   **Projection (sélectionner ou exclure des champs) :**
    *Pour sélectionner tous les documents en excluant les champs `prenom` et `nom` (le `_id` reste visible par défaut) :*
    ```javascript
    db.etudiants.find({}, {prenom: 0, nom: 0})
    ```
*   **Limiter le nombre de résultats (`limit`) :**
    ```javascript
    db.collection.find(...).limit(N)
    ```
*   **Sauter les premiers résultats (`skip`) pour la pagination :**
    ```javascript
    db.collection.find(...).limit(N).skip(M)
    ```
*   **Trier les résultats (`sort`) :**
    *`1` pour un tri ascendant, `-1` pour un tri descendant.*
    ```javascript
    db.collection.find(...).sort({field1: 1, field2: -1})
    ```

### 7. Mise à jour et Suppression
*   **Mise à jour d'un document (`update`) :**
    *Utilisation de l'opérateur `$set` pour modifier uniquement le champ spécifié sans écraser le reste du document.*
    ```javascript
    db.users.update({'prenom': 'Ahmed'}, {'$set': {'nom': 'Alami'}})
    ```
*   **Suppression de documents (`remove`) :**
    *   *Supprimer tous les documents d'une collection (à manipuler avec précaution) :*
        ```javascript
        db.collection.remove()
        ```
    *   *Supprimer les documents répondant à un critère spécifique :*
        ```javascript
        db.etudiants.remove({'_id': ObjectId("532d40c72d510b4635b8cfc9")})
        ```

---

# PARTIE 2 : NEO4J & CYPHER (Graphes)

Le langage **Cypher** est utilisé pour interroger et manipuler les bases de données orientées graphe Neo4j.

### 1. Syntaxe de base des motifs (Patterns)
*   `()` : Un nœud anonyme.
*   `(x)` : Un nœud nommé `x` (variable réutilisable dans la requête).
*   `(:label)` : Un nœud anonyme ayant l'étiquette (label) `label`.
*   `(x {city: 'Chicago'})` : Un nœud nommé `x` possédant la propriété `city` égale à "Chicago".
*   `-[]->` : Une relation anonyme dirigée.
*   `-[r]->` : Une relation nommée `r` dirigée.
*   `-[r:t]->` : Une relation nommée `r` de type `t` dirigée.
*   `(a)-[r]->(b)` : Deux nœuds `a` et `b` reliés par une relation `r`.

### 2. Requêtes de lecture (Interrogation)
*   **Renvoyer tous les nœuds de la base :**
    ```cypher
    MATCH (n) RETURN n
    ```
*   **Renvoyer tous les nœuds ayant le label `Product` :**
    ```cypher
    MATCH (n:Product) RETURN n
    ```
*   **Filtrer par propriété (deux syntaxes équivalentes) :**
    *   *Syntaxe imbriquée :*
        ```cypher
        MATCH (n:Product {name : "Neo4j in a nutshell"}) RETURN n
        ```
    *   *Syntaxe avec clause `WHERE` :*
        ```cypher
        MATCH (n:Product) WHERE n.name = "Neo4j in a nutshell" RETURN n
        ```
*   **Retourner uniquement une propriété spécifique d'un nœud :**
    ```cypher
    MATCH (prod:Product { name: "NoSQL Distilled" }) RETURN prod.name;
    ```
*   **Retourner les commandes d'un client spécifique (Martin) :**
    ```cypher
    MATCH (a:Customer { name: 'Martin' })-[:]->(x) RETURN x
    ```
*   **Trier les résultats (`ORDER BY`) :**
    ```cypher
    MATCH (customer:Customer) RETURN customer ORDER BY customer.name;
    ```
*   **Recherche par expression régulière (les clients dont le nom se termine par "in") :**
    ```cypher
    MATCH (customer:Customer) WHERE customer.name =~ ".*in$" RETURN customer.name;
    ```

### 3. Requêtes d'écriture (Création et Modification)
*   **Créer des nœuds et des relations séparément :**
    ```cypher
    CREATE (prod1:Product {name : "NoSQL Distilled"})
    CREATE (ord1:Orders)
    CREATE (ord1)-[:ORDERITEM { Price : 32.45 }]->(prod1)
    ```
*   **Créer le chemin complet en une seule instruction :**
    ```cypher
    CREATE (ord1:Orders)-[:ORDERITEM { Price : 32.45 }]->(prod1:Product {name : "NoSQL Distilled"})
    ```
*   **Mettre à jour ou ajouter une propriété (`SET`) :**
    ```cypher
    MATCH (n) WHERE n.name = "NoSQL Distilled" SET n.name = "NoSQL Distilled v1";
    ```
*   **Supprimer une propriété spécifique (`REMOVE`) :**
    ```cypher
    MATCH (n)-[r:ORDERITEM]->(m) REMOVE r.Price
    ```
*   **Supprimer une relation (`DELETE`) :**
    ```cypher
    MATCH (m)-[r:ORDERITEM]->(n:Product {name : "NoSQL Distilled"}) DELETE r
    ```
*   **Supprimer un nœud et toutes ses relations associées (`DETACH DELETE`) :**
    ```cypher
    MATCH (n:Product {name : "NoSQL Distilled"}) DETACH DELETE n
    ```
*   **Nettoyage complet de la base de données (Suppression de tous les nœuds et relations) :**
    ```cypher
    MATCH (n) DETACH DELETE n;
    ```

### 4. Chargement de données à partir d'un fichier CSV
```cypher
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM 'http://neo4j.com/docs/2.2.5/csv/artists-with-headers.csv' AS line
CREATE (:Artist { name: line.Name, year: toInt(line.Year) })
```

---

# PARTIE 3 : INTERFACE CONCEPTUELLE CLÉ-VALEUR

Pour les bases de données de type **Clé/Valeur** (comme Redis ou DynamoDB), les diapositives décrivent l'exploitation des données à travers les 4 opérations fondamentales du modèle **CRUD** :

*   **Create** : `create(key, value)` (crée un nouvel objet associé à sa clé)
*   **Read** : `read(key)` (lit un objet à partir de sa clé)
*   **Update** : `update(key, value)` (met à jour la valeur d'un objet existant)
*   **Delete** : `delete(key)` (supprime l'objet correspondant à la clé)