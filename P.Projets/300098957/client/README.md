# Configuration Client

#### :apple: Connection du mac sur un serveur VPN

:zero: Installer l’application `tunnelblick` pour Mac https://tunnelblick.net/

```
$ brew cask install tunnelblick
```

:one: dans un répertoire i.e. `amelie-vpn` sur le mac, copier
* les clés `client.{key,crt,csr}`, 
* le certificat `ca.crt` et 
* le fichier de configuration `client.conf` 

<image src ="images/image001.png" width="561" height="228"></image>

:two: Faire un `drag-and-drop` du fichier `client.conf` dans le paneau configuration de `TunnelBlick`

<image src ="images/image002.png" width="423" height="251"></image>

:three: Choisir la configuration pour `Moi seulement`

<image src ="images/image003.png" width="405" height="207"></image>

:four: Après plusieurs messages d’alertes insignifiants

<image src ="images/image004.png" width="255" height="375"></image>

:five: La configuration `client` est installée

<image src ="images/image005.png" width="421" height="235"></image>

:six: Appuyer sur le bouton `Connecter` et la connexion est établie

<image src ="images/image006.png" width="266" height="195"></image>
