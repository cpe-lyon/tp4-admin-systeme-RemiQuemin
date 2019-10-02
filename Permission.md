# tp4-admin-systeme-RemiQuemin


### Exercice 1. Gestion des utilisateurs et des groupes
#### • Commencez par créer deux groupes groupe1 et groupe2
Tout d'abord pour avoir les droits de créer les deux groupes : `sudo su` <br>
Par la suite `addgroup groupe1`, `addgroup groupe2`.

#### • Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell.
Création des différents utilisateurs :  
```
useradd u1 -m
useradd u2 -m 
useradd u3 -m
useradd u4 -m
```
#### • Placez les utilisateurs dans les groupes :
#### - u1, u2, u4 dans groupe1
Pour placer user u1 dans le groupe groupe1 :
``` 
usermod -a -G groupe1 u1
usermod -a -G groupe1 u2
usermod -a -G groupe1 u4
``` 

#### - u2, u3, u4 dans groupe2
Pour placer par exemple user u1 dans le groupe secondaire groupe2 :
``` 
usermod -a -G groupe2 u2
usermod -a -G groupe2 u3
usermod -a -G groupe2 u4
``` 
#### • Donnez deux moyens d’afficher les membres de groupe2
Deux moyens d'afficher les membres de groupe2
-`grep '^groupe2' /etc/group`. <br>
-Sinon avec le paquet "members" avec `apt get install members`ensuite il suffit de faire `members groupe1` et `members groupe2`.

#### • Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4.
Pour faire de groupe1 le groupe proprietaire on utilise la commande `chown`.
```
chown u1:group1 /home/u1
chown u1:group1 /home/u2 
chown u1:group2 /home/u3 
chown u1:group2 /home/u4 
```

#### • Remplacez le groupe primaire des utilisateurs :
#### - groupe1 pour u1 et u2 <br>
#### - groupe2 pour u3 et u4 <br>
Pour remplacer le groupe primaire des utilisateurs : 
```
usermod -g groupe1 u1 
usermod -g groupe1 u2
usermod -g groupe2 u3
usermod -g groupe2 u4
```

#### • Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
Tout d'abord je crée mes deux repertoires : `mkdir /home/groupe1 /home/groupe2`, ensuite `chown u1: groupe1 groupe1` et `chown u2: groupe groupe2`, enfin je donne les permissions :  `chmod -R 720 groupe2` et `chmod -R 720 groupe1`.

#### • Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
Nous pouvons utiliser le sticky bit : 
```
chmod +t /home/groupe1
chmod +t /home/groupe2
```

#### • Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
Non il n'est pas possible de se connecter en tant que u1 avec la commande `su u1`, en effet celui-ci n'a pas de mot de passe, ont ne peut donc pas se connecter.

#### • Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
Tout d'abord nous initialisons le mot de passe de u1: `sudo passwd u1` <br>
```
[sudo] password for serveur:
New password:
Retype new password:
passwd: password updated successfully
```
Il est maintenant possible de se connecter a u1.

#### • Quels sont l’uid et le gid de u1 ?
En tapant `id` : 
```
uid=1001(u1) gid=1005(groupe1) groups=1005(groupe1)
```

#### • Quel utilisateur a pour uid 1003 ?
```
id 1003
uid=1003(u3) gid=1006(groupe2) groups=1006(groupe2)
``` 
C'est l'utilisateur u3.

#### • Quel est l’id du groupe groupe1 ?
`"grep "groupe1" /etc/group`.
• Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)
`grep "1002" /etc/group` : C'est le groupe 2.

• Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
`sudo gpasswd -d u3 groupe2`
Quand on se connecte à u3, on constate qu'il n'a plus accès au dossier groupe2.

#### • Modifiez le compte de u4 de sorte que :
##### — Il expire au 1er juin 2019
`root@serveur:/home# sudo chage -E 60/01/2019 u4`.
##### — Il faut changer de mot de passe avant 90 jours
`root@serveur:/home# sudo chage -M 90 u4`.
##### — Il faut attendre 5 jours pour modifier un mot de passe
`root@serveur:/home# sudo chage -m 5 u4`.
##### — L’utilisateur est averti 14 jours avant l’expiration de son mot de passe
`root@serveur:/home# sudo chage -W 14 u4`.
##### — Le compte sera bloqué 30 jours après expiration du mot de passe
`root@serveur:/home# sudo chage -I 30 u4`.

#### • Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
L'interpréteur de commandes de l'utilisateur est /bin/bash, à l'aide de la commande `echo $SHELL`

#### • à quoi correspond l’utilisateur nobody ?
l'utilisateur nobody est un compte utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.

#### • Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? 
La commande sudo conserve le mot de passe en memoire pendant 15 minutes.
#### Quelle commande permet de forcer sudo à oublier votre mot de passe ?
la commande qui permet de forcer sudo à oublier son mot de passe est sudo -k.


### Exercice 2. Gestion des permissions

#### • Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?
Tout d'abord je cree le dossier : `mkdir test`,  `cd test`, `nano fichier`.
Les droits du dossier test sont : 755  et les droits du fichier fichier sont 644.

####  • Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
Pour retirer tous les droits sur ce fichier , il faut faire `chmod 000 fichier`. J'arrive à le modifier et a l'afficher car je suis en root, les droits d'accès ne s'applique donc pas pour le root.

####  • Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
Pour redonner les droits en ecriture et execution : `chmod 300 fichier`. <br>
Avec `"echo hello world" > fichier`. Quand on fait : 
``` sudo cat fichier
hello world
```
Cela signifie que nous pouvons de nouveau écrire dans le fichier "fichier".

####  • Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
Nous n'avons pas les droits d'exécution, l'accès est refusé. Cependant, en root ca marche parce qu'il a tout les droits.

#### • Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
`sudo chmod u-r ../test`. On ne peut plus lister le contenu du repertoire depuis que nous avons retiré le droit de lecture. 

#### Rétablissez le droit en lecture sur test

#### • Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?
```
serveur@serveur:~/test$ nano nouveau
serveur@serveur:~/test$ mkdir sstest
serveur@serveur:~/test$ chmod u-w nouveau
serveur@serveur:~/test$ chmod u-w ../test
serveur@serveur:~/test$ nano nouveau
[ Error writing nouveau: Permission denied ]
serveur@serveur:~/test$ chmod u+w ../test
serveur@serveur:~/test$ nano nouveau
[ Error writing nouveau: Permission denied ] 
serveur@serveur:~/test$ rm nouveau
rm: remove write-protected regular file 'nouveau'? yes
serveur@serveur:~/test$ ls
fichier  sstest
```

#### • Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
```
chmod u-x test
nano test/nouveau
[ Path 'test' is not accessible ]
cd test/
-bash: cd: test/: Permission denied
ls test
ls: cannot access 'test/sstest': Permission denied
ls: cannot access 'test/fichier': Permission denied
fichier  sstest
``` 

#### • Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?

#### • Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

#### • Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

#### • Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

#### • Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

#### • Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
- chmod u=rx,g=wx,o=r fic
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
- chmod 653 fic en sachant que les droits initiaux de fic sont 711
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---

#### • Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?


