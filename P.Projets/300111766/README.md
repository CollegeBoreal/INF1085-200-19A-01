##  ProjetSamba :  partage de fichier Linux/Windows
   
   Introduction
   
  📌 Qu'est-ce que samba ?
   Samba est un logiciel libre sous licence GPL, permettant de supporter le protocole SMB/CIFS le partage de ressources réseau.
Le partage réseau a été développé par IBM en 1985 pour OS/2 et s'appelait alors LAN Manager. Il a ensuite été mis en avant par Microsoft sous le nom de SMB pour en faire un tout nouveau protocole de partage. Il a été appelé aussi CIFS (Common Internet File System ) entre 1998 et 2006, et a encore été renommé sous le nom de SMB2 pour la sortie de Windows Vista et conserve ce nom jusqu'à Windows 8.

 
 📌Installation

1. La première chose que nous devons faire avant de configurer un partage SMB / CIFS sur notre Raspberry Pi est de nous assurer que tout est à jour.

Nous pouvons mettre à jour la liste des packages et tous nos packages en exécutant les deux commandes suivantes.

  sudo apt-get update
  sudo apt-get upgrade
  
2. Maintenant que notre système d'exploitation Raspbian est entièrement à jour, nous pouvons maintenant procéder à l'installation du logiciel Samba sur notre Raspberry Pi.
Nous pouvons installer les packages dont nous avons besoin pour configurer Samba en exécutant la commande suivante.
Ouvrir dans Google Traduction

  sudo apt-get install samba samba-common-bin
  

3. Avant de configurer notre stockage réseau sur notre Pi, nous devons d'abord créer un dossier que nous partagerons.
 nous allons créer le répertoire dans le répertoire personnel des utilisateurs «pi».
Créez ce dossier en exécutant la commande suivante.
  mkdir /home/pi/shared

4. Nous pouvons maintenant partager ce dossier à l'aide du logiciel Samba. Pour ce faire, nous devons modifier le fichier de configuration samba.
  sudo nano /etc/samba/smb.conf
  
