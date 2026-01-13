	### Exercice 1:
La structure est: Mot de passe :GID :Liste utilisateurs autorisés
### Exercice 2:
```bash
❯ id bin
uid=1(bin) gid=1(bin) groups=1(bin),3(sys),2(daemon)
```
L'utilisateur bin existe et son id est 1.

### Exercice 3:
 En premier lieux, on cherche le root puis on cherche un utilisateur avec le meme droits.
 ![[Pasted image 20251007150648.png]]
 Donc, non root est le seul utilisateur avec ces droits.
### Exercice 4:
![[Pasted image 20251007150944.png]]
donc bin appartient au groups: bin, sys and daemon

### Exercice 5:
![[Pasted image 20251007151259.png]]
ali appartient au seul group: ali

### Exercice 6:
![[Pasted image 20251007151857.png]]
### Exercice 7:
![[Pasted image 20251007151921.png]]
### Exercice 8:
methode 1:
![[Pasted image 20251007173320.png]]

methode2: 
![[Pasted image 20251007174322.png]]
### Exercice 9:
pour se connecter correctement il faut définir un mot de pass
### Exercice 10:
![[Pasted image 20500314083348.png]]
![[Pasted image 20500314083356.png]]
### Exercice 11:
```bash
sudo useradd newuser
```
### Exercice 12:
![[Pasted image 20500314083733.png]]

### Exercice 13:
1. ![[Pasted image 20500314084224.png]]![[Pasted image 20251007175156.png]]
2. test de connection
![[Pasted image 20251007180211.png]]

3. ![[Pasted image 20251007180354.png]]
4. ![[Pasted image 20251007180423.png]]
5. ![[Pasted image 20251007180537.png]]
6. ![[Pasted image 20251007180607.png]]
7. ![[Pasted image 20251007181104.png]]
8. ![[Pasted image 20251007181158.png]]
9. ![[Pasted image 20251007181259.png]]
10. ```bash
    #!/bin/bash
USERS=("student01" "student02" "root" "newuser")
for user in "${USERS[@]}"; do
if id "$user" &>/dev/null; then
sudo usermod -L "$user"
echo "Compte $user desactive."
else
echo "Compte $user n'existe pas."
fi
done
    ```
### Exercice 14:
```bash
#!/bin/bash
read -p "nombre d'utilisateurs ?> " n
for ((i=1; i<=n;i++)); do
read -p "Enter le login pour $i : " login
read -s -p "Enter password: " pass
echo sudo useradd -m -s /bin/bash "$login"
echo "$login:$mdp" | sudo chpasswd
done
```
### Exercice 15:
1) Utilité de grep
La commande grep permet de rechercher dans un fichier les lignes contenant un mot ou un motif
donné.
2) Analyse de la commande
x=\$(grep '^id' /etc/inittab | cut -d : -f2); cd /etc/rc.d/rc$x.d; ls S* | cut -c4-
Extraction du niveau d’exécution : x=\$(grep '^id' /etc/inittab | cut -d : -f2)
Recherche la ligne commençant par id dans /etc/inittab et en extrait le 2e champ, puis le stocké dans
x
Accès au répertoire correspondant cd /etc/rc.d/rc$x.d
Affichage des scripts ls S* | cut -c4-
Liste les fichiers commençant par S (scripts de démarrage) et affiche leurs noms à partir du 4e
caractère, en supprimant le préfixe Sxx.