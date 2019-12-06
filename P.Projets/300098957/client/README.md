# Configuration Client

J’ai pu connecter mon mac sur ton serveur VPN
 
:one: dans un répertoire `amelie-vpn` sur mon mac, j’ai copié 
* les clés `client.{key,crt,csr}`, 
* le certificat `ca.crt` et 
* le fichier de configuration `client.conf` 

<image src ="images/image001.png" width="561" height="228"></image>

:two: J’ai téléchargé l’application `tunnelblick` pour Mac https://tunnelblick.net/

```
$ brew cask install tunnelblick
```
:three: J’ai ensuite fait un `drag-and-drop` du fichier `client.conf` dans le paneau configuration de tunnelblick

<image src ="images/image002.png" width="423" height="251"></image>

:four: J’ai cliqué la configuration pour Moi seulement

<image src ="images/image003.png" width="405" height="207"></image>


<image src ="images/image004.png" width="255" height="375"></image>

