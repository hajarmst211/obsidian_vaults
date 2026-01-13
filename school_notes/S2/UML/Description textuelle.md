# Identification:
- cas: Réserver un séjour
- Acteur: gérant
- Responsable: Fatima Moustiane
- Version: 1
- Resume: L'agent planifie et d'enregistre un voyage complet pour un client, incluant le transport et le logement.

# Besoins fonctionnels:
##### Pre-conditions:
- les informations du client
- une place disponible dans la croisière
##### Scenario nominal:
1. Le client se presente a l'agent de la companie et fournit ses informations
2. L'agent verifie si il y a une place disponible dans la croisier
3. Si oui, l'agent reserve de plus du séjour, hébergement et le transport pour ce client.

##### Scenario alternatif
###### 1: demande de facture detaillee
L'agent remet une facture detaillee au client
###### 2: demande d'un taxi
L'agent (Le systeme informatique) demande un taxi pour le client

##### Scenario d'erreur:
###### 1: pas de place disponible
->Annulation de la reservation
###### 2: Gestion du retard client au départ (Avion/Train)
-> Decaler  le voyage

##### Post-condition
-> Confirmation de la reservation

# Besoin non fonctionnels
- Sécurité
- Performance: Synchronisation avec la base de donnée
- Rapidité du logiciel



