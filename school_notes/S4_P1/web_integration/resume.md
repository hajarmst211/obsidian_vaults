# Systeme distribue:
## Caracteristiques:
- **Pas d'horloge physique commune :** C'est une hypothèse importante qui introduit l'élément de distribution et engendre un **asynchronisme** inhérent aux processeurs.
- **Pas de mémoire partagée :** Contrairement à un système centralisé, la communication entre les entités se fait principalement par **transmission de messages**.
- **Séparation géographique :** Les entités sont physiquement distinctes et connectées via un réseau informatique (type LAN ou WAN).
- **Autonomie et hétérogénéité :**
    - Les entités sont "faiblement couplées".
    - Elles peuvent avoir des systèmes d'exploitation (OS), des processeurs (CPU) ou des architectures différentes.
    - Malgré ces différences, elles coopèrent en offrant des services ou en résolvant des problèmes conjointement.

## Les défis (Challenges) associés
- **L'hétérogénéité :** Faire travailler ensemble des composants différents.    
- **La transparence :** Masquer l'aspect distribué (l'utilisateur ne doit pas se soucier de l'emplacement des données).
- **La tolérance aux pannes :** Une panne d'un composant ne doit pas arrêter tout le système.
- **L'extensibilité (Scalability) :** Le système doit maintenir sa qualité de service même si le nombre d'utilisateurs augmente

---
# Architecture
### 1. L'Architecture Client / Serveur 
C'est l'architecture la plus populaire. Elle repose sur deux composants majeurs qui interagissent via un réseau :
*   **Le Serveur :** Un composant (processus) qui implémente un service spécifique (ex: fichiers, base de données) et attend les requêtes.
*   **Le Client :** Un composant qui demande un service au serveur en lui envoyant une requête et attend sa réponse.

Il a deux variantes :
*   <mark style="background: #BBFABBA6;">Client lourd :</mark>
	 - **Le Client fait le travail :** C'est lui qui fait les calculs, qui vérifie les règles (ex: calculer une facture, traiter une image).
	- **Le Serveur se repose :** Il ne sert pratiquement qu'à stocker les informations dans une base de données. 
* C'est plus difficile à maintenir mais cela réduit la charge du serveur.
*   <mark style="background: #BBFABBA6;">Client léger</mark> : Le client ne gère que la partie présentation. Tout se passe sur le **Serveur**. La partie "Métier" et la partie "Données" sont toutes les deux chez le serveur.
	* Mise a jour facile
	* Evolutif
### 2. L'Architecture 3 Tiers (Multi-tiers)
Cette architecture divise l'application en trois couches distinctes:
1.  **Couche présentation :** L'interface utilisateur (page web, GUI). Elle collecte les entrées et affiche les résultats.
2.  **Couche métier (Business logic) :** Elle implémente les règles logiques, effectue les calculs et achemine les données entre les couches.
3.  **Couche données :** Elle gère le stockage, la persistance et l'accès aux données 

### 3. L'Architecture Peer-to-Peer (P2P)
Dans cette architecture, la gestion des ressources est décentralisée : **chaque utilisateur (peer) est à la fois client et serveur**. Le cours détaille trois types de systèmes P2P :

*   **Système centralisé (ex: Napster) :** Un serveur central sert d'annuaire(liste) pour indexer les ressources et localiser les pairs que le client a besoin , mais les échanges se font directement entre pairs.
*   **Système purement décentralisé :**
    *   **Non structuré2:** Recherche par propager la demande entre les voisins, avec une durée de vie limitée (TTL).
    *   **Structuré:** On utilise une <mark style="background: #BBFABBA6;">Table de Hachage Distribuée (DHT)</mark>. Imagine que tous les utilisateurs sont placés sur un grand cercle (anneau). Chaque fichier a une place précise mathématiquement calculée sur ce cercle. <mark style="background: #BBFABBA6;">Tres rapide</mark>
*   **Système hybride:** Utilise des **"Super peers"** (courtiers) qui possèdent une forte capacité de calcul et de bande passante. Ils servent de serveurs locaux pour les petits utilisateurs autour d'eux. Ils listent ce qui est disponible à proximité.

---
# Thread
- Heriter la classe **thread**
- Creer une classe qui implemente la classe **runnable**
###### Etapes methode 1:
- Créer une classe qui hérite la classe **Thread**
- Redéfinir la méthode **runnable** 
- Creer un objet **thread
- Lancer le thread 

- Par défaut, chaque thread a <mark style="background: #ABF7F7A6;">la meme priorité </mark>que celui qui l'a crée.
---
# Systèmes Distribués
### Définition d'un Système Distribué (S.D.)
*   C'est un ensemble d'entités logicielles (appelées **processus**) s'exécutant indépendamment et en parallèle sur des machines.
### La Norme OSI (7 couches)**
C'est l'architecture de l'ISO pour les systèmes ouverts (pages 75-76) :
1.  **Application :** Protocoles applicatifs (Web, mail, transfert de fichiers).
2.  **Présentation :** Codage/décodage et représentation des données (entiers, chaînes).
3.  **Session :** Gestion de l'ouverture/fermeture et synchronisation de la connexion.
4.  **Transport :** Communication de bout en bout entre deux applications (TCP/UDP).
5.  **Réseau :** Construction des paquets et routage vers la destination (adresse IP).
6.  **Liaison :** Assemblage des trames et gestion de l'accès au support physique.
7.  **Physique :** Transmission et réception des données binaires sur le support.

### Le Modèle TCP/IP et l'Adressage
Le modèle TCP/IP est le standard de fait. Les applications font appel aux services de la couche transport :
*   **TCP (Transmission Control Protocol) :** Un protocole fiable (analogue à la téléphonie), point à point, mais plus lent à cause de la gestion du flux. Utilisé pour HTTP, FTP, SMTP.
*   **UDP (User Datagram Protocol) :** Un protocole peu fiable (analogue à la poste), permettant le <mark style="background: #FFB86CA6;">multicast/broadcast</mark>, très rapide car il tolère des pertes occasionnelles. Utilisé pour le streaming et la VOIP.
*   **Identification d'une application :** Pour communiquer, on utilise un couple de deux informations : l'**adresse IP** (identifiant de la machine) et le **numéro de port** (identifiant local de l'application sur la machine). Le port est un entier codé sur 16 bits (jusqu'à 65 536). Les ports inférieurs à 1024 sont réservés aux protocoles systèmes (ex: 80 pour HTTP, 21 pour FTP).

### Les Sockets et les Rôles de Communication
Une **socket** (ou prise) est le point de communication par lequel un processus émet ou reçoit des données. Chaque socket est associée à un port local et permet de communiquer avec un port distant sur une machine distante.
*   **Dissymétrie :** Le client et le serveur ont des rôles différents. C'est toujours le client qui doit connaître précisément la localisation du serveur (IP et port) et qui doit initier la connexion ou la communication. Le serveur, lui, communique via une socket liée à un port d'écoute spécifique.

### La Gestion des Flux (I/O) en Java
Avant d'envoyer des données sur le réseau, Java utilise le concept de **Flux (Streams)** pour encapsuler l'envoi et la réception :
1.  **Flux de caractères :** Utilisent le format Unicode (2 octets). Les classes de base sont `Reader`  et `Writer`.
    *   On distingue les **Flux** (source/destination) et les **Filtres** (traitement des données). Le préfixe `Buffered` (ex: `BufferedReader`) permet d'améliorer les performances en mettant les données en tampon.
    *   La classe `Reader` propose des méthodes comme `read()` (renvoie -1 à la fin), `skip()` ou `close()`.
    *   La classe `Writer` propose des méthodes `write()` pour écrire des caractères, des tableaux ou des chaînes de caractères (`String`).
2.  **Flux d'octets :** Utilisés pour les données binaires. Ils héritent de `InputStream` ou `OutputStream`.
    *   Les sous-classes incluent `FileInputStream` / `FileOutputStream` pour les fichiers, ou encore `ObjectInputStream` pour les objets.
    *   La méthode `available()` permet d'estimer le nombre d'octets restant à lire.

### Programmation des Sockets TCP en Java
Le protocole TCP fonctionne en mode connecté (point à point).
*   **Côté Serveur :** On utilise la classe `ServerSocket`. Le serveur enregistre son service sur un port et se met en écoute avec `accept()`. Lorsqu'un client se connecte, `accept()` retourne une socket de service spécifique pour ce client.
*   **Côté Client :** On utilise la classe `Socket`. Le client crée une socket en précisant l'adresse et le port du serveur, ce qui déclenche la demande de connexion.
*   **Manipulation des données :** Une fois la socket créée, on récupère les flux d'entrée/sortie (`getInputStream` / `getOutputStream`) pour communiquer.
*   **Classes Utiles :** `InetAddress` permet de manipuler les adresses IP (nom d'hôte, adresse numérique) et `NetworkInterface` permet de lister les cartes réseau de la machine.

### Service Multi-clients (Multithreading)
<mark style="background: #FFB86CA6;">Un serveur classique peut être bloqué(l'execution s'arrete a cette ligne) par l'appel à accept() ou par une lecture de données (readLine())</mark>. Pour servir plusieurs clients en parallèle :
*   Le serveur doit créer un **nouveau thread** pour chaque nouvelle connexion.
*   Le thread principal continue d'écouter sur le port, tandis que le thread enfant gère la communication spécifique avec le client qui vient de se connecter.

### Programmation des Sockets UDP en Java
Le protocole UDP fonctionne en mode <mark style="background: #BBFABBA6;">non connecté</mark>. Il n'y a pas d'établissement de connexion préalable.
*   **Principe :** On envoie des paquets indépendants appelés **Datagrammes**. L'émission est non bloquante, mais la réception est bloquante. Il n'y a aucune garantie sur l'ordre d'arrivée ou la réception effective.
* **Cons:**
	* Perte de paquets
	* L'ordre des paquets n'est pas garantie
	* Paquets destruits si le destinataire n'est pas a l'ecoute
	* Ne permet d'envoyer que des tableaux de byte

*   **Fonctionnement :** 
	* Le serveur crée un socket et attend un paquet sur un port fixe. 
	* Le client crée un socket pour accéder a la couche UDP.
	- Le client envoie son paquet à l'adresse du serveur(@IP , port). 
	* Le serveur peut répondre car l'adresse du client est inscrite dans le paquet reçu.

### Le Multicast UDP/IP
multicast : envoi de données dans un sous-groupe du réseau
broadcast: envoi de données dans tout le réseau

*   **Adressage :** Utilise les adresses de **Classe D** (de 224.0.0.0 à 239.255.255.255).
*   **Fonctionnement en Java :** On utilise la classe `MulticastSocket` (qui hérite de `DatagramSocket`).
*   **Gestion de groupe :** Un processus doit explicitement rejoindre un groupe avec la méthode `joinGroup()` en précisant l'adresse multicast et l'interface réseau. Il peut quitter le groupe avec `leaveGroup()`.
*   **Utilités :** 
	* Idéal pour la diffusion de flux vidéo (conférences, TV)
	* pour localiser des serveurs sur un réseau (localiser un  serveur DHCP).
- **cons:**
	* n'est pas fiable.
---
# RMI
 L'objectif du RMI est de permettre l'invocation d'une méthode sur un objet distant de manière aussi simple et identique qu'un appel local.
### Principes de fonctionnement et Middleware
Le système repose sur une distinction claire entre le client (appelant) et le serveur (appelé).
*   **Le Serveur :** Héberge des méthodes accessibles à distance. Un élément spécial, le **Squelette (Skeleton)**, reçoit les demandes, appelle la fonction réelle et renvoie le résultat.
*   **Le Client :** Appelle localement une fonction sur un élément spécial appelé **Talon (Stub)** ou Proxy, qui se charge de relayer la demande au serveur.

Ce mécanisme s'appuie sur un **Middleware** (intergiciel). Il s'agit d'une couche logicielle située <mark style="background: #FFB86CA6;">entre le système d'exploitation/réseau et l'application</mark>. Son rôle est d'offrir des services de communication entre les elements de l'application ou le system distribue. <mark style="background: #FFB86CA6;">La communication est via sockets TCP/UDP. </mark>

### Familles de Middleware et Modèle RPC/RMI
Il existe plusieurs solutions de middleware :
1.  **Appel de procédure/méthode à distance :** RPC (procédure), RMI (Java), CORBA (multi-langage), Web Services.
2.  **Envoi de messages/événements :** Famille MOM (Message Oriented Middleware) ou JMS.
3.  **Mémoire partagée :** Comme JavaSpace.
####  Modèle RPC/RMI 
-  interaction forte et synchrone (le client attend la réponse).
- Le client doit généralement connaître le serveur
- la communication est de type <mark style="background: #FFB86CA6;">point à point</mark>. 
#### Modèle a diffusion ou memoire partage
- Les messages sont asynchrones  et plus dynamiques.
- Acces aux informations par plusieurs elements
- PA necessaire de connaitre les elements accedant a la memoire

### Fonctionnement détaillé d'un appel RMI
Le processus se déroule en 9 étapes clés :
1.  Le client récupère une référence de l'objet distant via un **service de nommage (annuaire)**.
2.  Le client invoque la méthode sur cette référence.
3.  Le **Stub (Talon)** intercepte l'appel et effectue le **Marshalling** (compactage des données et paramètres).
4.  Le Stub envoie ces données au **Skeleton** côté serveur.
5.  Le Skeleton effectue l'**Unmarshalling** (décompactage) pour identifier l'opération.
6.  Le Skeleton appelle la méthode réelle sur l'objet serveur.
7.  Le Skeleton récupère le résultat, le compacte et le renvoie au Stub.
8.  Le Stub décompacte le résultat et le transmet au client.
9.  L'exécution du client reprend son cours.

*Note : Contrairement au RPC, le RMI intègre les concepts de l'orienté objet (polymorphisme, interfaces).*

### Caractéristiques techniques du RMI
Le RMI est une solution intégrée au JDK de Java.
Il est "transparent", car il permet de communiquer sans écrire de code réseau explicite.
Il utilise le protocole propriétaire **JRMP** (Java Remote Method Protocol) <mark style="background: #BBFABBA6;">basé sur les sockets.</mark>

**Règles de développement :**
*   Utiliser le package `java.rmi`.
*   Toute interface distante doit hériter de `java.rmi.Remote`.
*   Chaque méthode de l'interface doit déclarer `throws RemoteException`.
*   L'implémentation serveur doit hériter de `UnicastRemoteObject`.
*   Toutes les données passées en paramètres ou en retour doivent implémenter `Serializable`.

### Étapes de développement côté Serveur
1.  **Définir l'interface distante **
2.  **Implémenter l'interface :** Créer la classe qui contient la logique métier en étendant `UnicastRemoteObject`.
3.  **Initialiser le serveur :** Créer une instance de l'objet, démarrer le registre RMI (port par défaut 1099) via `LocateRegistry.createRegistry(1099)` et enregistrer l'objet avec un nom unique via `Naming.rebind("nom", objet)`.

### Étapes de développement côté Client
1.  Utiliser la **même interface** que le serveur.
2.  Obtenir une référence de l'objet distant en interrogeant le registre via `Naming.lookup("rmi://hôte:port/nom")`.
3.  Appeler les méthodes normalement à partir de cette référence.

### Le Registre de noms (RMI Registry)
C'est l'annuaire qui fait le lien entre un nom logique et une référence d'objet.
*   **Démarrage :** Peut se faire en ligne de commande (`rmiregistry`) ou via le code Java (`LocateRegistry`).
*   **Opérations de la classe `Naming` :** `bind` (enregistrer), `rebind` (remplacer), `unbind` (supprimer), `list` (énumérer) et `lookup` (rechercher).
*   L'URL utilisée suit le format : `rmi://hote:port/nomObjet`.

### Évolution du Stub/Skeleton et Sécurité
*   **Évolution :** Dans les anciennes versions (JDK 1.2), il fallait générer manuellement les fichiers Stub/Skeleton avec `rmic`. Depuis le JDK 5.0, ces classes sont générées automatiquement et le Skeleton n'est plus nécessaire en tant que fichier séparé.
*   **Gestion de la sécurité :** Charger du code via le réseau nécessite un **Security Manager** (`System.setSecurityManager`).
*   **Fichier Policy :** Pour que RMI fonctionne, il faut définir des permissions dans un fichier de politique (`.policy`) afin d'autoriser les connexions réseau sur des ports spécifiques (ex: 1099 pour le registre et les ports supérieurs à 1024 pour la communication).
---
# web service
### Évolution des architectures et Urbanisation
Historiquement, les entreprises utilisaient des architectures distribuées, mais l'interopérabilité restait difficile en raison de l'hétérogénéité (langages de programmation différents, plateformes variées). 
Le document oppose deux modèles :
*   **L'architecture "en spaghetti" :** Caractérisée par des interconnexions redondantes, un développement coûteux, une grande complexité et une maintenance difficile.
*   **L'architecture "Urbanisée" :** Elle propose un découpage en modules autonomes et localise précisément les zones d'échange d'information (via un ESB - Enterprise Service Bus).

### Le concept d'Interopérabilité
L'interopérabilité est la capacité de systèmes hétérogènes à échanger des données et partager des fonctionnalités de manière fluide. Elle se décline en quatre niveaux :
1.  **Fondamentale :** Le simple transfert de données brutes.
2.  **Structurelle :** L'utilisation de formats d'échange standardisés (ex: JSON).
3.  **Sémantique :** La compréhension mutuelle de la signification des données.
4.  **Organisationnelle :** La coopération selon des processus métiers et des règles partagées.

### La notion de Service et ses caractéristiques
Le cours retrace l'évolution de l'abstraction logicielle : de la procédure vers le module, puis l'objet, le composant, et enfin le **Service**.
Un service est défini comme une unité autonome de fonctionnalités métier, accessible à distance, décrite par un **contrat** et indépendante de son implémentation (boîte opaque).

Ses caractéristiques clés sont :
*   **Autonomie :** Fonctionne indépendamment.
*   **Faible couplage :** Dépendances minimales entre services.
*   **Réutilisabilité :** Utilisable par plusieurs applications.
*   **Abstraction :** Cache la complexité interne.
*   **Composabilité :** Peut être combiné pour créer des fonctions complexes.
*   **Discoverability :** Peut être découvert via un registre.
*   **Stateless :** Ne stocke pas de données entre les requêtes.

### L'architecture Orientée Services (SOA)
Le SOA est un style architectural (proposé par Gartner en 2000) visant à construire des systèmes flexibles. Dans cette topologie :
*   La logique est organisée en modules (services).
*   Chaque service expose une interface claire (le contrat).
*   Le consommateur ne connaît pas l'implémentation interne.

Le principe repose sur un triangle d'acteurs : le **Fournisseur** (publie le service), le **Registre** (facilite la recherche/broker) et le **Consommateur** (localise et invoque le service).

### Fondamentaux des Web Services
Un Web Service est une implémentation concrète de SOA mise à disposition sur Internet. Il est associé à une URL, accessible via des protocoles standards (HTTP), indépendant des plateformes et auto-descriptif (XML). On distingue deux grandes familles : **SOAP** et **REST**.
*Note : Le document rappelle la différence entre Internet (le réseau) et le Web (système d'information utilisant HTTP).*

### Le protocole SOAP (Simple Object Access Protocol)
SOAP est un protocole d'échange d'informations basé sur des messages XML, standardisé par le W3C. Il est indépendant des langages et peut utiliser plusieurs protocoles de transport (HTTP, SMTP, POP).
Deux styles d'échange existent :
*   **Style RPC :** Invocation technique, couplage fort.
*   **Style Document :** Échange de messages XML orienté métier, couplage faible.

**Structure d'un message SOAP :**
1.  **Envelope (racine) :** Contient tout le message.
2.  **Header (optionnel) :** Informations d'en-tête (authentification, transactions).
3.  **Body (obligatoire) :** Contenu de l'appel (méthode, paramètres) ou de la réponse.
4.  **Fault (optionnel) :** Détails sur les erreurs rencontrées.

### WSDL (Web Service Description Language)
Le WSDL sert à décrire l'interface publique d'un service SOAP en XML. Il définit :
*   **Types :** Les types de données utilisés.
*   **Messages :** Les données transmises.
*   **PortTypes (ou Interface) :** Les opérations disponibles (Input/Output).
*   **Bindings :** Le protocole de transport et le format.
*   **Service :** L'adresse réseau (URL) du service.
Il existe quatre types d'opérations : *One-way* (réception seule), *Request-response* (classique), *Solicit-response* et *Notification*.

### UDDI (Universal Description, Discovery and Integration)
UDDI est le standard pour publier et découvrir des services web. Il fonctionne comme un annuaire organisé en trois parties :
*   **Pages blanches :** Infos générales sur l'entreprise.
*   **Pages jaunes :** Classification par secteur métier.
*   **Pages vertes :** Détails techniques et accès aux WSDL.
Les entités principales sont `BusinessEntity`, `BusinessService`, `BindingTemplate` et `tModel` (modèle technique). L'accès se fait via des API SOAP dédiées (Inquiry, Publish, Security).

### Mise en œuvre Java : JAX-WS
Java propose l'API **JAX-WS** pour les services SOAP. Elle repose sur des annotations pour mapper les classes Java vers le WSDL.
Deux approches de développement :
1.  **Top-down (Contract First) :** On part du WSDL pour générer les classes Java (outil `wsimport`).
2.  **Bottom-up (Contract Last) :** On part du code Java annoté pour générer le WSDL (outil `wsgen`).

**Annotations clés :**
*   `@WebService` : Définit la classe comme un service.
*   `@WebMethod` : Expose une méthode.
*   `@WebParam` / `@WebResult` : Nomme les paramètres et le retour dans le XML.
*   `@SOAPBinding` : Définit le style (RPC ou Document).

### Liaison de données avec JAXB
JAXB (Java Architecture for XML Binding) assure la conversion entre objets Java et XML. 
*   **xjc** convertit un schéma XML (XSD) en classes Java.
*   **schemagen** crée un schéma XML à partir de classes Java.
Des annotations comme `@XmlRootElement` ou `@XmlElement` permettent de contrôler finement la structure XML générée.

---

### Les Services Web REST (REpresentational State Transfer)
Contrairement à SOAP, REST n'est pas un protocole mais un **style d'architecture** basé sur les standards du Web (HTTP).
Principes fondamentaux :
1.  **Ressource :** Tout objet identifiable par une **URI** (ex: `/books/1`).
2.  **Interface uniforme :** Utilisation des verbes HTTP (GET, POST, PUT, DELETE) pour les opérations CRUD.
3.  **Représentations :** Une ressource peut être transmise en JSON, XML ou HTML.
4.  **Stateless :** Aucune session n'est stockée sur le serveur.
5.  **HATEOAS :** Le client découvre les actions possibles via des liens hypermédias fournis dans les réponses.

**Règles de nommage des URI :** Elles doivent utiliser des noms (pluriel de préférence) et refléter une hiérarchie logique (ex: `/books/1/authors`).

### Rappel sur le protocole HTTP
Le document rappelle le fonctionnement de HTTP : 
*   **Méthodes :** GET (lecture), POST (création), PUT (remplacement), DELETE (suppression), PATCH (modification partielle).
*   **Codes de statut :** 2xx (succès), 3xx (redirection), 4xx (erreur client comme 404), 5xx (erreur serveur).
*   **Anatomie :** Ligne de commande, En-têtes (Headers) et Corps (Body).

### Mise en œuvre Java : JAX-RS (Jersey)
**JAX-RS** est la spécification Java pour REST. **Jersey** en est l'implémentation de référence.
On utilise des annotations sur des POJO pour exposer des ressources :
*   `@Path` : Définit l'URI de la ressource.
*   `@GET`, `@POST`, etc. : Associe une méthode Java à un verbe HTTP.
*   `@Produces` / `@Consumes` : Spécifie le format (MIME type) des données (ex: `application/json`).
*   **Injection de paramètres :** `@PathParam` (variable dans l'URI), `@QueryParam` (paramètre après le `?`), `@FormParam` (données de formulaire), `@Context` (accès aux infos HTTP).

**WADL :** C'est l'équivalent (optionnel) du WSDL pour REST, décrivant les ressources et méthodes offertes.

### Format de données JSON
JSON (JavaScript Object Notation) est le format privilégié pour REST car il est léger, lisible par l'humain et indépendant des langages. Il repose sur deux structures :
1.  **Objet :** Collection de couples clé/valeur `{...}`.
2.  **Tableau :** Liste ordonnée de valeurs `[...]`.

### Documentation et Conclusion
Pour documenter automatiquement les API REST, on utilise la spécification **OpenAPI** et des outils comme **Swagger**. Cela permet de générer une interface interactive pour tester les services.

**Comparaison finale SOAP vs REST :**
*   **SOAP :** Strict, fortement typé (WSDL), XML uniquement, idéal pour les systèmes critiques complexes.
*   **REST :** Souple, léger, supporte JSON/XML, plus rapide à implémenter, idéal pour les applications web et mobiles modernes.

---
# Fast API
Voici un résumé très détaillé du Chapitre 5, consacré à **FastAPI** et aux API Web modernes.

---

### Introduction et Motivations
Le cours commence par expliquer pourquoi FastAPI est pertinent aujourd'hui. Bien que les services REST puissent être implémentés en Java ou JavaScript, le langage **Python** est devenu la référence dans les domaines de la **Data Science** et de l'**Intelligence Artificielle**. FastAPI apparaît comme le framework moderne de choix pour créer des API REST rapidement au sein de cet écosystème.

### Présentation de FastAPI et caractéristiques clés
FastAPI est défini comme un framework Python moderne, performant et rapide. Ses principales forces sont :
*   **Rapidité :** Des performances extrêmement élevées, comparables à Node.js ou Go, grâce à l'utilisation de *Starlette* (pour le web) et *Pydantic* (pour les données).
*   **Simplicité :** Un code intuitif basé sur les annotations Python qui accélère le développement.
*   **Documentation automatique :** Générée nativement sans effort supplémentaire.
*   **Standards :** Basé sur OpenAPI (Swagger) et JSON Schema.
*   **Asynchronisme :** Support natif de la programmation asynchrone pour une meilleure gestion des entrées/sorties.

### Les Endpoints (Points d'accès)
Un endpoint est un point d'interaction avec l'API. Dans FastAPI, ils sont définis par des **décorateurs** appliqués à des fonctions Python. Ces décorateurs associent une méthode HTTP (GET, POST, PUT, DELETE) et une URL à une logique métier spécifique. 

### Gestion des paramètres des Endpoints
Le cours détaille trois types de paramètres pour interroger les données :
1.  **Path parameters (Paramètres de chemin) :** Utilisés pour identifier une ressource spécifique directement dans l'URL (ex: `/product/{id}`).
2.  **Query parameters (Paramètres de requête) :** Utilisés pour filtrer ou analyser les données via des chaînes de requête (ex: `?year=2024`).
3.  **Body parameters (Paramètres de corps) :** Utilisés pour envoyer des données complexes au format JSON. FastAPI utilise les modèles **Pydantic** pour valider automatiquement que les données reçues correspondent au format attendu.

### Accès aux bases de données et SQLModel
Pour l'accès aux données, FastAPI peut utiliser du SQL brut ou des ORM classiques comme SQLAlchemy. Cependant, le cours met en avant **SQLModel**, un outil qui combine la puissance de SQLAlchemy (pour la base de données) et de Pydantic (pour la validation). 
Le processus inclut :
*   La création d'un **Modèle** (classe Python) définissant la structure de la table.
*   La création d'un objet **Engine** pour gérer le pool de connexions.
*   La gestion de **Sessions** transactionnelles pour exécuter les requêtes.

### Schémas et opérations CRUD
Le cours souligne l'importance de définir des **schémas** distincts pour valider les entrées et les réponses de l'API lors des opérations CRUD (Create, Read, Update, Delete). Par exemple, on peut avoir un schéma spécifique pour la création d'un objet et un autre pour sa lecture, permettant ainsi de masquer certains champs sensibles ou techniques.

### Documentation interactive automatique
L'un des grands avantages de FastAPI est la génération automatique d'une interface de test interactive. En se rendant sur l'URL `/docs`, les développeurs accèdent à **Swagger UI**, qui permet de visualiser tous les endpoints disponibles, de consulter les modèles de données et de tester les appels API directement depuis le navigateur.

### Programmation Synchrone vs Asynchrone
FastAPI permet de définir des fonctions `async def`. Cette approche est particulièrement bénéfique pour les tâches gourmandes en temps d'attente (I/O), comme les appels à une base de données, les requêtes vers des API externes ou la lecture de fichiers. Pendant qu'une fonction attend une réponse, le serveur peut traiter d'autres requêtes, ce qui optimise les performances globales.

### Structuration et organisation de l'application
Pour les projets d'envergure, il est déconseillé de tout coder dans un seul fichier. Le cours propose une structure modulaire :
*   **Regroupement par modules :** Utilisation de fichiers séparés pour les modèles, les schémas et les routes.
*   **APIRouter :** Un outil permettant de créer des blocs de routes avec des préfixes de chemin (ex: `/api/v1`), des tags pour la documentation et des dépendances groupées (comme la sécurité).

### Gestion des dépendances et scalabilité
Pour rendre l'application "scalable" (évolutive), il est crucial de séparer les responsabilités. Le cours montre comment utiliser l'injection de dépendances pour gérer, par exemple, la session de base de données. Cela permet de s'assurer que chaque requête dispose de sa propre connexion et que celle-ci est correctement fermée après usage.

### Conclusion sur FastAPI
FastAPI se distingue par sa rapidité de développement et sa documentation automatique via Swagger UI. La séparation claire entre les tables SQL (Models), la validation (Schemas) et la logique (Routers) facilite grandement la maintenance à long terme.

### Conclusion générale du module
Le cours se termine sur une réflexion globale :
*   **Évolution des paradigmes :** On est passé d'une programmation bas niveau (threads/sockets) à une stratification de l'abstraction avec les API REST.
*   **Écosystème riche :** REST est le standard, mais il est complété par gRPC (performance binaire), GraphQL (flexibilité) et WebSockets (temps réel).
*   **Rôle central de l'IA :** Les Web API sont aujourd'hui le moteur principal pour exposer des modèles de Machine Learning de manière robuste.
*   **Finalité :** L'objectif est de bâtir des systèmes interopérables capables de distribuer non seulement de la donnée, mais de la véritable "intelligence" logicielle.









