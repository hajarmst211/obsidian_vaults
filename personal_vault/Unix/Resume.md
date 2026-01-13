## Users and groups
1. To access the root folder:
```bash
sudo -H /bin/bash
```
\* specifies that it should search the whole folder
##### Access rights:
```bash
drwxrwx---
```
- the first characters says if the eleement is a <span style="color:rgb(242, 130, 130)">directory or not</span>
- the next three characters are the access rights of <span style="color:rgb(242, 130, 130)">the owner</span>
- the next three characters are the access rights of <span style="color:rgb(242, 130, 130)">the group members</span>
- the next three characters are the access rights of <span style="color:rgb(242, 130, 130)">other users</span>
##### Users
###### Add user:
```bash
useradd -m -G _additional_groups_ -s _login_shell_ _username_
```
`-m`/`--create-home`
`-u assign id`
`-g/`--initial goupe`
`-G`/`--groups`:a comma separated list of supplementary groups which the user is also a member of.
`-s`/`--shell`: a path to the user's login shell.

###### View which groups a user is in:
```bash
groups username
```
###### System user:
```bash
useradd --system -s /usr/bin/nologin _username_
```
###### changing directory or username
```bash
usermod -d _/my/new/home_ -m _username_
```
the `-m` option also automatically creates the new directory and moves the content there.
###### delete a user
```bash
userdel -r user_id
```
###### To change other users password:

```
sudo passwd USERNAME
```
###### to login as a user:
```bash
su username
su - username 
```
- su - : 
	- Changes the working directory to the target user's home directory
	- resets the shell settings
	
###### making a file executable:
```bash
chmod +x path/to/our/file
```
###### Passwords:
• Modifier le mot de passe d'un utilisateur:
 `passwd Nom_utilisateur`
• Supprimer le mot de passe d'un utilisateur:
`passwd -d Nom \_utilisateur`
• Verrouiller le compte d'un utilisateur, ce qui empêche sa
connexion:
`passwd -l Nom_utilisateur`
• Déverrouiller le compte d'un utilisateur:
 `passwd -u Nom_utilisateur ou # passwd -d Nom_utilisateur`

##### Groups:
###### creating a group
```bash
groupadd group
```
###### adding user to a group
```bash
gpasswd -a _user_ _group_
```
###### renaming a group
```bash
groupmod -n _new_group_ _old_group_
```
# ou passwd Nom_utilisateur
##### Grep:
1. that start with "word": \*
```bash
grep "^word" *
```
2. Lines the end with "word" in file: $
```bash
grep "word$" file.txt
```
3. to find a
```bash
grep –e "Agarwal" –e "Aggarwal" –e "Agrawal" geekfile.txt
```
4.  Use -f to Read Patterns from a File
```bash
grep –f pattern.txt  geekfile.txt
```

## Bash scripting:
- To create a script, start with the shebang `#!` followed by the path to Bash, usually `/bin/bash`. Make sure your script has execute permissions.
- \# are for comments
- to assign a value to a variable use = without spacing
- $ for variable values 
- Logical Operators:
	- `&&`: Logical AND
	- `||`: Logical OR
	- `!`: Logical NOT
-  File Test Operators:
	- `-e`: Checks if a file exists
	- `-d`: Checks if a directory exists
	- `-f`: Checks if a file is a regular file
	- `-s`: Checks if a file is not empty
-  Defining function: 
```bash
my_function() {
  echo "Hello, World!"
}
```
- Calling a function: `my_function`
- Function with arguments:
```bash
greet() {
  local name=$1 #this $1 stnads for 'the first argument'
  echo "Hello, $name!"
}
greet "Alice"
```
- Arrays creation: `my_array=("value1" "value2" "value3")`
- Accessing array elements: `${my_array[0]}`
- Modifying: `my_array[1]="new_value"`
- 
