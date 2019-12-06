# SAMBA

## 1.Introduction 

VPN en anglais "Virtual Private Network" (réseau privé virtuel) désigne un réseau crypté dans le réseau Internet, qui permet à une société dont les locaux seraient géographiquement dispersés de communiquer et partager des documents de manière complètement sécurisée, comme s'il n'y avait qu'un local avec un réseau interne.D'une maniere terre a terre il permet d'échanger des informations de manière sécurisée et anonyme en utilisant une adresse IP différente a celle de votre ordinateur.

### 2.Notre but 
- Créer un tunnel de réseau privé virtuel (VPN) permettant des connexions distantes sécurisées et invisibles.
- Concevoir des architectures de pare-feu plus sophistiquées pour diviser votre réseau de manière stratégique en segments isolés.
- Créer un environnement de réseau virtuel afin de pouvoir tester vos configurations.

## 1. Create a Samba user account on the Linux server. 

```sudo apt-get update
```
```sudo -i
```
``` sudo apt-get install samba
```
##  2.Designate a share directory 

```# mkdir -p /samba/sharehome 
   # touch /samba/sharehome/myfile
   # chmod 777 /samba/sharehome
   # nano /etc/samba/smb.conf
```

###  3.Define the share through the edit smb.conf.file

```[sharehome] path = /samba/sharehome writable = yes
```

#### 4. Test the configuration. 
```#testparm
```

```$ su sambauser
```








