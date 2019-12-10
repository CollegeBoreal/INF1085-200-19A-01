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

##  4.Designate a share directory 

```# mkdir -p /samba/sharehome 
   # touch /samba/sharehome/myfile
   # chmod 777 /samba/sharehome
   # nano /etc/samba/smb.conf
```

##  3.Define the share through the edit smb.conf.file

```
[sharehome] path = /samba/sharehome writable = yes
```
## 4. Test the configuration. 

```
#testparm
$ su sambauser
# smbclient //localhost/sharehome 
```








