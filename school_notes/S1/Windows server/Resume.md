

le controle de l'espace disk se fait par NTFS quotas

## Soul. Admin Windows
*   Installer OS bien fonctionner
*   La sécurité et la gestion
*   Offrir d'un système informatique bien sous Windows.

## Active Directory

**Cœur du système de création d'utilisateurs dans un réseau Windows.**

**Rôle:**
*   Centraliser la gestion (utilisateur, gpe, ordi...)
*   Depuis un seul endroit
*   Appliquer des stratégies de sécurité (GPO)
*   Organiser le réseau. (unité au UO)

## Structure Logique d'un AD (hiérarchie)
- **Arbre**: collection de domaines avec le meme namespace
- **Domaine**: logical grouping of projects. Chaque Domaine est organize par un ou pleusieurs DC: controlleur de domaine
*   **Forêt** -> Ensemble complet de tous les domaines sécurisé. Groupe de domaines.

*   **Catalogue Global:**
    *   Base de données centrale qui regroupe des informations sur les objets, les utilisateurs et les attributs d'un AD
---> permet de chercher et d'authentifier rapidement les objets de toute la forêt Active Directory.

## Structure Physique d'un AD
**Comment les serveurs sont reliés**

*   **Contrôleur de domaine:**
    *   <mark style="background: #BBFABBA6;">Serveur qui contient l'AD</mark>
    *   Et gère l'authentification des utilisateurs.
*   **Site:**
    *   Ensemble d'ordi reliés par un <mark style="background: #BBFABBA6;">réseau (LAN)</mark>
*   **Réplication:**
    *   Processus d'échange de données entre les CD pour garder la base à jour.

---

## FSMO
**Flexible Single Master Operations**
**C'est quoi?**
*   Des rôles dans AD qui ne peuvent être exercés que par un seul DC pour éviter les conflits.
*   D'abord, un domaine peut avoir plusieurs DC pour assurer la redondance, la disponibilité et la répartition des charges.
*   Cependant, certaines opérations critiques doivent être gérées par un seul DC.

**Exemple:**
*   Attribuer un RID(relative ID) unique pour les identifiants en même temps. n'est pas possible? **car**: la prévention des collisions d’identifiants et à la cohérence distribuée du système dans toute la foret.
*   Donc on utilise les FSMO

**Rôles FSMO:**
*   **Rôle forêt:**
    *   **Maître du Schéma:**
        *   Gère quels types d'objets peuvent exister dans AD
        *   et quelles infos ils peuvent avoir.
    *   **Maître d'attribution des noms de domaines (AD DNS)**
        *   Ajout et suppression des domaines
        *   Fait apparaître les domaines.
*   **Rôles domaine:**
    *   **RID Master:**
        *   Identifiants uniques aux objets
    *   **Émulateur PDC:**
        *   Synchronise l'heure.
        *   Gère les anciens systèmes.
        *   GPO répliquées.
    *   **Infrastructure Master:**
        *   Mise à jour entre domaines.

## Partitions AD
---> Portion distinct de base de donnée du AC.
- **BUT:** pour organiser et faciliter la réplication.
- chaque répartition a ses propres:
	- <mark style="background: #FFB86CA6;">Espace de noms(elle commence à une racine différente)</mark>
	- <mark style="background: #FFB86CA6;">Ensemble d'objets de regles de replications (à quels DCs elle est copiée).</mark>
	- <mark style="background: #FFB86CA6;">Ensemble d'objets de domaine</mark>
	
## NTDS
---> moteur interne de stockage et de fonctionnement de l’annuaire.
---> C’est le **fichier physique** dans lequel **toutes les informations AD sont stockées**
---> Chaque **objet** y est identifié par un **GUID unique**.


## AD DS: active directory domaine services
---> celui qui <mark style="background: #ADCCFFA6;">transforme un simple serveur</mark> Windows en **contrôleur de domaine (Domain Controller)**.
---> Lorsqu’un utilisateur se connecte, AD DS vérifie ses identifiants
---> <mark style="background: #ADCCFFA6;">Organise le réseau</mark> en **domaines**, **arbres** et **forêts**
---> Il a la base de donnée principale de AC, les contrôleur de domaines la repartent entre eux
- <mark style="background: #ADCCFFA6;">Sa base de donne</mark> est situe a: `C:\Windows\NTDS\NTDS.dit`.  Ce fichier contient :
	- les comptes utilisateurs, mots de passe (hashés), groupes, ordinateurs ;
	-  la configuration du domaine ;
	- les objets et attributs définis dans le schéma AD.
---

## DFS (Distributed File System)
<mark style="background: #FFF3A3A6;">---></mark> organise et réplique des fichiers à travers plusieurs serveurs, tout en donnant aux utilisateurs l’impression qu’ils accèdent à un **seul espace de stockage centralisé**.

*   Système qui permet de regrouper les dossiers situés sur de plusieurs serveurs.
*   Si plusieurs serveurs ont un dossier partagé avec DFS les utilisateurs y accèdent comme s'il s'agissait d'un seul dossier.
*   Simplifie l'accès.

- Le service DFS repose sur **deux mécanismes complémentaires** :
### DFS Namespace 
- C’est la **vue logique** du système de fichiers distribué.  
- Il regroupe plusieurs partages réseau (shares) sous un **nom unique**.  
- L’arborescence DFS est stockée dans Active Directory et répliquée entre serveurs.
*   Chaque lien DFS est accessible via une racine.
* *   **Proximité:**
    *   Serveur le plus proche
    *   Temps de réponse.

### DFS Replication (DFSR)

---> C’est le **mécanisme de synchronisation** des données entre serveurs.  
Il utilise un algorithme appelé **RDC (Remote Differential Compression)**, qui <mark style="background: #FFF3A3A6;">ne transfère que les blocs modifiés dans un fichier</mark>
Par exemple, si un document de 500 Mo change de 2 Mo, DFSR ne transfère que ces 2 Mo.



## IIS (Internet Information Services)

*   Serveur Web Windows
*   Gère les sites
*   Web sur Windows.


---


##### RODC (Read-Only Domain Controller)
--->  Ne peut pas modifier la BD de la copie de l'AD.

## GPO (Group Policy Object)

*   Objet de stratégie de Groupe
*   Ensemble d'ordre appliqué
    à un ou plusieurs utilisateurs
    pour contrôler leur comportement
    dans le domaine Windows.

## Gestion des objets dans ADDS

**Utilisateurs**
*   Local
*   Global
*   Universel

**Groupes**
*   Sécurité
*   Distribution
*   Autres types d'objets

**Utilisateurs et ordinateurs**
*   Utilisateurs d'un domaine
*   Visibles par les autres
*   Du domaine
*   Pas visible par les autres
*   Du domaine

## Ordre d'application des GPO

*   **LSDOU**
    *   Local
    *   Site
    *   Domaine
    *   OU (Unité d'organisation)

*   **Quelle règle l'emporte quand plusieurs règles sont définies?**
    *   La dernière règle appliquée
    *   A la priorité.
*   Pour ça que les GPO
    *   Ont la priorité.


---

## Reverse DNS

*   IP -> Domaine name

## NETBIOS

*   Traduit le Nom NETBIOS
    d'un ordi en IP
*   Ex: Adminum -> 192.0.0.1 (IP)
*   IP: Permet de reconnaître
    les noms que le réseau
    local.

**NETBIOS**
*   Local
*   Nom ordi -> IP
*   Nom court
*   Ancien

**DNS**
##### DNs resolver
Un DNS resolver (ou serveur récursif) est un serveur client du DNS qui fait le travail de trouver la réponse complète à une requête DNS au nom de l’utilisateur

##### DNS autoritaire
Un serveur DNS autoritaire est celui qui détient la réponse officielle pour un domaine.

## Requete DNS

C'est la demande envoyée au DNS Types de requêtes.

*   **Client (Utilisateurs)**
    *   **Recursive:**
        *   DNS résout
        *   complètement le
        *   nom pour le
        *   client.
*   **Client (Serveurs)**
    *   **Iterative:**
        *   DNS donne
        *   seulement une
        *   piste (un autre
        *   serveur à contacter)
        *   Le client continue la
        *   recherche.

## Types du serveur DNS

*   **Primaire:**
    *   Gère les
    *   zones DNS.
*   **Secondaire:**
    *   Copie de la
    *   zone DNS.
*   **Cache:**
    *   Stockage
    *   temporaire des résultats.
    *   Accélère la
    *   résolution
    *   Réduit les charges
    *   Sur le serveur
    *   DNS.

---


## WQL (Windows Query Language)

*   Fiche qui spécifie une
    condition afin de permettre
    de cibler l'application
    d'un GPO uniquement
    sur les entités répondant
    à certains critères comme
    la RAM, Version Windows
    ou le type de Processeur
    Langage de WQL.
*   On l'utilise pour des
    stratégies globales.

## Version Windows

```sql
SELECT * FROM WIN32_OperatingSystem WHERE Version LIKE "10%"
```

## Ou RAM

```sql
SELECT * from WIN32_ComputerSystem where TotalPhysicalMemory > 4096
```
(en Mo)

## Type de Processeur

```sql
SELECT * from WIN32_Processor WHERE Name LIKE "%Intel%"
```

---

## Définition et Avantages du DC

*   Service de gestion
    centralisée sur Windows.
*   Serveur qui permet de gérer
    les utilisateurs, ordi et
    ressources d'un réseau.

**AV:**
1.  **Centralisation:**
    Tout est géré depuis
    cet endroit.
2.  **Sécurité:**
    Contient un contrôleur
    d'accès (GPO).
3.  **Organisation:**
    OU, groupes...

## Différents types de forêts/arborescence

*   **Forêt:** Ensemble
    d'arborescence.
*   **Arborescence:** Plusieurs domaines
    liés hiérarchiquement.
*   **Domaine:** Unité de
    base de l'AD contenant les
    objets.

## Rôle du DC et du RODC

**Le DC?**
*   C'est l'exécutif de l'AD,
    il gère et authentifie les
    utilisateurs, et gère la base
    de données NTDS.

**Le RODC?**
*   Version lecture seule du DC.
*   Fournit l'authentification
    locale sans permettre de modifier
    l'AD.

## Types de groupes à étendre

*   Types de Distribution (emails).
*   Sécurité (permissions).

**Types d'étendue:**
*   Local, global, universel.

## Héritage GPO, Comment peut-on le bloquer?

*   Hériter des GPO du niveau
    supérieur (LSDOU).
*   Il peut être bloqué
    manuellement.

## Rôles FSMO

*   Maître Schéma.
*   Maître des noms de domaines.
*   Maître Infrastructure.
*   Maître RID.
*   Émulateur PDC.

*Il faut comprendre le rôle de chacun.*

---


## DHCP

*   Attribue automatique
    les IP et le
    serveur.
*   Lorsque le serveur utilise
    du nom local.
*   Passerelle @Routeur.
*   DNS.
*   Durée de bail: durée
    de validité de @IP.
*   WINS.

## Demande (DHCP Discover)

1.  Réponse (DHCP Offer).
2.  Acceptation (DHCP Request).
3.  Confirmation (DHCP Acknowledge).

## Agent de relais DHCP

*   Intermédiaire entre les
    clients DHCP et le serveur
    DHCP.
*   Si le serveur DHCP
    et le client ne sont
    pas sur le même
    sous-réseau.

## APIPA (Automatic Private IP Addressing)

*   Automatique @IP.
*   En l'absence d'un serveur
    DHCP.
*   Si le PC ne reçoit pas
    d'adresse du DHCP.
*   Il s'attribue une IP de
    la plage?
    169.254.0.1 à 169.254.255.254.
*   Masque 255.255.0.0.
*   Communication que l'on a avec
    les ordinateurs @APIPA
    sur un réseau local.
*   Automatique
    sans qu'il soit
    configuré.

## DNS

*   Nom de domaine -> @IP.
*   FQDN (Fully Qualified Domain Name).
*   Chemin complet d'un
    objet dans la hiérarchie.
*   **DNS:**
    *   Hôte: MOC4
    *   Nom de domaine: Enaus.
    *   FQDN: MOC4.Enaus.

---


## Questions et Réponses (partie 1)

16. **IIS?**
    *   Serveur web Windows
    *   Il est le plus intégré
        dans l'écosystème de
        Windows et d'autres
        serveurs web.
17. **Comment installer IIS?**
    *   Gestionnaire de serveur
    *   Ajouter des rôles
        et fonctionnalités.
18. **Fichiers de configuration importants d'IIS et à quoi servent-ils?**
    *   Fichier `web.config`
        pour la configuration
        avancée.
    *   `Host.config`: réglages
        globaux.
19. **Explique comment héberger un site web simple avec IIS?**
    *   Créer un fichier HTML
    *   Le placer dans le
        répertoire racine du
        site configuré dans IIS.

20. **DHCP?**
    *   Attribution automatique
        des IP.
21. **Fail DHCP?**
    *   Attribution automatique et
        manuelle des IP.
22. **APIPA?**
    *   Attribue une IP
        automatique en cas
        d'absence de DHCP
        (169.254.x.x).

23. **Domaine name? @IP?**
24. **Types de requêtes?**
    *   Req récursive:
        Le serveur fait tout le
        travail.
    *   Req itérative:
        Le serveur ne donne qu'une
        piste vers les autres
        serveurs.
25. **Type DNS?**
    *   Primaire: gère les
        zones DNS.
    *   Secondaire: Copie
        du DNS primaire.
    *   Cache: stocke
        temporairement les résultats.

---

## Questions et Réponses (partie 2)

21. **Méthode d'authentification pour restreindre l'accès aux utilisateurs?**
    *   Auth Windows.
22. **Contrôle total?**
    *   Tous les droits d'accès.
    *   Lecture, écriture, suppression.
23. **Port DNS?**
    *   53.
24. **C'est quoi PTR?**
    *   Enregistrement DNS
        pour la résolution inverse.
    *   IP -> Nom de domaine.
    *   Rôle: faciliter la recherche
        du nom inverse.
25. **Espace disque minimal DC?**
    *   20 GO.
26. **Commande qui force la mise à jour des GPO sur un client Windows?**
    *   `gpupdate /force`.
27. **Durée de Bail pour DHCP configuré sur un réseau local?**
    *   24 heures.

28. **FAT32?**
    *   Copie et cryptage.
29. **Type de contrôleur pour les IPs ne devant être visibles qu'à l'extérieur?**
    *   Host only.
30. **Si un serveur fichiers montre des valeurs élevées pour l'I/O et le fichier d'attente du disque?**
    *   On doit fragmenter le
        disque pour réorganiser
        le fichier, accélérer
        l'accès aux données et
        améliorer la performance
        du système.
31. **Commande FSMO?**
    *   `netdom query fsmo`.

---

## Examen Connections

## Questions et Réponses (partie 3)

1.  **Def. Catalogue Global?**
    *   Base globale partielle de la BD de
        l'AD qui permet de rechercher
        rapidement les objets dans
        toute la forêt.
    *   RODC (Read-Only Domain Controller): c'est une
        copie en lecture seule du DC
        qui permet d'authentifier les
        utilisateurs localement mais
        ne permet pas de modifier les
        stratégies ni les données de l'AD.
2.  **Def DNS Résolvant et DNS Autoritaire?**
    *   **DNS Résolvant:**
        *   Un DNS résolvant recherche
            la réponse complète à une
            requête. En interrogeant
            d'autres serveurs si besoin.
    *   **DNS Autoritaire:**
        *   Celui qui possède les
            infos exactes d'un domaine.
            Il ne cherche rien ailleurs,
            il fournit directement la
            réponse officielle.

3.  **Comment contrôler l'espace disque sur plusieurs utilisateurs?**
    *   En configurant des
        quotas (limite d'espace
        disque) sur les dossiers
        partagés pour équilibrer
        l'usage du disque entre tous
        les utilisateurs.
4.  **Commande pour afficher les rôles FSMO?**
    *   Liste des rôles FSMO
        et indique quel CD les
        possède.
    *   `netdom query fsmo`.
5.  **Quels FSMO pour la gestion des identifiants uniques et de l'attribution d'approbation dans une forêt AD?**
    *   RID Master (pour les identifiants uniques).
    *   Infrastructure Master (pour les approbations entre domaines).

---


## Questions et Réponses (partie 4)

6.  **Partitions AD?**
    *   Partition Domaine
    *   Partition Schéma
    *   Partition DNS
    *   Partition Configuration.
7.  **NTDS Rôle, Quels outils permettent de gérer AD?**
    *   **NTDS:** Base de
        données centrale de l'AD qui
        stocke utilisateurs, ordinateurs,
        groupes et divers.
    *   **Outils:** "Utilisateurs et Ordinateurs AD", "Sites et Services AD".
8.  **DFS? Avantages?**
    *   Service qui permet
        de regrouper et organiser
        des dossiers partagés situés
        sur plusieurs serveurs dans
        un namespace unique, donnant
        l'impression qu'ils sont tous
        sur un seul serveur.
    *   **AV:**
        *   Redondance (éviter la
            perte de données).
        *   Centralisation.
        *   Simplifier la gestion.

9.  **Rôle du DFS Name Space et des dossiers DFS?**
    *   **DFS Name Space:**
        *   Fournit unifié l'accès aux
            partagés situés sur plusieurs
            serveurs en créant un chemin
            logique unique que les
            utilisateurs peuvent utiliser
            comme s'il était virtuel.
    *   **Les dossiers DFS:**
        *   Sont les dossiers
            partagés par les
            serveurs qui sont liés
            au Name Space.
10. **Stratégie de réplication?**
    *   **Unidirectionnelle:** un seul sens.
    *   **Bidirectionnelle:** deux sens.
    *   **Anneau de circulation:** entre plusieurs serveurs.

**U.B.A. (pour comprendre l'application des GPO)**
*   **U:** Unité d'organisation
*   **B:** Bloquer
*   **A:** Application
*   **L:** Local
*   **S:** Site
*   **D:** Domaine
*   **O:** OU

---

## Exercice

1.  **Requête WQL:**
    Retourne les ordinateurs
    Windows 10 ayant plus
    de 8 GO de mémoire RAM.

---


## Questions et Réponses (partie 5)

6.  **Rôle de la partition Schéma?**
    *   **Def:** Section de la BD de l'AD
        qui décrit la structure de
        tous les objets de l'annuaire.
    *   **Rep:** Elle définit et stocke
        les classes d'objets (utilisateur,
        groupe...) et leurs attributs (nom,
        Prénom, email) afin de créer un modèle standard
        pour tous les DC.
7.  **Un employé quitte l'entreprise et non remplaçant doit accéder à des fichiers cryptés, que faire?**
    *   **Rep:** On doit récupérer les
        clés EFS pour qu'on puisse
        accéder aux fichiers cryptés.
    *   **EFS (Encrypting File System):**
        Fonction qui chiffre les fichiers
        afin que seul l'utilisateur puisse
        y accéder.
8.  **Protocole?**
    *   En créant une OU distincte
        pour chaque département
        pour le domaine
        `digitalsoft.local` et en
        attribuant des OU pour
        les GPO spécifiques de
        chaque département afin
        de séparer les responsabilités
        et de pouvoir appliquer
        des GPO adaptées à chaque
        groupe ou au service.
9.  **GPO?**
    *   **GPO1**
    *   **GPO2**
    *   **GPO3**
    *   Il faut créer
        un GPO spécifique
        (coffre) qui sera
        un starter GPO de façon
        simple. GPO s'applique
        à ces utilisateurs spécifiques
        sans impacter les autres
        départements.
10. **Le serveur est ici la valeur entrée.**
    *   Pour un fond d'écran
        GPO, on doit utiliser
        un fond d'écran accessible
        aux clients, jamais un
        chemin local C:\.
