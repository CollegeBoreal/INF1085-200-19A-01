
# Comment configurer un serveur OpenVPN sur Debian 




## :one:  Installer OpenVPN et EasyRSA

:pushpin: Pour commencer installer les pacquet de notre serveur VPN ET OpenVPN:

$ sudo apt update
$ sudo apt install openvpn

*Emettre des certificats:VPN TLS / SSL

:pushpin: Pour ce faire, nous téléchargerons la dernière version de EasyRSA, que nous utiliserons pour construire notre infrastructure de clé publique CA (PKI), à partir du référentiel GitHub officiel du projet:


[🎥] wget -P ~/ https://github.com/OpenVPN/easy-rsa/releases/download/v3.0.4/EasyRSA-3.0.4.tgz

:pushpin: Apres telechargement il faut l'archiver:

$ cd ~
$ tar xvf EasyRSA-3.0.4.tgz
Apres installation de tout les logiciels passons a l'etape 2.

### :two: Configuration des variables EasyRSA et création de l'autorité de certification

:pushpin: Sur home acceder au repertoire EasyRSA :

$ cd ~/EasyRSA-3.0.4/
Sur ce repertoire acceder au fichier  nommé vars.example
Faites une copie de ce fichier et nommez-la vars sans extension de fichier:

$ cp vars.example vars

-Ouvrez ce nouveau fichier en utilisant votre éditeur de nano que moi je prefere:
$ nano vars


Recherchez les paramètres qui définissent les valeurs par défaut des champs pour les nouveaux certificats. Cela ressemblera à ceci:



:m: Décommentez ces lignes et mettez à jour les valeurs surlignées

ctrl O pour save et ctrl x pour exit

:m: Le répertoire EasyRSA contient un script appelé easyrsa (permet de a la creation et a la gestion des certification)
Exécutez ce script avec :


$ ./easyrsa init-pki

:m: Exécutez easyrsa avec "build-ca" qui va construire 'autorité de certification et créera deux fichiers importants - ca.crt( certificat public) et ca.key- qui constitueront les côtés public et privé( certificat prive) d'un certificat SSL.
vous pouvez aussi exécuter la build-cacommande avec l' nopassoption suivante:

$ ./easyrsa build-ca nopass


Dans la sortie, il vous sera demandé de confirmer le nom commun de votre autorité de certification:

appuyez sur ENTERpour accepter le nom par défaut

### :tree: Création des fichiers de certificat de serveur, de clé et de chiffrement

:m: Commencez par naviguer vers le répertoire EasyRSA sur votre serveur OpenVPN :
$ cd EasyRSA-3.0.4/

:pushpin: lancez le easyrsascript avec l' init-pki option 
./easyrsa init-pki
Appelez ensuite à easyrsanouveau le script, cette fois avec l' gen-reqoption suivie d'un nom commun pour la machine:


$./easyrsa gen-req server nopass

:pushpin:Cela créera une clé privée pour le serveur et un fichier de demande de certificat appelé server.req. Copiez la clé du serveur dans le /etc/openvpn/répertoire:


$ sudo cp ~/EasyRSA-3.0.4/pki/private/server.key /etc/openvpn/
