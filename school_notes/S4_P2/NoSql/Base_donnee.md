### I. Évolution des Systèmes de Gestion de Données

#### 1. Les premiers systèmes de gestion des données (Avant les années 1970)
##### Systèmes de gestion de données à fichiers plats :
*   Les données étaient stockées de manière persistante sur <mark style="background: #FFB86CA6;">disques ou bandes magnétiques </mark>(un support en plastique magnétisé populaire des années 1950 à 1970).
*   *Organisation physique :* La bande magnétique est divisée en blocs de données séparés par des espaces (écarts). <mark style="background: #FFB86CA6;">L'accès est  séquentiel</mark> (lecture d'un bloc puis des suivants en séquence), s'adaptant directement aux contraintes physiques du support.
*   *Allocation de l'espace :* Les développeurs attribuaient des tailles de <mark style="background: #FFB86CA6;">stockage fixes</mark> pour chaque entité (par exemple, pour un client : ID de 10 caractères, Nom de 40 caractères, Adresse de 100 caractères, Téléphone de 10 caractères, soit un enregistrement fixe de 160 caractères). 
*   *Accès aléatoire :* Pour chercher des informations non séquentielles sur bande, le déplacement de la tête de lecture d'un bout à l'autre est très lent. <mark style="background: #FFB86CA6;">L'accès aléatoire est plus efficace</mark> avec l'apparition des lecteurs de disques, car la tête n'a besoin de se déplacer que sur le rayon du disque au maximum.
- **Limitations :**
	* <mark style="background: #FFF3A3A6;">La structure logique des données est souvent directement codée au sein même des programmes applicatifs</mark> . Donc, <mark style="background: #FFB86CA6;">les modifications de la structure des fichiers imposent de modifier le code </mark> des programmes. 
	* ces systèmes génèrent de <mark style="background: #FFB86CA6;">la duplication </mark>de données et rendent complexe la <mark style="background: #FFB86CA6;">gestion de la confidentialité et des droits d'accès.</mark>
##### Systèmes de gestion de données hiérarchiques :
*   Conçus pour pallier <mark style="background: #FFB86CA6;">l'inefficacité des recherches </mark>dans les fichiers plats.
*   Les données sont organisées selon une hiérarchie de relations <mark style="background: #FFB86CA6;">parent-enfant</mark>, commençant par un nœud racine qui relie la couche supérieure des enregistrements, lesquels peuvent à leur tour pointer vers des enregistrements enfants contenant des détails supplémentaires.
*   **Limitations :** Si un element est partage par deux clients, le système doit le <mark style="background: #FFB86CA6;">dupliquer</mark> pour le lie a les deux, ce qui crée des risques d'erreurs (<mark style="background: #FFB86CA6;">incohérences</mark>) lors des mises à jour.

##### Systèmes de gestion de données réseau :
*   Proches du modèle hiérarchique, mais lèvent la limite du parent unique (<mark style="background: #FFB86CA6;">un enfant peut avoir plusieurs parents</mark>).
*   Ils intègrent deux composants essentiels : un <mark style="background: #ABF7F7A6;">schéma explicite et la base de données elle-même.</mark>
*   **Contraintes :** Les <mark style="background: #BBFABBA6;">relations sont orientées et unidirectionnelles</mark>, permettant de représenter des relations complexes de type $1\text{-}n$ ou $n\text{-}n$.
*   **Limitations :**
	* Modèles <mark style="background: #ABF7F7A6;">complexes à concevoir</mark> et à administrer.
	* Pour accéder à une donnée, le programme doit parcourir <mark style="background: #ABF7F7A6;">un grand nombre de liens</mark> (complexité).
	* Toute modification du schéma nécessite de réécrire <mark style="background: #ABF7F7A6;">les applications d'accès</mark>.

---
#### 2. La révolution des bases de données relationnelles (BDR)
##### Caractéristiques clés :
*   <mark style="background: #FFB86CA6;">Séparation stricte</mark> entre <mark style="background: #FFB86CA6;">l'organisation logique</mark> des structures de données(les tables,coloneset lignes) et leur <mark style="background: #FFB86CA6;">stockage physique</mark>.
*   <mark style="background: #FFF3A3A6;">Normalisation</mark> de la conception par des <mark style="background: #FFB86CA6;">règles strictes éliminant les anomalies de données.</mark>  
*   Conception pensée pour supporter <mark style="background: #FFB86CA6;">des centaines à des milliers d'utilisateurs simultanés.</mark>
*   <mark style="background: #FFB86CA6;">Regroupement des opérations en une seule transaction cohérente</mark> (par exemple, un transfert bancaire de 100 DH nécessite de débiter un compte d'épargne et de créditer un compte courant ; <mark style="background: #FFF3A3A6;">le système garantit que l'état intermédiaire n'est jamais visible pour les autres utilisateurs</mark>).

---
#### 3. Fonctionnement d'un SGBDR (Système de Gestion de Bases de Données Relationnelles)
Un SGBDR est une application qui permet d'effectuer <mark style="background: #FFB8EBA6;">les opérations CRUD</mark> (Create, Read, Update, Delete) à l'aide d'un langage standardisé : le **SQL**. La plupart des utilisateurs finaux interagissent avec des applications tierces qui communiquent de façon transparente avec le SGBDR.

Un SGBDR opérationnel repose sur quatre composants essentiels :
<mark style="background: #BBFABBA6;">gestion de stockage et de memoire, dictionnaire des donnees et language de requete</mark>
1.  **Programmes de gestion du stockage :**
    *   Gèrent <mark style="background: #FFB86CA6;">l'écriture et la lecture persistante des blocs de données</mark> sur disques ou clés USB.
    *   Facilitent la création et l'utilisation d'<mark style="background: #BBFABBA6;">index de données</mark>, qui contiennent les informations de localisation des blocs sur le disque pour un accès rapide (par exemple, associer le nom d'un client à sa position physique exacte sur le disque).
    *   <mark style="background: #ABF7F7A6;">Optimisation</mark>:
	    * <mark style="background: #ABF7F7A6;">du placement physique des données</mark>
	    * <mark style="background: #ABF7F7A6;">compression des informations </mark>
	    * <mark style="background: #ABF7F7A6;">gèrention des copies de secours </mark>(sauvegarde) en cas de défaillance matérielle.
2.  **Programmes de gestion de la mémoire :**
    *   La taille des données étant souvent bien supérieure à la mémoire vive (RAM) disponible, ces composants sont chargés de <mark style="background: #FFB86CA6;">la gestion du montage, conservation et puis liberation des donnees dans la RAM.</mark>
    *   La lecture en mémoire étant beaucoup plus rapide que sur disque, l'efficacité de ce gestionnaire dicte les performances globales du système.
3.  **Dictionnaire de données :**
    *   Conserve les <mark style="background: #FFB86CA6;">métadonnées sur la structure physique et logique de la base </mark>: schémas, tables, colonnes, index, contraintes d'intégrité et vues.
4.  **Langage de requête :**
    *   Se compose du **LDD** (Langage de Définition de Données) pour <mark style="background: #ADCCFFA6;">créer/supprimer</mark> des schémas, tables, vues, index ou contraintes, et <mark style="background: #ADCCFFA6;">accorder les droits</mark> de lecture/écriture.
    *   Se compose du **LMD** (Langage de Manipulation de Données) pour <mark style="background: #ADCCFFA6;">insérer, mettre à jour, supprimer et sélectionner</mark> (lire) les données.

---

### III. Les Limites des Systèmes Relationnels Traditionnels

Avec l'avènement du Web, des géants de l'Internet (Google, Meta, X, Amazon) ont été confrontés à une échelle d'utilisateurs inédite par rapport aux usages professionnels classiques.

*   **Nouveaux besoins du Web :**
    *   Prise en charge de <mark style="background: #FFB86CA6;">volumes massifs d'opérations de lecture et d'écriture simultanées.</mark>
    *   Temps de réponse à très faible latence.
    *   Haute disponibilité globale (24h/24, 7j/7).
*   **Limites de la mise à l'échelle verticale (Scaling) :**
    *   Historiquement, Si la base de données est trop lente, on augmente ses capacités physiques internes(<mark style="background: #FFF3A3A6;">ajout de: CPU, RAM, ...</mark>). Cette approche est <mark style="background: #FFB86CA6;">coûteuse et atteint rapidement des limites physiques</mark>.
*   **Coût de la répartition et des jointures :**
    *   Les SGBDR offrent des fonctionnalités de jointure complexes et d'intégrité référentielle pour lier les entités entre elles.
    *   Pour que ces jointures soient performantes, <mark style="background: #FFF3A3A6;">les données liées doivent être stockées physiquement sur le même nœud serveur</mark>. 
    *   Dès lors, <mark style="background: #FFB86CA6;">distribuer et partitionner ces données sur plusieurs nœuds serveurs devient extrêmement complexe et coûteux en performances.</mark>
*   **Les contraintes du modèle ACID(Atomicité, Cohérence, Isolation, Durabilité) :**
	* c'est une question <mark style="background: #ABF7F7A6;">de coût de performance et d'architecture.</mark>
    *   Les SGBD relationnels sont fondamentalement transactionnels et respectent scrupuleusement les contraintes ACID :
        *   **Atomicité :** Tout ou rien (la transaction réussit entièrement ou échoue complètement).
        *   **Cohérence :** Les transactions doivent respecter toutes les règles d'intégrité de la base.
        *   **Isolation :** Une transaction ne peut pas voir les données en cours de modification par une autre transaction non validée.
        *   **Durabilité :** Une fois validée, la transaction est enregistrée de façon permanente (via les logs de transaction) et résiste aux pannes.
    *   À l'échelle d'un système distribué mondial, maintenir ces propriétés ACID strictes tout en conservant des performances acceptables devient impossible.
##### Conclusion:
- Nécessité de <mark style="background: #ABF7F7A6;">distribuer les traitements de données</mark> entre différents serveurs.
- <mark style="background: #ABF7F7A6;">Difficile de maintenir les contraintes ACID</mark> à l’échelle du système distribué entier tout en maintenant des performances correctes
---

### IV. La Transition vers les Systèmes Distribués et le NoSQL

#### 1. Vers des bases de données distribuées
La plupart des SGBD NoSQL se libère des contraintes ACID, ou même ne proposent pas de
gestion de transactions

Système distribué = <mark style="background: #FFB86CA6;">Système qui permet de coordonner plusieurs machines via des
messages envoyés par réseau</mark>

<mark style="background: #FF5582A6;">--></mark> Deux stratégies de mise à l'échelle (scaling) se présentent :
1.  **Distribution de traitement (Scaling de traitement) :** <mark style="background: #FFB86CA6;">Répartir la charge de calcul</mark> sur plusieurs machines pour soulager les serveurs.
2.  **Distribution des données (Scaling des données) :** <mark style="background: #FFB86CA6;">Répartir le stockage physique</mark> des données sur plusieurs machines, puis diriger chaque requête vers la machine appropriée. 
--> <mark style="background: #FFF3A3A6;">partionnement horizontal et vertical</mark>

*   *Structure physique typique :* Les serveurs d'un centre de données sont regroupés dans des châssis physiques (**racks**) connectés à haute vitesse ($\ge 1$ Go/s).

#### 2. Les objectifs fondamentaux d'un SGBD Distribué
Un SGBD distribué doit accomplir trois fonctions primordiales :
1.  <mark style="background: #FFB8EBA6;">Stocker</mark> les données de manière persistante.
2.  Maintenir la <mark style="background: #FFB8EBA6;">cohérence</mark> des données (toutes les copies doivent être identiques).
3.  Garantir la <mark style="background: #FFB8EBA6;">disponibilité</mark> des données (les utilisateurs doivent pouvoir accéder au système à tout moment).

#### 3. Équilibrer cohérence et temps de réponse
*   <mark style="background: #FFB8EBA6;">Le temps réseau</mark> pour synchroniser les nœuds est nécessaire pour garantir la coherence et la disponibilite des applications critiques telles que les systèmes financiers d'une banque.
- <span style="color:rgb(179, 104, 253)">Les BDs NoSQL</span> mettent souvent en œuvre une <mark style="background: #FFB8EBA6;">cohérence finale</mark> ; c'est-à-dire qu'il peut y avoir une période pendant laquelle les copies de données ont <mark style="background: #FFB8EBA6;">des valeurs différentes</mark> (si elle viennent de différents serveurs d'un cluster) , mais finalement toutes les copies auront la même valeur.
- Les BDs NoSQL utilisent souvent le concept de <mark style="background: #FFB8EBA6;">quorum</mark> dans les lectures et les écritures.
-  Un <mark style="background: #FFB8EBA6;">quorum</mark> correspond au nombre de serveurs qui doivent répondre à une opération de lecture ou d'écriture pour que l'opération soit considérée comme terminée (si trois serveur sont coherents et seulement un ne l'est pas, l'information est cnsidere correcte). 

---

### V. Cadres Théoriques : Théorème de CAP et Modèle BASE

L'émmergence du mouveent NoSQL s'appuie sur <mark style="background: #FFB8EBA6;">des règles et modèles théoriques décrivant les limites des architectures distribuées.</mark>

#### 1. Le Théorème de CAP (Brewer, 2000)
On ne peut obtenir que 2 des 3 propriétés suivantes dans un systeme reparti :
1.  **Cohérence (Consistency) :** Tous les noeuds du système voient exactement les mêmes données au même moment.
2.  **Disponibilité (Availability) :** Chaque client a la meme vue de chaque donnee a tout instant. 
3.  **Résistance au partitionnement (Partition tolerance) :** Le système continue de fonctionner correctement même dans le cas d’une coupure totale du réseau (aucune communication reseau)

*   **Classification des systèmes :**
	- Les <mark style="background: #FFF3A3A6;">SGBDRs </mark>assurent les propriétés de Cohérence et de Disponibilité => Systèmes <mark style="background: #FFF3A3A6;">AC</mark>
	- Les<mark style="background: #FFF3A3A6;"> SGBD NoSQL</mark> sont des systèmes <mark style="background: #FFF3A3A6;">CP ou AP</mark>

*   **Exemples d'application du Théorème de CAP :**
    *   *Cohérence :*
        *   Exemple 1 (Une seule instance de base de données) : Le système est naturellement cohérent puisqu'un seul nœud maintient l'état.
        *   Exemple 2 (Deux serveurs partitionnés par clés) : Le serveur 1 gère les clés de A à M, le serveur 2 gère les clés de N à Z. La cohérence est simple à garantir car il n'y a pas de recouvrement.
        *   Exemple 3 (Bases répliquées) : Pour garantir une cohérence parfaite, toute insertion sur une base doit être répliquée sur la seconde base de façon synchrone avant de valider l'opération pour l'utilisateur.
    *   *Disponibilité :*
        *   Exemple 1 (Un seul nœud) : Si le nœud tombe en panne, on perd 100% des données et de la disponibilité.
        *   Exemple 2 (Deux serveurs partitionnés sans réplication) : Si un nœud tombe en panne, on perd 50% de la disponibilité des données.
        *   Exemple 3 (Réplication active) : Cloner la base sur plusieurs serveurs assure une haute disponibilité. Multiplier les répliques protège le système des pannes physiques et permet de répartir efficacement la charge des lectures.
    *   *Résistance au partitionnement (Cas d'une coupure réseau entre 2 serveurs répliqués) :*
        *   Si l'application accepte les opérations de lecture et d'écriture de chaque côté de la coupure, les deux serveurs vont diverger et perdre leur cohérence. C'est le cas typique d'un système **AP** (ex : applications de réseaux sociaux).
        *   Si l'application exige une cohérence stricte (comme une application bancaire où un retrait de 1 000 Dh effectué à Rabat doit être immédiatement connu à Casablanca), elle doit bloquer les écritures sur le nœud isolé pendant la panne réseau. La cohérence est préservée, mais la disponibilité est perdue (les comptes sont gelés). C'est le cas d'un système **CP**.

#### 2. Le Modèle BASE (Alternative à ACID)
*   **BA (Basically Available) :** Le système privilégie la disponibilité. Une <mark style="background: #FFB8EBA6;">défaillance partielle</mark> d'une partie du cluster <mark style="background: #FFB8EBA6;">n'empêche pas le reste du système de fonctionner</mark>.
*   **S (Soft state) :** L'état du système peut évoluer sans intervention de l'utilisateur, au fur et à mesure que les mises à jour se propagent à travers les nœuds du cluster. <mark style="background: #FFB8EBA6;">Les données peuvent temporairement être écrasées par des informations plus récentes.</mark>
*   **E (Eventually consistent - Cohérence finale) :** La base de données peut se trouver temporairement dans un <mark style="background: #FFB8EBA6;">état incohérent</mark> (les répliques n'ont pas encore les mêmes valeurs), mais le système garantit qu'à terme, toutes les copies finiront d'etre dansl .

#### 3. Variantes de la cohérence finale
*   **Cohérence occasionnelle :** Garantit que la base de données <mark style="background: #FFF3A3A6;">reflète l'ordre chronologique exact </mark>dans lequel les opérations ont été soumises. (Exemple : si Amina modifie un solde à 1000 DH, puis qu'Ahmed le modifie à 2000 DH une minute après, toutes les répliques appliqueront d'abord le passage à 1000 DH avant d'appliquer la mise à jour à 2000 DH).
*   **Cohérence de Lisez-Vos-Écrits (Read-Your-Writes consistency) :** Garantit à l'utilisateur ayant effectué une mise à jour que <mark style="background: #FFF3A3A6;">ses lectures ultérieures renverront immédiatement sa valeur mise à jou</mark>r, même si la réplication globale sur les autres nœuds n'est pas encore finalisée. (Exemple : si Amina met à jour un solde à 1500 DH sur un serveur, toute requête ultérieure de sa part est dirigée de sorte qu'elle lise la valeur de 1500 DH).

### Caractéristiques générales des BD NoSQL
- Adoptent une <mark style="background: #BBFABBA6;">représentation non relationnelle</mark> des données.
-  Ne remplacent pas les BDR, mais sont une alternative/complément apportant <mark style="background: #BBFABBA6;">des solutions par rapports aux limites des BDR</mark>
-  Apportent une plus <mark style="background: #BBFABBA6;">grande performance</mark> dans le contexte des apps web avec des <mark style="background: #BBFABBA6;">volumétries de données </mark>exponentielle.
- Utilisent une très forte <mark style="background: #BBFABBA6;">distribution</mark> des données et des traitements associés sur de <mark style="background: #BBFABBA6;">nombreux serveurs</mark>.
- <mark style="background: #BBFABBA6;">Pas de schéma</mark> pour les données.
- Données distribuées: partitionnement horizontale des données sur plusieurs noeuds.
- Réplication des données sur plusieurs noeuds.
- Privilégient la disponibilité à la cohérence: <mark style="background: #BBFABBA6;">AP plutot que CP</mark> (tendance dominante)
---

### VI. Mécanismes Techniques et Distribution dans le NoSQL

#### 2. Prise en charge de l'extensibilité(scalability)
<mark style="background: #FFB8EBA6;">l'extensibilité:</mark> la capacité d'un système à **faire face à une augmentation de la charge** (volume de données ou nombre d'utilisateurs) en conservant des performances correcte

*  **Par réplication (duplication des données):**
    *   *Mode Maître-Esclave (Master-Slave) :* Le nœud <mark style="background: #FFF3A3A6;">maître</mark> reçoit toutes les requêtes d'<mark style="background: #FFF3A3A6;">écriture </mark>et propage de manière asynchrone les modifications vers les nœuds esclaves. Les <mark style="background: #FFB86CA6;">esclaves</mark> prennent en charge les requêtes de <mark style="background: #FFB86CA6;">lecture</mark>. <mark style="background: #BBFABBA6;">Ce modèle est limité par la capacité maximale d'écriture du nœud maître</mark>.
    *   *Mode Maître-Maître (Multi-Master) :* <mark style="background: #FFF3A3A6;">Tous les nœuds peuvent traiter à la fois des requêtes en lecture et en écriture</mark>. Ce modèle pose <mark style="background: #BBFABBA6;">des défis complexes de résolution de conflits et de gestion de la cohérence.</mark>
*   **Par Sharding / Partitionnement (division des données) :** 
    *   *Partitionnement Vertical :* Consiste à séparer et <mark style="background: #BBFABBA6;">isoler fonctionnellement les tables ou les concepts</mark> métiers sur des machines dédiées.
    *   *Partitionnement Horizontal :* Consiste à <mark style="background: #BBFABBA6;">distribuer l'ensemble des enregistrements d'une même table</mark> ou collection sur plusieurs machines pour répartir le volume.
    *   *Objectif :* Veiller à ce que les <mark style="background: #FFB8EBA6;">données</mark> fréquemment accédées ensemble résident sur <mark style="background: #FFB8EBA6;">le même nœud</mark>, et s'assurer que la <mark style="background: #FFB8EBA6;">charge</mark> globale est répartie <mark style="background: #FFB8EBA6;">uniformément</mark>. Une distribution simple peut générer un déséquilibre, d'où l'utilisation de techniques avancées.

#### 3. Le Consistent Hashing (Hachage Cohérent)
Le *Consistent Hashing* est un mécanisme de <mark style="background: #FFB8EBA6;">partitionnement horizontal</mark> ou l'objectif est de positionner les données et les serveurs sur un anneau virtuel <mark style="background: #FFB86CA6;">pour minimiser les mouvements lors de changements</mark>.
![[Pasted image 20260601202816.png]]

*   Une même fonction de hachage $h(x)$ est appliquée pour positionner les serveurs et les clés de données sur l'anneau.
*   <mark style="background: #FFB86CA6;">Chaque clé de donnée est stockée sur le premier nœud disponible</mark> rencontré en parcourant l'anneau dans le **sens horaire**.
*   **Gestion de la dynamique du cluster :**
    *   *Arrivée d'un nœud :* Si un nœud D s'insère sur l'anneau entre les objets, seules les données se trouvant immédiatement avant lui sur l'anneau (dans le sens anti-horaire) lui sont réassociées, minimisant ainsi les mouvements de données dans le réseau.

#### 4. Protocoles de communication et tolérance aux pannes
*   **Protocole Gossip (Protocole de rumeur) :**
    * Le protocol Gossip implique des <mark style="background: #FFB86CA6;">interactions périodiques par pair de noeuds poour detection de pannes</mark>
    *   <mark style="background: #FFF3A3A6;">De manière périodique, chaque nœud choisit un autre nœud au hasard et lui envoie un message contenant son propre état</mark>. Le destinataire renvoie un accusé de réception (ACK) ainsi que des informations sur les nœuds qu'il n'arrive pas lui-même à joindre.
    *   Un mécanisme de <mark style="background: #FFF3A3A6;">quorum</mark> permet d'évaluer collectivement ces rumeurs avant de déclarer officiellement un nœud indisponible.
*   **Transfert suggéré (Hinted Handoff) :**
	* <mark style="background: #FFB8EBA6;">Objectif:</mark> pour garantir que le système reste **disponible en écriture**, même lorsqu'un serveur est momentanément indisponible.
	
    *  <mark style="background: #FFB8EBA6;">Explication:</mark> Lors d'une opération d'écriture, si un <mark style="background: #FFF3A3A6;">nœud cible</mark> est détecté comme <mark style="background: #FFF3A3A6;">indisponible</mark>, <mark style="background: #FFB86CA6;">le système écrit temporairement la donnée sur un autre nœud opérationnel</mark> (<mark style="background: #FFF3A3A6;">nœud de coordination</mark>) en y associant une note indiquant que cette écriture devra être rejouée sur le nœud défaillant dès son retour.
    *   Si toutes les répliques sont hors ligne, le nœud de coordination prend l'écriture en charge. Dès que le nœud d'origine redevient actif, le nœud temporaire lui renvoie les données accumulées.
    *  <mark style="background: #FFB8EBA6;">Impact :</mark><mark style="background: #FFF3A3A6;"> Améliore considérablement la disponibilité en écriture et la tolérance aux partition</mark>s, mais augmente temporairement le risque d'incohérence des lectures.

#### 5. Évaluation mathématique du niveau de cohérence (Le Quorum)
Soient :
*   <span style="color:rgb(180, 137, 8)">N :</span> Le nombre de nœuds contenant une copie (réplique) des données.
*   <span style="color:rgb(180, 137, 8)">W : </span>Le quorum d'écriture (nombre minimal de répliques devant accuser réception d'une mise à jour pour qu'elle soit considérée comme réussie).
*  <span style="color:rgb(180, 137, 8)"> R :</span> Le quorum de lecture (nombre de répliques consultées lors d'une opération de lecture).

*   **Règle de cohérence forte :**
    Si **$W + R > N$**, alors le système garantit une **cohérence forte**. Il y a mathématiquement toujours une intersection entre l'ensemble des nœuds sur lesquels on écrit et l'ensemble des nœuds sur lesquels on lit (au moins un nœud consulté lors de la lecture détient la version la plus récente).
*   **Règle de cohérence finale :**
    Si **$W + R \le N$**, le système n'offre qu'une **cohérence finale/éventuelle**.

*   **Scénarios de configuration du Quorum :**
    *   *Scénario $R = N$ et $W = N$ (Cas extrême) :* La cohérence est <mark style="background: #FFF3A3A6;">forte</mark>. Le système peut fournir des garanties transactionnelles <mark style="background: #FFF3A3A6;">ACID strictes</mark>, mais la moindre panne d'un seul nœud rend le système <mark style="background: #FFF3A3A6;">indisponible</mark> en écriture et en lecture.
    *   *Scénario $R = 1$ et $W = N$ :* La cohérence est forte. Configuration idéale pour les systèmes ayant un <mark style="background: #FFF3A3A6;">très fort volume de lecture et peu d'écritures</mark>. La lecture est ultra-rapide (un seul nœud interrogé), mais l'écriture nécessite l'accord de tous les nœuds. Si un nœud tombe en panne, le système ne peut plus écrire.
    *   *Scénario $R = N$ et $W = 1$ :* Cohérence forte, mais avec de gros risques d'incohérence si des nœuds sont en retard de réplication (bien que la lecture de tous les nœuds $R=N$ permette de retrouver la dernière version). L'écriture est ultra-rapide (un seul ACK suffit), mais si un nœud tombe en panne, <mark style="background: #FFF3A3A6;">la lecture complète échoue.</mark>
    *   *Scénario $R = W = (N + 1) / 2$ (Quorum majoritaire) :* Fournit un compromis <mark style="background: #FFF3A3A6;">équilibré</mark> garantissant une cohérence finale robuste et une bonne tolérance aux pannes.

#### 6. Gestion de la concurrence et de la cohérence interne
*   **Le Versioning (Gestion de versions) :**
    *   Méthode pour gérer <mark style="background: #FFB8EBA6;">les accès simultanés avec mise à jour</mark>.
    *   Plutôt que d'écraser physiquement la donnée d'origine, le système NoSQL marque l'ancienne donnée comme "obsolète" et ajoute une <mark style="background: #FFB8EBA6;">nouvelle version</mark> contenant la donnée <mark style="background: #FFB8EBA6;">modifiée</mark>. Plusieurs versions <mark style="background: #FFB8EBA6;">coexistent</mark> en base, la plus récente étant <mark style="background: #FFB8EBA6;">prioritaire</mark>.
    *   Ce mécanisme nécessite un processus de balayage périodique (<mark style="background: #BBFABBA6;">garbage collection</mark>) pour purger physiquement les données obsolètes.
*   **Les Horloges Vectorielles (Vector Clocks) :**
    - **Événement local :**  
    Avant de réaliser une action locale (enregistrer une donnée, par exemple), un serveur incrémente de 1 sa propre valeur dans son vecteur.
    - Exemple sur A= : [0, 0, 0] devient [1, 0, 0].
        
- **Envoi d'un message :**  
    Lorsqu'un serveur envoie un message à un autre, il incrémente d'abord sa propre valeur (règle 1), puis il joint une copie de son vecteur au message.
    
- **Réception d'un message :**  
    Lorsqu'un serveur reçoit un message contenant le vecteur de l'expéditeur :
    - Il compare son propre vecteur avec le vecteur reçu.
    - Pour chaque position du vecteur, il conserve la valeur **la plus grande** (le maximum).
    - Il incrémente ensuite de 1 sa propre valeur dans son vecteur pour enregistrer l'événement de réception.

#### 7. Le modèle MapReduce
*   Modèle de programmation pour <mark style="background: #FFB8EBA6;">le traitement parallèle massif de données distribuées</mark> au sein d'un cluster, popularisé par Google en 2004.
*   *Fonctionnement dans les bases de données :* Le système applique localement la fonction **Map** sur chaque nœud de stockage détenant une portion des données à traiter, puis regroupe et synthétise les résultats intermédiaires via la fonction **Reduce**.
*   Ce modèle est conceptuellement proche des opérations de distribution/rassemblement (*Scatter* et *Gather*) de la bibliothèque de programmation parallèle MPI.

---

### VII. Taxonomie et Caractéristiques des Bases de Données NoSQL

#### 1. Caractéristiques générales des bases de données NoSQL
*   Adoptent des représentations de données <mark style="background: #FFB8EBA6;">non relationnelles</mark>.
*   Ne remplacent pas les BDR mais s'imposent comme une <mark style="background: #FFF3A3A6;">alternative</mark> ou un complément performant dans des contextes spécifiques (<mark style="background: #FFB8EBA6;">Web à volumétrie exponentielle</mark>).
*   Reposent sur une <mark style="background: #FFB8EBA6;">distribution</mark> massive des données et des calculs.
*   Sont **schemaless** (absence de schéma fixe prédéfini pour les données).
*   Mettent en œuvre le partitionnement horizontal (sharding) et la réplication sur de nombreux serveurs.
*   <mark style="background: #FFB8EBA6;">Privilégient la disponibilité à la cohérence stricte (modèles AP plutôt que CP).</mark>

---

#### 2. Les quatre grandes familles de bases de données NoSQL

Voici la taxonomie des systèmes NoSQL, classée par type :
##### 1. Bases de données Clé / Valeur
*   **Concepts clés :** Assimilé à une <mark style="background: #ABF7F7A6;">table de hachage distribuée</mark>. Chaque objet est identifié par une <span style="color:rgb(180, 137, 8)">clé unique</span> (seule méthode pour requêter). En raison de l'<mark style="background: #ABF7F7A6;">absence de structure complexe</mark>, la logique de traitement est portée par <mark style="background: #ABF7F7A6;">l’application qui interroge la BD</mark>.
*   **Exemples :** <mark style="background: #FFB86CA6;">DynamoDB, Voldemort, Redis.</mark>
*   **Cas d'usage :** Cache et gestion de sessions (<mark style="background: #FFB8EBA6;">où l'intégrité relationnelle n'est pas requise</mark>), profils/préférences utilisateurs, paniers d'achat, flux IoT, logs.
*   **Avantages :** 
    *   Grande <mark style="background: #FFF3A3A6;">simplicité</mark> du modèle.
    *   Performances très élevées en <mark style="background: #FFF3A3A6;">lecture et en écriture</mark>.
    *   Excellente mise à l'échelle horizontale (<mark style="background: #FFF3A3A6;">évolution, disponibilité</mark>).
*   **Inconvénients :** Trop rudimentaire pour les données complexes ; requêtes limitées à la clé d'identification ; surcharge de la couche applicative.

---

##### 2. Bases de données Orientées Colonnes
*   **Concepts clés :** Stockage par colonne pour <mark style="background: #ABF7F7A6;">le traitement massif de données et l'analyse</mark>. Permet d'ajouter des colonnes de <mark style="background: #ABF7F7A6;">manière dynamique</mark> (évite les valeurs *NULL*).
    *   *Structure :* Colonne (clé, valeur, timestamp) $\rightarrow$ Super-colonne (liste de colonnes) $\rightarrow$ Famille de colonnes (équivalent table).
    *   *Comparaison :* Clé primaire $\rightarrow$ <mark style="background: #FFF3A3A6;">Row Key</mark> ; Nom de colonne $\rightarrow$ <mark style="background: #FFF3A3A6;">Idem</mark>.
*   **Exemples :** <mark style="background: #FFB86CA6;">HBase, Cassandra.</mark>
*   **Cas d'usage :** <mark style="background: #BBFABBA6;"> Le traitement analytique massif (BI) et MapReduce moteurs de recherche, gestion de flux rapides </mark>
*   **Avantages :**
    *   Support natif des <mark style="background: #FFF3A3A6;">données semi-structurées</mark>.
    *   Colonnes naturellement <mark style="background: #FFF3A3A6;">indexées</mark> pour des <mark style="background: #FFF3A3A6;">recherches ciblées</mark>.
    *   <mark style="background: #FFF3A3A6;">Mise à l'échelle horizontale robuste</mark>.
*   **Inconvénients :**
    *   Bases les plus <mark style="background: #FFF3A3A6;">complexes à appréhender.</mark>
    *   Modèle <mark style="background: #FFB86CA6;">inadapté</mark> pour les données fortement <mark style="background: #FFF3A3A6;">interconnectées</mark>.
    *   Nécessite une <mark style="background: #FFB86CA6;">maintenance</mark> rigoureuse lors de l'<mark style="background: #FFF3A3A6;">ajout</mark>, de la <mark style="background: #FFF3A3A6;">suppression</mark> ou du <mark style="background: #FFF3A3A6;">regroupement</mark> de colonnes.

---

##### 3. Bases de données Orientées Documents
*   **Concepts clés :** Collections de documents autonomes, semi-structurés, au <mark style="background: #ABF7F7A6;">format hiérarchique de type JSON ou XML</mark>. Système "<mark style="background: #ABF7F7A6;">sans schéma</mark>" (*schemaless*) permettant des documents <mark style="background: #ABF7F7A6;">hétérogènes</mark>. Permet d'interroger directement le contenu et les attributs internes.
    *   *Intérêt du JSON :* Standard d'échange, directement <mark style="background: #FFF3A3A6;">manipulable</mark> comme un <mark style="background: #FFF3A3A6;">objet</mark> en mémoire, bonne <mark style="background: #FFF3A3A6;">lisibilité humaine</mark> et <mark style="background: #FFF3A3A6;">moins volumineux</mark> que l'XML.
*   **Exemples :** <mark style="background: #FFB86CA6;"> MongoDB, CouchDB, RavenDB.</mark>
*   **Cas d'usage :** Systèmes de <mark style="background: #FFB8EBA6;">Gestion de Contenu</mark> (CMS), <mark style="background: #FFB8EBA6;">historisation</mark> d'événements (logs), <mark style="background: #FFB8EBA6;">catalogues</mark> e-commerce.
*   **Avantages :**
    *   Modèle <mark style="background: #FFF3A3A6;">puissant et expressif </mark> (structures imbriquées).
    *   Forte <mark style="background: #FFF3A3A6;">flexibilité</mark> sans maintenance de schéma.
*   **Inconvénients :**
    *   Modèle <mark style="background: #FFB86CA6;">inadapté</mark> pour les données fortement <mark style="background: #FFF3A3A6;">interconnectées</mark>.
    *   <mark style="background: #FFB86CA6;">Requêtes de recherche</mark> principalement <mark style="background: #FFF3A3A6;">optimisées</mark> sur les <mark style="background: #FFF3A3A6;">clés</mark> et index déclarés.
    *   Peut devenir <mark style="background: #FFB86CA6;">lent</mark> pour les agrégations de <mark style="background: #FFF3A3A6;">données  volumineuses</mark>.

---

##### 4. Bases de données Orientées Graphes
*   **Concepts clés :** Reposent sur <mark style="background: #ABF7F7A6;">la théorie des graphes</mark> pour modéliser des <mark style="background: #ABF7F7A6;">données complexes dont les relations sont plus importantes que les données elles-mêmes</mark>. 
    *   *Éléments :* <span style="color:rgb(180, 137, 8)">Les nœuds</span> (entités), <span style="color:rgb(180, 137, 8)">les relations</span> (arcs orientés) et <span style="color:rgb(180, 137, 8)">les propriétés rattachées</span> (clés/valeurs sur nœuds ou arcs).
    *   *Technologie :* Associe au stockage <mark style="background: #FFB8EBA6;">un moteur d'indexation physique ultra-performant dédié au parcours des arcs</mark>. Idéal pour les <mark style="background: #ABF7F7A6;">réseaux</mark> ou les <mark style="background: #ABF7F7A6;">systèmes de recommandation</mark>.
*   **Exemples :** <mark style="background: #FFB86CA6;">Neo4j (langage Cypher), OrientDB, SPARQL.</mark>
*   **Cas d'usage :** Moteurs de <mark style="background: #ABF7F7A6;">recommandation</mark>, <mark style="background: #ABF7F7A6;">réseaux sociaux</mark>, calculs d'itinéraires, <mark style="background: #ABF7F7A6;">services financiers</mark> (détection de fraudes).
*   **Avantages :**
    *   Modèle d'une grande <mark style="background: #FFF3A3A6;">puissance expressive</mark>.
    *   Performances de parcours de liaisons plus rapides que les jointures SQL.
*   **Inconvénients :**
    *   Le partitionnement horizontal (ou <mark style="background: #FFF3A3A6;">sharding complexe</mark>) sur plusieurs serveurs est difficile en raison de <mark style="background: #FFF3A3A6;">l'interdépendance des nœuds</mark>.
---

### VIII. Synthèse comparative globale : Avantages et Inconvénients du NoSQL

#### 1. Avantages généraux des bases de données NoSQL
*   **Performances stables :** Les temps de réponse restent proportionnels au volume de données traité et ne s'effondrent pas avec la croissance de la base.
*   **Facilité de migration et agilité :** Contrairement aux SGBDR, il n'est pas nécessaire d'interrompre le service ou d'effectuer des opérations lourdes d'altération de table (*Schema migration*) pour déployer de nouvelles fonctionnalités applicatives.
*   **Fragmentation et élasticité automatique :** Capacité à répartir automatiquement les données sur plusieurs serveurs sans intervention de la couche applicative. Des serveurs physiques peuvent être ajoutés ou retirés à chaud (élasticité à la volée).
*   **Cohérence pratique :** Pour l'utilisateur final, le système offre une illusion de cohérence robuste grâce aux réglages fins des algorithmes de réplication.
*   **Intégration Cloud :** S'intègrent nativement avec les infrastructures Cloud et les systèmes de virtualisation modernes.
*   **Flexibilité des schémas :** La structure des objets stockés peut évoluer à tout moment sans impact bloquant pour l'application.

#### 2. Inconvénients généraux des bases de données NoSQL
*   **Jeunesse de la technologie :** Manque relatif de maturité par rapport aux systèmes relationnels éprouvés depuis plusieurs décennies.
*   **Supervision et outillage :** Les outils de monitoring, de débogage et d'administration système sont moins développés, ce qui peut freiner leur adoption dans certains environnements de production hautement critiques.
*   **Image élitiste :** Le NoSQL souffre parfois de l'image d'une technologie complexe réservée uniquement aux très grandes entreprises du Web manipulant de gigantesques volumes de données.