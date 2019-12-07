# Configuration Client VPN

#### :apple: Connection du mac sur un serveur VPN

:zero: Installer l’application [Tunnelblick](https://tunnelblick.net/) <image src ="https://tunnelblick.net/common/tb-icon-64x64.v1.png" width="16" height="16"></image> pour Mac

```
$ brew cask install tunnelblick
```


:a: Installer les cles


* Avec Docker Machine: 

```
$ docker-machine scp isaha:Desktop/client.zip .
```

* Avec scp:

```
$ scp pi@192.168.1.10:Desktop/client.zip .
```
:one: dans un répertoire i.e. `Desktop/client` sur le mac, observer
* les clés `client.{key,crt,csr}`, 
* la clé `ta.key`,
* le certificat `ca.crt` et 
* le fichier de configuration `client.conf` 

<image src ="images/image001.png" width="561" height="228"></image>

:two: Faire un `drag-and-drop` du fichier `client.conf` sur l'icône <image src ="https://tunnelblick.net/common/tb-icon-64x64.v1.png" width="16" height="16"></image> de `Tunnelblick` dans la barre de menu

<image src ="images/image002.png" width="423" height="251"></image>

:three: Choisir la configuration pour `Moi seulement`

<image src ="images/image003.png" width="405" height="207"></image>

:four: Après plusieurs messages d’alertes insignifiants

<image src ="images/image004.png" width="255" height="375"></image>

:five: La configuration `client` est installée

<image src ="images/image005.png" width="421" height="235"></image>

:six: Appuyer sur le bouton `Connecter` et la connexion est établie

<image src ="images/image006.png" width="266" height="195"></image>

:seven: Pour déconnecter, Sélectionner `Tunnelblick` <image src ="https://tunnelblick.net/common/tb-icon-64x64.v1.png" width="16" height="16"></image> dans la barre de menu.

<image src ="images/image007.png" width="530" height="150"></image>

:ab: Verifier dans le terminal

```
$ ifconfig
[...]
utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
	inet 10.8.0.6 --> 10.8.0.5 netmask 0xffffffff 
```


