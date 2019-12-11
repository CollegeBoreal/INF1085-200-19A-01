                                  PARETAGE DE FICHIER LINUX/WINDOWS: SAMBA
   
   Introduction
   
  📌 Qu'est-ce que samba ?
   Samba est un logiciel libre sous licence GPL, permettant de supporter le protocole SMB/CIFS le partage de ressources réseau.
Le partage réseau a été développé par IBM en 1985 pour OS/2 et s'appelait alors LAN Manager.
 
 📌Installation

 1️⃣  La première chose que nous devons faire avant de configurer un partage SMB / CIFS sur notre Raspberry Pi est de nous assurer que tout est à jour.
Nous pouvons mettre à jour la liste des packages et tous nos packages en exécutant les deux commandes suivantes.

          ~ $ sudo apt-get update && sudo apt-get upgrade
   
  
 2️⃣  Maintenant que notre système d'exploitation Raspbian est entièrement à jour, nous pouvons maintenant procéder à l'installation du logiciel Samba sur notre Raspberry Pi.
Nous pouvons installer les packages dont nous avons besoin pour configurer Samba en exécutant la commande suivante.

          ~ $ sudo apt-get install samba samba-common-bin
       
  
 3️⃣  Avant de configurer notre stockage réseau sur notre Pi, nous devons d'abord créer un dossier que nous partagerons.
nous allons créer le répertoire dans le répertoire personnel des utilisateurs «pi».
Créez ce dossier en exécutant la commande suivante.

        
          ~ # mkdir /home/pi/shared

        
  4️⃣  Nous pouvons maintenant partager ce dossier à l'aide du logiciel Samba. Pour ce faire, nous devons modifier le fichier de configuration samba.
 
          ~ # sudo nano /etc/samba/smb.conf
 
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

 NB Après avoir fait les modifications sur nano, appuyez et maintenez CTRL+O touche ENTER puis CTRL+X pour enregistrer les modifications   et quitter. 

5️⃣  dans cet etape nous devons ajouter Pi en tant qu'utilisateur Samba. Pour ce faire nous devons entrez la commande suivante pour l'ajouter au serveur de SAMBA:

          ~ #sudo smbpasswd -a pi
 
6️⃣   Mintenant il nous reste a redémarrer le service Samba à l'aide de la commande suivante:

          ~ # sudo service smbd restart

  Pour lancer Samba entrer ces commandesa partir de Systemctl
  
          ~ # systemctl start smbd
          ~ # systemctl enable smbd

  
  Apres la configuration de Samba, ous pouvez le tester en utilisant la commande:
               
          ~ # test parm

   
   NB : Pour acceder a notre fichier de partage ouvrez l'Explorateur de fichiers Windows et cliquer sur Réseau et accéder au dossier partagé Raspberry Pi sur Windows. Ares cliquez sur le dossier et saisissez les informations d'identification comme le nom d'utilisateur du Pi et le mot de passe qu'on avait mis pour l'utilisateur de Samba.


