# :snowflake: Installation de Samba Sur un Rasberry Pi4

### :one: Se connecter sur le Rasberry Pi en ulissant SSH

````$ ssh pi@10.13.237.79````

### :two: Mettre à jour en utilisant les commandes

````$ sudo apt-get update & sudo apt-get upgrade````

### :three: Installer Samba

````$ sudo apt-get install samba samba-common-bin````


![image](samba.PNG)

### :four: Créer un répertoire "shared" qu'on aura à partager

````$ mkdir /home/pi/shared````


![image](shared.PNG)

### :five: Modifier le fichier de configuration de Samba "smb.conf"

````$ sudo nano /etc/samba/smb.conf````


![image](conf.PNG)

### :six: Ajouter ceci vers la fin de la page

![image](777.PNG)

:page_with_curl:
### :hash: [Pimylifeupshare]

Ceci définit le partage lui-même, le texte entre les crochets est le point auquel vous accéderez au partage. Par exemple, la nôtre sera à l'adresse suivante: //10.13.237.79/pimylifeupshare

### :hash: « path » - 

Cette option est le chemin vers le répertoire de votre Raspberry Pi que vous souhaitez partager.

### :hash: « writeable »

Lorsque cette option est définie sur « yes », cela permettra au dossier d'être inscriptible.

### :hash: «create mask» et « directory mask » - 

Cette option définit les autorisations maximales pour les fichiers et les dossiers. La définition de 0777 permet aux utilisateurs de lire, d'écrire et d'exécuter.

### :hash: « Public »

Si ce paramètre est réglé sur « no », le Pi aura besoin d'un utilisateur valide pour accorder l'accès aux dossiers partagés.

### :seven: Quitter le terminal

Dans notre cas CTRL+O puis ENTER après CTRL+X

### :eight: Changer le mot de passe, le mot de passe par defaut est "raspberry"

````$ sudo smbpasswd -a pi````

![image](password.PNG)

### :nine:  Redémarrage de raspberry

Enfin, avant de nous connecter à notre partage Raspberry Pi Samba, nous devons redémarrer le service samba afin qu'il se charge dans nos modifications de configuration.

````$ sudo systemctl restart smbd````

![image](restart.PNG)

### :keycap_ten: Exécutez la commande ci-dessous pour imprimer l'adresse IP locale du Pi.

````$ hostname -I````

![image](ip.PNG)

### :one::one: Connecter à votre Samba sous Windows 10

Commencez par ouvrir « file explorer ».

![image](win.PNG)

Cliquez sur map drive

![image](map.PNG)

Puis, entrez le text pour mapper le lecteur réseau

\\10.13.237.79\pimylifeupshare

![image](map1.PNG)

### :one::two: Se connecter à votre repertoire "shared"

Utilisateur: pi

Mot de passe: Djumah9

![image](cred.PNG)

### :one::three: Le resultat final

![image](final.PNG)





