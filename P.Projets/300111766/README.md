##  ProjetSamba :  partage de fichier Linux/Windows
   
   Introduction
   
   Qu'est-ce que samba ?
   Samba est un logiciel libre sous licence GPL, permettant de supporter le protocole SMB/CIFS le partage de ressources réseau.
Le partage réseau a été développé par IBM en 1985 pour OS/2 et s'appelait alors LAN Manager. Il a ensuite été mis en avant par Microsoft sous le nom de SMB pour en faire un tout nouveau protocole de partage. Il a été appelé aussi CIFS (Common Internet File System ) entre 1998 et 2006, et a encore été renommé sous le nom de SMB2 pour la sortie de Windows Vista et conserve ce nom jusqu'à Windows 8.
Ces termes sont à connaître car ils apparaissent pour certains points de la configuration de samba, qui se charge de compatibilité avec ce protocole de partage réseau mais aussi avec différents produits Microsoft, comme NetBios (TCP/IP) et le nommage wins et maintenant DNS, et MSRPC (appel de procédure à distance), et il peut aussi être utilisé comme contrôleur de domaine “actif directory”. 
Le protocole SMB/CIFS est toujours un logiciel propriétaire, et ces spécifications étaient donc fermées au début. Mais pour des raisons d'intéropérabilité, suite à un procès, Microsoft a été contraint d'ouvrir certaines de ses spécifications qui ont été distribuées par le MSDN (open Specifications Developer Center). 
Samba a donc été créé afin de rendre possible la communication entre différents systèmes d'exploitations d'un même réseau, mais on peut aussi l'utiliser pour le partage de Linux à Linux.
On le doit d'abord aux travaux de rétro-ingénierie d'Andrew Tridgel qui est son principal développeur. Depuis 2007, et la fermeture des spécifications SMB le développement et les performances de samba ne cessent de se développer. 

I.Installation de Samba
On commence donc par installer samba qui va nous permettre de rendre ce dossier accessible sur le réseau : pour ce faire on doit tapper la commande:


  # apt-get install samba
  # smbclient //localhost/sharehome
 
