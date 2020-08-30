# Develpy-Write-up
Write-up Room Develpy Tryhackme


Cliquez [ici](https://tryhackme.com/room/bsidesgtdevelpy) pour accéder au room.

>Première étape : Identifier la cible et ses vulnérabilités.

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
![](img/Develpy_nc.png?raw=true)

