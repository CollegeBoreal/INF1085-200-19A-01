## Présentation du VPN

Le réseau privé virtuel est un type de réseau privé qui utilise les télécommunications publiques, comme Internet, au lieu des lignes louées pour communiquer.
Devenu populaire au fur et à mesure que plus d'employés travaillaient dans les régions éloignées.

## Un bref aperçu de son fonctionnement

- Deux connexions - l'une à Internet et l'autre au VPN.
- Datagrammes - contient les données, la destination et l'adresse et les informations sur la source.
- Pare-feu - Les VPN permettent aux utilisateurs autorisés de passer à travers les pare-feu.
- Protocoles - les protocoles créent des tunnels VPN

## Comment installer !

Avant de débuter l’installation avec OpenVPN en elle-même, il est recommandé de rechercher et d’installer sur votre Raspberry Pi toutes les mises à jour disponibles : pour ce faire, il suffit d’entrer les commandes suivantes sur la console :
```
sudo apt-get update

sudo apt-get upgrade
```
Si vous n’avez pas encore changé les identifiants par défaut de votre Raspberry Pi (nom d’utilisateur « Pi » et mot de passe « Raspberry »), il est temps de le faire, sinon n’importe qui pourra accéder à votre système, localement ou sur le réseau via SSH. Grâce à la commande ci-dessous, vous accéderez à la configuration de votre nano-ordinateur, et pourrez choisir un mot de passe sécurisé.
```
sudo raspi-config
```
Installer OpenVPN et mettre en place des fichiers easy-rsa

Commencez par utiliser la commande ci-dessous pour installer le programme OpenVPN et OpenSSL, qui permet d’encrypter la connexion Internet :
```
sudo apt-get install openvpn openssl
```
Après avoir installé OpenVPN, copiez les scripts tout prêts « easy-rsa » dans le fichier de configuration OpenVPN.
```
sudo cp -r /usr/share/easy-rsa /etc/openvpn/easy-rsa
```
Ouvrez ensuite le fichier suivant dans la console : « /etc/openvpn/easy-rsa/vars », et entrez-y la commande ci-dessous :
```
sudo nano /etc/openvpn/easy-rsa/vars
```
Il s’agit maintenant d’ajuster ce fichier. Vous pouvez modifier les paramètres en remplaçant l’intégralité de la ligne « export EASY_RSA="`pwd` » par la commande suivante :
```
export EASY_RSA="/etc/openvpn/easy-rsa"
```
La longueur de la clé peut également être ajustée, ce qui détermine le niveau de sécurité du cryptage.

Ensuite, retournez dans le dossier de configuration « easy-rsa », accordez les droits root et intégrez les paramètres réglés précédemment dans les variables de l’environnement en exécutant le script « vars » grâce à la commande « source ». Enfin, vous pouvez rendre le fichier de configuration obtenu accessible via un lien symbolique intitulé « openssl.cnf ».
```
cd /etc/openvpn/easy-rsa
sudo su
source vars
ln -s openssl-1.0.0.cnf openssl.cnf
```
Certificats et clés pour installer OpenVPN

Commencez par réinitialiser les clés et créez ensuite les premiers fichiers de clés pour OpenVPN.
```
./clean-all
./build-ca OpenVPN
```
Le programme vous demande ensuite de rentrer les deux lettres correspondant à votre « nom de pays » : entrez CN pour le Canada. Les champs suivants ne sont pas essentiels, et vous pouvez directement confirmer en pressant la touche « entrée ».

Ensuite, voici comment générer les fichiers clés pour le serveur :
```
./build-key-server server
```
Entrez à nouveau le code à deux lettres correspondant au pays, et laissez les champs suivants vides. Enfin, confirmez la requête, et générez le certificat en appuyant deux fois sur la touche « Y ».

La section suivante concerne l’installation d’un ou plusieurs clients VPN. Il faut créer un certificat et une clé pour chaque appareil auquel vous souhaitez accéder depuis le serveur VPN. Le processus est similaire à celui utilisé pour installer le certificat et la clé sur le serveur (entrer le code pays et le confirmer deux fois). Vous pouvez attribuer un nom spécifique à chacun des appareils : ceci permet ensuite de créer un client pour un « laptop », une « tablet » ou un « smartphone », grâce à la commande ci-dessous :
```
./build-key laptop
./build-key smartphone
./build-key tablet
…
```
Si vous souhaitez attribuer un mot de passe aux clients, il faudra remplacer la commande ci-dessus par celle-ci :
```
./build-key-pass laptop
./build-key-pass smartphone
./build-key-pass tablet
…
```
Voici ensuite comment compléter la génération du certificat grâce à la commande pour l’échange de clés Diffie-Hellman :
```
./build-dh
```
Cette étape peut prendre du temps. Une fois que le processus est complété, fermez votre session d’utilisateur root :
```
exit
```
Générer les fichiers de configuration pour le serveur OpenVPN

Ces fichiers sont générés grâce à la commande suivante :
```
sudo nano /etc/openvpn/openvpn.conf
```
Le fichier vide contient désormais plusieurs commandes, expliquées ci-dessous. En premier lieu, activez le routage par tunnel IP via « dev tun », et sélectionnez le protocole de transport UDP, en sélectionnant « proto udp » Si vous souhaitez utiliser le protocole TCP, sélectionnez « proto tcp »). La ligne suivante vous permet de déterminer si le serveur OpenVNP est accessible sur port 1194, mais ce paramètre peut être modifié.
```
dev tun
proto udp
port 1194
```
Ensuite, créez un certificat root (ca) SSL/TLS, un certificat digital (cert) et une clé digitale (key) grâce au fichier « easy-rsa ». Veillez à entrer le bon cryptage de bits (1024, 2048 etc.).
```
ca /etc/openvpn/easy-rsa/keys/ca.crt
cert /etc/openvpn/easy-rsa/keys/server.crt
key /etc/openvpn/easy-rsa/keys/server.key
dh /etc/openvpn/easy-rsa/keys/dh2048.pem
```
C’est maintenant le moment de préciser que le Raspberry Pi est utilisé en tant que serveur. Pour ce faire, il faut nommer l’adresse IP et le masque réseau de façon à ce qu’ils soient attribués au VPN.
```
server 10.8.0.0 255.255.255.0
```
La commande « redirect-gateway def1 bypass-dhcp » permet de diriger l’intégralité du trafic IP vers le tunnel IP. Si vous avez des exigences élevées en ce qui concerne le niveau de sécurité, vous pouvez les expérimenter lors du paramétrage ; si toutefois ceci cause des problèmes ou ralentit la navigation, il est recommandé d’annuler cette configuration. Les instructions ci-dessous, en revanche, devront être utilisées dans tous les cas, car c’est grâce à elles que vous pourrez renommer les serveurs publics DNS avec lesquels fonctionnera votre serveur VPN. Dans la commande suivante, un serveur de 1&1 IONOS est désigné par « 217.237.150.188 », et un serveur de Google par « 8.8.8.8 ». Vous pouvez toutefois modifier ces codes comme vous le souhaitez en spécifiant les adresses IPv4 d’autres serveurs DNS. Dans la commande « log-append /var/log/openvpn », assurez-vous que l’information de connexion est écrite dans le fichier « /var/log/openvpn ».
```
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 217.237.150.188"
push "dhcp-option DNS 8.8.8.8"
log-append /var/log/openvpn
```
Avec « persist-key », les fichiers clés ne sont pas lus à nouveau, et les pilotes des réseaux TAP et TUN ne sont pas redémarrés. Après le démarrage d’un programme, les droits du démon OpenVPN sont restreints par « user nobody » et « group nogroup ». Avec « status /var/log/openvpn-status.log », vous créez un fichier de statut qui rapporte l’état de votre connexion. Il est également recommandé de concilier la précision des informations de connexion grâce à la commande « verb ». Si vous choisissez « 0 », vous ne recevrez aucune notification, à l’exception des messages d’erreur. Une valeur comprise entre 1 et 4 est adaptée à une utilisation normale, et des valeurs supérieures sont à utiliser en cas de résolution de problèmes. Il s’agit ensuite de paramétrer la commande « client à client », qui non seulement détecte le serveur, mais également les autres clients VPN, et d’activer la compression LZO avec « comp-lzo » (cet élément doit également être activé dans le fichier de configuration du client).
```
persist-key
persist-tun
user nobody
group nogroup
status /var/log/openvpn-status.log
verb 3
client-to-client
comp-lzo
```
Sauvegardez les modifications avec « Ctrl + O » et sortez de l’éditeur avec « Ctrl + X ».
Créer un script pour un accès Internet avec un client

Pour accéder à votre réseau local grâce à un tunnel VPN, il faut créer une redirection. Il convient pour cela de créer d’abord un fichier « /etc/init.d/rpivpn » :
```
Sudo nano /etc/init.d/rpivpn
```
Dans ce fichier, copiez les commentaires suivants afin de créer un titre pour le script Init Linux :
```
#! /bin/sh
### BEGIN INIT INFO
# Provides:          rpivpn
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: VPN initialization script
### END INIT INFO
```
Ensuite, activez « ip forward » en écrivant « 1 » dans ce dossier :
```
echo 'echo "1" > /proc/sys/net/ipv4/ip_forward' | sudo -s
```
Puis créez une redirection pour les paquets VNP en utilisant le filtre de paquets « iptables ».
```
iptables -A INPUT -i tun+ -j ACCEPT
iptables -A FORWARD -i tun+ -j ACCEPT
```
Il s’agit ensuite de paramétrer les commandes qui autorisent vos clients VNP à accéder au LAN et à Internet. Voici comment procéder :
```
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -F POSTROUTING
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
```
Enregistrez et fermez à nouveau le fichier avec « Ctrl + O » et « Ctrl + X ».

Pour que la redirection soit effective, il faut ensuite assigner les droits adaptés au script, et l’installer en tant que script Init.
```
sudo chmod +x /etc/init.d/rpivpn
sudo update-rc.d rpivpn defaults
```
Ensuite, exécutez le script et redémarrez le serveur VPN.
```
sudo /etc/init.d/rpivpn
sudo /etc/init.d/openvpn restart
```
Terminer l’installation des clients

La dernière étape consiste à regrouper les certificats et les clés de chaque client dans des paquets respectifs. Pour ce faire, il faut leur accorder les droits root en ouvrant le dossier « /etc/openvpn/easy-rsa/keys/ », créer le fichier de configuration client, et entrer les commandes suivantes dans le fichier « laptop ». La configuration est identique pour chaque client, il suffit de changer le nom de l’appareil dont il est question.
```
sudo su
cd /etc/openvpn/easy-rsa/keys
nano laptop.ovpn
```
Dans le fichier client « opvn », entrez les commandes suivantes :
```
dev tun
client
proto udp
remote x.x.x.x 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert laptop.crt
key laptop.key
comp-lzo
verb 3
```
Il faut toutefois ajuster le contenu des fichiers ci-dessus. À la quatrième ligne, remplacez « x.x.x.x » par l’adresse IP de votre fournisseur DDNS (si vous utilisez une adresse publique statique, vous pouvez simplement l’insérer ici), suivie par le port grâce auquel le serveur VPN doit être accessible. Aux troisième et quatrième rangs, entrez le nom de votre client (ici, « laptop »). Après avoir inséré ces modifications, sauvegardez-les avec « Ctrl + O », puis fermez l’éditeur avec « Ctrl + X ».

Enfin, copiez le fichier de configuration avec les certificats et les clés dans un fichier zip. Si vous n’avez pas encore installé de pack zip sur votre Raspberry Pi, vous pouvez le faire grâce à la commande suivante :
```
apt-get install zip
```
Pour créer un fichier zip, utilisez les commandes ci-dessous en vous assurant que chaque ligne comprend le bon nom de client.
```
zip /home/pi/raspberry_laptop.zip ca.crt laptop.crt laptop.key laptop.ovpn
```
Il faut ensuite ajuster les droits des fichiers et terminer l’installation par « exit ».
```
chown pi:pi /home/pi/raspberry_laptop.zip
exit
```
