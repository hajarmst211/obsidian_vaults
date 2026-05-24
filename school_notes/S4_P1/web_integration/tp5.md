# partie 1
### Teste du serveur
![[Pasted image 20260324005203.png]]
![[Pasted image 20260324005415.png]]
![[Pasted image 20260324005424.png]]

### question 5: le role de chaque classe de l'application

###### **Méthode `startServer()`**
- **Configure le serveur Jersey** :
  - Crée un `ResourceConfig` (configuration de Jersey)
  - Spécifie le package à scanner pour trouver les ressources REST (`org.example.resource`)
  - Désactive/désactive WADL (`WADL_FEATURE_DISABLE`)

- **Crée et démarre le serveur Grizzly** :
  - Utilise `GrizzlyHttpServerFactory` pour créer un serveur HTTP Grizzly
  - Associe l'URI de base avec la configuration Jersey

###### **Méthode `main()`**
- **Point d'entrée de l'application** :
  - Démarre le serveur
  - Affiche la liste des endpoints disponibles dans la console
  - Attend une entrée utilisateur (Appuyez sur Entrée) pour arrêter le serveur
  - Gère l'arrêt propre du serveur


### Question 6: Quelle annotation permet de définir une ressource REST ? Où est-elle utilisée dans le projet ?

L'annotation `@Path` permet de définir une ressource REST. Elle est utilisée dans le projet dans la classe `ProductResource` (package `org.example.resource`).

L'annotation `@Path` est également utilisée au niveau des méthodes pour définir des sous-chemins
### Question 7: Quelles sont les méthodes HTTP utilisées dans cette API ? Donner un exemple pour chacune.

Les méthodes HTTP utilisées dans cette API sont :

- **GET** pour Récupérer des ressources

- **POST**  pour créer une nouvelle ressource  ```

- **PUT** pour modifier une ressource existante


- **DELETE** pour supprimer une ressource
### Question 8: Compléter la méthode deleteProduct dans ProductService pour supprimer un produit en fournissant son ID et retourne true après (false sinon)
- La modification est ajoutee au niveau du code aussi
```java
public boolean deleteProduct(Long id) {
    if (products.containsKey(id)) {
        products.remove(id);
        System.out.println("Produit supprimé avec ID: " + id);
        return true;
    }
    return false;
}
```

### Question 9: Compléter la méthode getProductById dans ProductResource qui permet de retourner un produit selon son id
- NB: la modification est faite au code aussi
```java
@GET
@Path("/{id}")
public Response getProductById(@PathParam("id") Long id) {
    Product product = productService.getProductById(id);
    if (product != null) {
        return Response.ok(product).build();
    } else {
        return Response.status(Response.Status.NOT_FOUND)
                .entity("{\"error\": \"Produit avec l'ID " + id + " non trouvé\"}")
                .build();
    }
}
```

### Question 10: A quoi sert la classe Response et comment l'utiliser ?

La classe `Response` de JAX-RS permet de construire des réponses HTTP personnalisées avec un contrôle précis sur :

- **Le code de statut HTTP** (200 OK, 201 Created, 404 Not Found, etc.)
- **Les en-têtes HTTP** (Content-Type, Location, etc.)
- **Le corps de la réponse** (JSON, XML, texte)
- **La localisation** (pour indiquer où la ressource a été créée)


### Question 11: Utiliser Postman ou navigateur pour tester le endpoint /products
 
##### get product:
![[Pasted image 20260324010803.png]]
##### get product by id
![[Pasted image 20260324010826.png]]

##### Rechercher avec keyword: iphone
![[Pasted image 20260324010846.png]]

##### product count
![[Pasted image 20260324010927.png]]

### Question 12: Tableau WADL complété

| Élément WADL           | Nom trouvé / valeur                                                                                                                                                                       | Rôle / Description                                                                                                                                        |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`<application>`**    | `xmlns="http://wadl.dev.java.net/2009/02"` (implicite)                                                                                                                                    | Racine du document WADL, contient toutes les ressources et méthodes. Inclut les docs générées par Jersey et la section grammars                           |
| **`<resources>`**      | `base="http://localhost:9090/api/"`                                                                                                                                                       | Conteneur des ressources (ex : /products). Définit l'URI de base pour toutes les ressources de l'API                                                      |
| **`<resource>`**       | `path="/products"`<br>`path="/form"`<br>`path="/uri-info"`<br>`path="/search"`<br>`path="/{id}"`<br>`path="/count"`                                                                       | Décrit un endpoint spécifique et son chemin relatif. Chaque sous-ressource est imbriquée dans la ressource parente `/products`                            |
| **`<method>`**         | `name="GET"` (id="getAllProducts")<br>`name="POST"` (id="createProduct")<br>`name="PUT"` (id="updateProduct")<br>`name="DELETE"` (id="deleteProduct")                                     | Méthode HTTP utilisée (GET, POST, PUT, DELETE). L'attribut `id` correspond au nom de la méthode Java dans le code                                         |
| **`<request>`**        | Contient `<representation mediaType="application/json"/>` ou `<param>`                                                                                                                    | Paramètres attendus pour la requête (queryParam, pathParam, formParam). Définit ce qui doit être envoyé au serveur                                        |
| **`<response>`**       | `<representation mediaType="application/json"/>`                                                                                                                                          | Type de réponse attendue (representation). Toutes les réponses de l'API sont au format JSON                                                               |
| **`<representation>`** | `mediaType="application/json"`<br>`mediaType="application/x-www-form-urlencoded"`                                                                                                         | Format des données (ex : application/json). JSON pour les endpoints REST classiques, formulaire pour `/form`                                              |
| **`<doc>`**            | `jersey:generatedBy="Jersey: 2.35 2021-09-03 11:06:43"`<br>`jersey:hint="This is simplified WADL..."`<br>`title="Generated" xml:lang="en"`                                                | Documentation associée à la ressource ou méthode. Indique la version de Jersey et comment obtenir le WADL complet avec `?detail=true`                     |
| **`<param>`**          | `name="id" style="template" type="xs:long"`<br>`name="q" style="query" type="xs:string"`<br>`name="name" style="query" type="xs:string"`<br>`name="price" style="query" type="xs:double"` | Description et type des paramètres (nom, type, style : query/path/form). Les paramètres de formulaire sont imbriqués dans `<representation>` pour `/form` |
