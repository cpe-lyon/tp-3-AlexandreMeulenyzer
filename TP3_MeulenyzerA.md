# TP 3 - Utilisateurs, groupes et permissions

Exercice 1. Gestion des utilisateurs et des groupes  

1/  
![](file:///C:/Users/Administrator/Pictures/Capture%20d’écran%202022-09-22%20081154.jpg)

2/  
```bash
useradd alice  
useradd bob  
useradd charlie  
useradd dave  
```

3/  
![](../Pictures/Capture%20d’écran%202022-09-22%20084049.jpg)

4/  
cat /etc/group

![](../Desktop/Capture%20d’écran%202022-09-22%20083949.jpg)  
cat /etc/passwd

![](../Pictures/Capture%20d’écran%202022-09-22%20084210.jpg)

5/  
```bash
root@localhost:/home# sudo chgrp dev /home/bob
root@localhost:/home# sudo chgrp dev /home/alice
root@localhost:/home# sudo chgrp infra /home/charlie
root@localhost:/home# sudo chgrp infra /home/dave
```
6/  
```bash
usermod -g dev bob
usermod -g dev alice
usermod -g infra charlie
usermod -g infra dave
```
7/
```bash
root@localhost:/home# chmod a+rw infra
root@localhost:/home# chmod a+rw dev
```
8/
On dois utilisé le sticky bit, en faisait chmod +t dossier

9/
On ne peut pas ouvrir la session Alice car le compte n'est pas activé et mot de passe pas attribué

10/
une fois le compte activé avec la commande `passwd alice`, nous pouvons nous connecté à sa session sans problème

11/ En allant dans le dossier /etc/passwd

12/
```bash
root@localhost:/home# grep "1003" /etc/passwd
bob:x:1003:1002::/home/bob:/bin/bash
charlie:x:1004:1003::/home/charlie:/bin/bash
dave:x:1005:1003::dave:/bin/bash
```
13/l'ID est 1002 en faisant cat /etc/group
`dev:x:1002:` 

14/
Vue que j'ai d'abord crée dev, c'est lui qui a le GID 1002, alors que infra a le 1003
15/
```bash
User@localhost:~$ sudo gpasswd -d charlie infra
Removing user charlie from group infra
```

16/
```bash
root@localhost:/home/User# chage dave
Changing the aging information for dave
Enter the new value, or press ENTER for the default

        Minimum Password Age [0]: 
        Maximum Password Age [90]: 
        Last Password Change (YYYY-MM-DD) [2022-09-22]: 
        Password Expiration Warning [7]: 
        Password Inactive [-30]: 
        Account Expiration Date (YYYY-MM-DD) [2023-06-01]: 
       
```
17/
C'est l'invite de commande en # et non pas en $

18/ 
nobody est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient.

19/ 15 minute par défaut, et pour sortir de sudo su : logout

# Exercice 2. Gestion des permissions

1/ User a le droit d'écrire et de lire, les groupes seulement de lire, et les autres seulement de lire.  
annexe :
![](../Pictures/Capture%20d’écran%202022-09-22%20103059.jpg)

2/ En retirant tout les droits, seulement en utilisant root (sudo) nous avons accès au fichier.

3/Nous n'avons pas besoin d'avoir les droits de lire un fichiers pour le modifier

4/ Nous n'avons pas les permissions nécessaires, car nous avons besoin du droit de lecture en plus du droit d’exécution pour exécuté un fichier.

5/Nous n'avons pas le droit de lecture sur le répertoire donc le ls ne marche pas, néanmois nous avons le droit sur le fichier donc nous pouvons l'affiché et l'exécuté

6/ Si le fichier nous refuse des droits mais pas le dossier, c'est les droits du fichiers qui sont prioritaires.

7/En étant a l’extérieur du répertoire, sans les droits d'executions, nous ne pouvons pas rentré dedans, supprimé, modifié, de fichier, nous ne pouvons qu'affiché le contenu du fichier.

8/En étant dans le répertoire, nous ne pouvons rien faire.

9/D'abord j'enleve les droits a tout le monde sauf le créateur, puis je permet a tout le monde de lire le fichier "fichier"
```bash
User@localhost:/home/test$ sudo chmod go-rwx fichier 
User@localhost:/home/test$ sudo chmod a+r fichier 
```

10/ ici je fais en sorte que seul le créateur puisse accédé au répertoir/fichier qu'il créer
```bash
User@localhost:/home$ umask go-rwx
User@localhost:/home$ umask 
0077
````

11/
`umask 027` donc u=rwx, g=rx, o=


13/ 
- chmod u=rx,g=wx,o=r fic = chmod 534
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x--- = chmod 602
- chmod 653 fic en sachant que les droits initiaux de fic sont 711 = chmod u=rw, g=rx, o= wx
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x--- = chmod 570   