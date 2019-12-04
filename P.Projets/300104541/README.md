# SECURING NETWORK CONNECTIONS

## 1.Introduction VPN

VPN en anglais "Virtual Private Network" (réseau privé virtuel) désigne un réseau crypté dans le réseau Internet, qui permet à une société dont les locaux seraient géographiquement dispersés de communiquer et partager des documents de manière complètement sécurisée, comme s'il n'y avait qu'un local avec un réseau interne.D'une maniere terre a terre il permet d'échanger des informations de manière sécurisée et anonyme en utilisant une adresse IP différente a celle de votre ordinateur.

### 2.Notre but 
- Créer un tunnel de réseau privé virtuel (VPN) permettant des connexions distantes sécurisées et invisibles.
- Concevoir des architectures de pare-feu plus sophistiquées pour diviser votre réseau de manière stratégique en segments isolés.
- Créer un environnement de réseau virtuel afin de pouvoir tester vos configurations.

## Construire un tunnel VPN

L'installation VPN sur votre serveur nécessite deux packages: openvpn et, pour gérer le processus de génération de clé de cryptage, easy-rsa. Si nécessaire, les utilisateurs de CentOS doivent d’abord installer le référentiel epel-release. Pour vous permettre de tester facilement l’accès à une application serveur, vous pouvez également installer le serveur Web Apache (apache2 pour Ubuntu et httpd sur CentOS). Pendant que vous configurez votre serveur, vous pouvez également le faire correctement et activer un pare-feu qui bloque tous les ports autres que 22 (SSH) et 1194 (le port OpenVPN par défaut). Cet exemple illustre la manière dont cela fonctionnera sur le ufw d’Ubuntu




