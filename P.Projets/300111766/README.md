##  ProjetSamba :  partage de fichier Linux/Windows
   
   Introduction
   
  📌 Qu'est-ce que samba ?
   Samba est un logiciel libre sous licence GPL, permettant de supporter le protocole SMB/CIFS le partage de ressources réseau.
Le partage réseau a été développé par IBM en 1985 pour OS/2 et s'appelait alors LAN Manager. Il a ensuite été mis en avant par Microsoft sous le nom de SMB pour en faire un tout nouveau protocole de partage. Il a été appelé aussi CIFS (Common Internet File System ) entre 1998 et 2006, et a encore été renommé sous le nom de SMB2 pour la sortie de Windows Vista et conserve ce nom jusqu'à Windows 8.
  📌Installation
On commence donc par installer samba qui va nous permettre de rendre ce dossier accessible sur le réseau : pour ce faire on doit tapper la commande:


  # apt-get install samba
  # smbclient //localhost/sharehome
 
