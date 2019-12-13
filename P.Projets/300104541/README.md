# SAMBA:Sharing files with Windows users using Samba

## 1.Introduction
```
Samba est un logiciel d'interopérabilité qui implémente le protocole propriétaire SMB/CIFS de Microsoft Windows dans les ordinateurs tournant sous le système d'exploitation Unix et ses dérivés de manière à partager des imprimantes et des fichiers dans un réseau informatique. Samba facilite l'interopérabilité entre systèmes hétérogènes Windows-Unix. Il offre la possibilité aux ordinateurs d'un réseau d'accéder aux imprimantes et aux fichiers des ordinateurs sous Unix9 et permettent aux serveurs Unix de se substituer à des serveurs Windows10.
```
### 2.Notre but 
```

1- Créez un compte utilisateur Samba sur le serveur Linux.
2- Désigner un répertoire de partage.
3- Définissez le partage via le fichier d'édition smb.conf.
4- Testez la configuration.
5- Connectez-vous à partir d'un client Windows.
```

## 3.1 Créez un compte utilisateur Samba sur le serveur Linux.
   ## 3.1.2 Installer Samba 
   
 ```
$ sudo apt-get install samba
$ sudo apt-get update
 ```
```
Ensuite vous devrez utiliser un programme appelé smbpasswd pour configurer un utilisateur Samba en tant que compte que les clients utiliseront pour se connecter. Mais comme l'autorité Samba sera ajoutée à un compte existant, vous devez d'abord créer un nouveau compte Linux.Vous pouvez choisir n'importe quel nom:
- # adduser sambauser 
- # smbpasswd -a sambauser
```

##  4.Designer un repertoire de partage  
```
Créer un répertoire où sera basé le partage. Pour faciliter les tests ultérieurs, il faut donc cree un nouveau répertoire. Étant donné que plusieurs clients peuvent finir par travailler avec des fichiers dans ce répertoire:

- # mkdir -p /samba/sharehome
- # touch /samba/sharehome/myfile
```
```
Vous pouvez éviter les problèmes d'autorisations potentiels en utilisant chmod pour ouvrir les autorisations de répertoire au 777 (lecture, écriture et exécution pour tous les utilisateurs):

- # chmod 777 /samba/sharehome
```
```
C'est l'environnement Samba tout construit. Vous pouvez maintenant ajouter une configuration au fichier smb.conf dans le répertoire / etc / samba /. Vous devriez parcourir le fichier de configuration pour avoir une idée du degré de personnalisation possible et de la complexité des choses:
 - # nano /etc/samba/smb.conf
 ```

##  3. Définir le partage via le fichier d'édition smb.conf.
```
En etant dans nano taper les commandes suivantes sans les commentaires (#):
```
```
[sharehome] 
path = /samba/sharehome 
writable = yes
```
Puis enregister la modifacation et ensuite sortez.
```
Utilisez maintenant systemctl pour démarrer et activer le démon Samba. Ubuntu connaît Samba comme smbd, mais sur CentOS, ce sera smb (sans le d):
```
```
# systemctl start smbd
# systemctl enable smbd
```
## 4. Testez la configuration
```
Il est toujours mieux d assurer ces arrieres avant d aller un peu plus loin, du coup  testparm sera notre moyen de verification c est a dire,il montrera si la section que vous avez ajoutée peut être correctement lue par le service Samba. Cette sortie suggère que tout va bien:
```
```
- # testparm
```
Encore un test avant d'inviter vos amis Windows: vous pouvez utiliser le programme smbclient pour vous connecter à votre partage Samba depuis la machine locale. Vous devez d'abord basculer l'utilisateur (su) vers le compte sambauser que vous avez associé à Samba plus tôt. Vous pouvez ensuite exécuter smbclient sur l'adresse et le nom d'hôte du partage (// localhost / sharehome
va falloir d abord cree Samba client et ensuite proceder aux etapes suivantes :

``` 
First il faut basculer (su) vers le compte sambauser que vous avez associe a Samba 
```
```
$ su sambauser
Password: 
```
```
Ensuite il faut creer Samba Client
```
```
- sudo apt-get install smbclient.
```
```
# smbclient //localhost/sharehome 
Enter sambauser's password:
```

# Resultats 
```
# smbclient //localhost/sharehome
Enter sambauser's password:
Domain=[WORKGROUP] OS=[Windows 6.1] Server=[Samba 4.3.11-Ubuntu] 
smb: \>
```

# 5.Connectez-vous à partir d'un client Windows
```
-Clique droite  sur le logo de Windows ensuite appuye sur run 
- Tape: \\10.13.237.41\sharehome
```
```
Exemple
```
![image](https://github.com/CollegeBoreal/INF1085-200-19A-01/blob/master/P.Projets/300104541/sketch5.png?raw=true)
![image]()



```
su sambauser password: !Bernadette
```




