La configuration et installation de SAMBA

Samba est une solution open source qui permet aux utilisateurs de configurer des partages de fichiers et d'impression rapides et sécurisés. Dans cet article, je couvrirai comment configurer Samba avec le stockage en bloc de Vultr sur Debian 9. Cela inclut les quotas facultatifs, l'authentification et les instructions pour y accéder via votre connexion à domicile.
sudo apt update
Les commandes:

sudo apt-get install samba
verification de reussite:
ou est samba
The following should be its output:

samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
3️⃣ Configuring Samba
Now that Samba is installed, you need to create a directory for it to share:

Before that, make sure you are connecting on root, by typing:

sudo -i
Create the share folder:

# mkdir -p /samba/sharehome
# touch /samba/sharehome/myfile
# chmod 777 /samba/sharehome


La commande ci-dessus 👆 crée un nouveau dossier "partage de fichiers" dans votre répertoire personnel que vous partagerez plus tard.

Le fichier de configuration de Samba se trouve dans /etc/samba/smb.conf Pour ajouter le nouveau répertoire en tant que partage, vous modifiez le fichier en exécutant:

# nano /etc/samba/smb.conf
définissez votre dossier de partage en ajoutant les lignes suivantes:

[sharehome]                                --> name of your share file/ optional
    comment = Samba on Ubuntu              --> A brief description of the share/ optional
    path = /samba/sharehome                --> where you create your share file/ The directory of your share.
    writable = yes                         --> Permission to modify the contents of the share folder
Then press Ctrl-O to save and Ctrl-X to exit from the nano text editor.

maintenantSamba est configurer:

# systemctl start smbd
# systemctl enable smbd
📌 Setting up User A

4️⃣ creer un compte SAMBA
Samba n'utilisant pas le mot de passe du compte système, nous devons configurer un mot de passe Samba pour notre compte utilisateur:

# adduser sambauser
# smbpasswd -a sambauser
📌 Testing your Samba configuration
5️⃣ test your configuration
to show the section you added run the following command:

# testparm
pour basculer l'utilisateur (su) vers le compte sambauser que vous avez associé à Samba que vous exécutez:

$ su sambauser
Installation de samba client avec ses commandes:

sudo apt-get install smbclient
and then run this command to log in to your Samba share from the local machine :

# smbclient //localhost/sharehome
je vous envoie le password et le user name par email.
