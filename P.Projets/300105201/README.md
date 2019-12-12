## How to Setup a Raspberry Pi Samba Server

``` 
Raspberry user: pi

Password: Password 

Ip adress: 10.13.237.78

```

1- La première chose que nous devons faire avant de configurer un partage SMB / CIFS sur notre Raspberry Pi est de nous assurer que tout est à jour.
Pour les mettre a jour on utilise ces deux commandes :

```
 ~ # sudo apt-get update
```
```
 ~ # sudo apt-get upgrade
 ```

2- Maintenant que notre système est à jour, on peut procéder à l'installation du logiciel Samba sur notre Raspberry Pi.
Passons a l'installation des packages dont nous avons besoin pour configurer Samba en exécutant la commande suivante:
```
 ~ # sudo apt-get install samba samba-common-bin
```

3- Avant de configurer notre stockage réseau sur notre Pi, nous devons d'abord créer un dossier que nous partagerons.
Ce dossier peut être situé n'importe où, y compris sur un disque dur externe monté. Pour ce didacticiel, nous allons créer le répertoire dans le répertoire personnel des utilisateurs «pi».
Créez ce dossier en exécutant la commande suivante :

```
~ # mkdir /home/pi/shared
```
4- Nous pouvons maintenant partager ce dossier à l'aide du logiciel Samba. Pour ce faire, nous devons modifier le fichier de configuration samba.
Le fichier de configuration «smb.conf» est l'endroit où vous stockerez tous vos paramètres pour vos partages.
Nous pouvons commencer à modifier le fichier de configuration en exécutant la commande ci-dessous.
```
~ # sudo nano /etc/samba/smb.conf
```
dans ce meme fichier, on va ajouter ces codes :
```
[pimylifeupshare]
path = /home/pi/shared
writeable=Yes
create mask=0777
directory mask=0777
public=no
```
Avant de poursuivre, je vais vous exoliquez La signifiaction de ces codes :
[pimylifeupshare] : Ceci définit le partage lui-même, le code entre crochets est le point auquel vous accéderez au partage. Par exemple, la nôtre sera à l'adresse suivante: // raspberrypi / pimylifeupshare

path = /home/pi/shared : Cette option est le chemin vers le répertoire de votre Raspberry Pi que vous souhaitez partager.

writeable=Yes :  Lorsque cette option est définie sur «Oui», cela permettra au dossier d'être inscriptible.

create mask=0777 and directory mask=0777 : Cette option définit les autorisations maximales pour les fichiers et les dossiers. La définition de 0777 permet aux utilisateurs de lire, d'écrire et d'exécuter.

public=no : Si ce paramètre est réglé sur «non», le Pi aura besoin d'un utilisateur valide pour accorder l'accès aux dossiers partagés.

5-  Ensuite, nous devons configurer un utilisateur pour notre partage Samba sur le Raspberry Pi. Sans cela, nous ne pourrons pas établir de connexion avec le lecteur réseau partagé.

Nous allons créer un utilisateur Samba appelé «pi».
Exécutez la commande suivante pour créer l'utilisateur. Vous serez ensuite invité à saisir le mot de passe.

```
 ~ # sudo smbpasswd -a pi
```
Ensuite, avant de nous connecter à notre Raspberry Pi Samba, nous devons redémarrer le service samba afin qu'il se charge dans nos modifications de configuration.
```
 ~ #sudo systemctl restart smbd
```
 La dernière chose que nous devons faire avant d'essayer de nous connecter à notre partage Samba est de récupérer l'adresse IP locale de notre Raspberry Pi.

Tout d'abord, assurez-vous que vous êtes connecté à un réseau en connectant un câble Ethernet ou en configurant le WiFi.

Bien que vous puissiez vous connecter en utilisant le nom de réseau du Pi, nous récupérerons l'adresse IP au cas où cette option ne fonctionnerait pas sur votre réseau domestique.

Exécutez la commande ci-dessous pour imprimer l'adresse IP locale du Pi.
```
 ~ # hostname -I
```
Pour finir, lancer les commandes suivantes pour lancer Samba a partir de Systemctl :
```
 ~ # systemctl start smbd
```
```
~ # systemctl enable smbd
```
Pour vous connecter à votre Samba sous Windows, commencez par ouvrir l'Explorateur de fichiers.

Dans l'Explorateur de fichiers, cliquez sur l'onglet Ordinateur
Saisissez le nom d'utilisateur et le mot de passe que vous avez définis à l'aide de l'outil «smbpasswd» plus haut dans le didacticiel.

Une fois terminé, appuyez sur le bouton «OK» pour continuer.

            
