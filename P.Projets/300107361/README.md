# PROJECT INSTALLATION DE SAMBA SUR RASPBIAN :penguin:  > :strawberry: (NETWORK FILE SHARING) 

# INTRODUCTION

Le partage de dossiers et d'imprimantes dans un réseau local est une fonctionnalité des systèmes d'exploitation modernes permettant d'accéder à des ressources d'un 
ordinateur (dossiers de données et imprimantes) à partir d'un autre ordinateur situé dans un même réseau local (réseau domestique ou d'entreprise).

Le logiciel Samba est un outil permettant de partager des dossiers et des imprimantes à travers un réseau local.
Il permet de partager et d'accéder aux ressources d'autres ordinateurs fonctionnant avec des systèmes d'exploitation Microsoft® Windows® et Apple® Mac OS® X, ainsi que des systèmes GNU/Linux, *BSD et Solaris dans lesquels une
implémentation de Samba est installée.


# INSTRUCTIONS
## STEP :one:
Il faut d'abord verifier et installer les dernieres pacquets de mise a jour et de mise a niveaux pour Raspberry Pi.
Ouvrer le Terminal (PuTTY ou GITBash) si vous vous connectez a distance, ou le CLI(Commande Line Interface) si vous etes directement connecte au Raspberry.

Puis lancer la commande suivante:
```
~ $ sudo apt-get update && sudo apt-get upgrade
``` 

Cette commande installe les mises a jour et lance le mise a niveaux une fois ce dernier termine.


## STEP :two:
Apres en etre sure que tous les paquets sont bien installes, vous pouvez maintenant installer SAMBA utilisant les commandes suivantes:
```
 ~ $ sudo apt-get install samba samba-common-bin
```

## STEP :three:(Optional)
Si vous ne voulez pas partager tous les fichiers dans le Raspberry Pi, vous pouvez creer un **fichier de partages** dans lequel vont se trouver les fichiers 
designes au partage, en utilisant cette commande:

```
~ $ sudo -i
~ # mkdir ~/share 
```

##  STEP :four:
Maintenant vous devez configurer Samba pour commencer a partager les 
fichiers de repertoires du Raspberry Pi avec d'autres ordinateurs 
connectes sur le reseau. 

Entrez les commandes suivantes:

```
 ~ # sudo nano /etc/samba/smb.conf 
```
Le fichier de configuration de Samba est bien documenté. Vous pouvez faire défiler le fichier pour voir ce que vous souhaitez activer.

Vous pouvez également tout supprimer en appuyant longuement sur ```CTRL+K``` ou copiez et collez simplement le code suivant au bas du fichier:

```
[global]
netbios name = Pi
server string = The Pi File Center
workgroup = WORKGROUP

[HOMEPI]
path = /home/pi
comment = No comment
writeable=Yes
create mask=0777
directory mask=0777
public=no
```

### Voici une courte explication de la signification du code ci-dessus:

* ` WORKGROUP` : c'est le domaine dont le serveur Samba fera partie. Par défaut, Windows a le groupe de travail défini comme WORKGROUP.
* `PATH`: c'est le chemin du répertoire du Raspberry Pi qui sera partagé.
* `WRITEABLE`: s'il est défini sur oui, il permettra au dossier  d'être accessible en écriture.
* `CREATE MASK` and `DIRECTORY MASK` : lorsqu'il est défini sur 0777, l'utilisateur peut lire, écrire et exécuter.
* `PUBLIC`: s'il est défini sur no, il autorisera uniquement les utilisateurs valides à accéder au dossier partagé.

Après avoir entré les informations ci-dessus, appuyez et maintenez `CTRL+O` touche `ENTER` puis `CTRL+X` pour enregistrer les modifications et quitter.


##  STEP :five:

* :a:
À l'heure actuelle, nous n'avons que la configuration de l'utilisateur Pi dans le Raspberry Pi. Nous devons maintenant ajouter Pi en tant qu'utilisateur Samba.

Entrez la commande suivante pour l'ajouter au  serveur de SAMBA:

```
 ~ #sudo smbpasswd -a pi 
```
Entrez un nouveau mot de passe lorsque vous y êtes invité par le prompt. 

Vous pouvez utiliser le même mot de passe que votre utilisateur Raspberry Pi mais pour 
des raisons de sécurité, entrez un mot de passe différent.
* :b:

Creation d'un autre utilisateur si necessaire:
```
~ # adduser x
```
Et lui deleguer un mot de passe:
```
~ # smbpasswd -a x
    Password: ******
```

##  STEP :six:
Enfin, redémarrez le service Samba à l'aide de la commande suivante:

``` 
~ # sudo service smbd restart 
```
Et lancer les commandes suivantes pour lancer Samba a partir de Systemctl
```
~ # systemctl start smbd
~ # systemctl enable smbd
```

OK, donc Samba est maintenant configuré. Vous pouvez tester la configuration en utilisant la commande:

``` 
 ~ # test parm
```

## Comment Y Acceder?
Ouvrez l'Explorateur de fichiers Windows et cliquer sur Réseau pour accéder au dossier partagé Raspberry Pi sur **Windows**.  

Lorsque vous cliquez sur le dossier, vous serez invité à saisir vos informations d'identification. 
Saisissez le nom d'utilisateur du Pi, puis le mot de passe Samba que vous avez créé pour cet utilisateur. 

Une fois connecté, vous pourrez gérer les dossiers et fichiers du Pi.

### A partir du Pi
Lancer la commande suivante dans le CLI:
-------------- --------
`~ $ su x` or  `su pi`
-------------- --------
Vous serez ensuite demandez a vous authentifier
```
Password: *********
````

``` :warning: en progres ```
.
.
.

.
.
.
.
.
.
.
.
Pour partager de manière simple des ressources entre plusieurs ordinateurs, 
l'utilisation de Samba est conseillée.

