                                               🔎 Web servers 
                                       Building  a MediaWiki server ( Chap 7 )
                                       
   😊 MediaWiki est un logiciel libre orienté serveur, disponible sous licence Licence publique générale GNU (GNU General Public License - GPL). Il est conçu pour fonctionner dans une ferme composée de nombreux serveurs, hébergeant un site web pouvant avoir plusieurs millions de clics par jour.
MediaWiki est un logiciel extrêmement puissant, adaptable à souhait et permettant une implémentation de wiki aux fonctionnalités riches. Il utilise PHP pour interpréter et afficher les données contenues dans une base de données de type MySQL notamment.
Les pages utilisent le format wikitexte de MediaWiki ; ainsi les utilisateurs peuvent contribuer facilement sans avoir de connaissance en HTML ou CSS.

                                              🎾 Building a LAMP server
                                              Statique ou Dinamique SiteWeb?
😀 LAMP est un acronyme pour Linux, Apache, MySQL, PHP. C'est une pile logicielle comprenant le système d'exploitation,
un serveur HTTP, un système de gestion de bases de données et un langage de programmation interprété, et qui permet de mettre en place un serveur web.
   Le serveur LAMP est une configuration Linux tellement courante qu'Ubuntu, au moins, a son propre métapaquet d'installation.Le signe d'insertion (^) à la fin de cet exemple identifie la cible comme un paquet spécial regroupé pour simplifier l'installation de piles logicielles communes : 
   
   $ sudo apt install lamp-server^
   
   Cette commande, après vous avoir demandé de créer un mot de passe de base de données, déposera automatiquement un serveur Web fonctionnel sur votre système, vous laissant avec rien d'autre à faire que de créer du contenu de site Web. Diriger votre navigateur 
   web vers l'adresse IP du serveur devrait afficher une page de bienvenue créée lors de l'installation d'Apache.
   
                                                         🙆‍♂️ Remarque
   
   Mais l'automatisation n'est pas toujours la meilleure solution.
    Parfois,vous voudrez personnaliser votre pile de logiciels en spécifiant des versions de versions particulières 
    pour assurer la compatibilité des applications,ou en substituant 
    un paquet à un autre (MariaDB sur MySQL, par exemple,comme vous allez bientôt le voir).
    La configuration manuelle sera particulièrement utile dans ce cas,car elle vous forcera à mieux comprendre 
    comment chaque bit fonctionne. 
    
    C'est l'approche que je vais adopter dans ce chapitre.
    Je vous montre la configuration manuellement.Vous avez le choix,soite vous pouvez l'installer 
    sur une machine physique ou sur une machine virtuelle(CB-DEV).
                                   
                                  
                                  
                                  👇 Voici les commandes SSH

    
    
   
   👍 Machine Physique:

$ ssh name@10.13.237.10.X 
     
     👍 Machine virtuelle (CB-DEV)

$ docker-machine ssh CB-DEV

     👍 Voici la commande Pour savoir le nom ou l'adresse IP de votre machine virtuellE

$ docker-machine ls
  
      Steps By Steps:
      
    😍 Voici une liste de ce qu'il faut faire pour atteindre votre objectif :
    
 1️⃣ Installez Apache
 
 2️⃣ Installez le langage de script PHP(7.0.0) côté serveur.

 3️⃣ Installez un moteur SQL (mysql dans ce cas)
 
 4️⃣ Installer et configurer MediaWiki
 
                                      1️⃣   Etape1: Install Apache2

Le serveur HTTP open source Apache tend à dominer le marché des serveurs Web sur toutes les plateformes. Parce qu’il est si populaire, et malgré le fait qu'Apache a de sérieux concurrents, dont Nginx (également multiplate-forme) et le IIS (qui fonctionne exclusivement sur les serveurs Windows). Alors, O Commence!
$ sudo apt update
$ sudo apt install apache2

L'URL que vous utiliserez pour accéder à un site Apache fonctionnant sur votre worksta-tion est localhost. Si, à la place, vous avez choisi de travailler sur un conteneur LXC ou une Virtual-Box VM, alors vous utiliserez l'adresse IP de la machine pour l'URL. Pour vous assurer d'avoir un accès réseau aux sites fonctionnant sur votre VirtualBox VM, assurez-vous qu'elle est configurée pour utiliser un adaptateur ponté (comme vous l'avez fait au chapitre 2).

                                              2️⃣   Etape2 : Install PHP
                                              
 L'ingrédient final de LAMP est le langage de script PHP. PHP est un outil qui peut être utilisé pour écrire vos propres applications web. Les applications PHP pré-intégrées sont souvent utilisées par des applications tierces comme MediaWiki pour accéder et traiter les ressources système. On peut donc supposer que vous aurez besoin du P dans votre serveur LAMP.
 $ sudo apt install php
 $ sudo apt install libapache2-mod-php
 
 Vous devriez prendre l'habitude de redémarrer Apache chaque fois que vous apportez des modifications à la configuration système d'un serveur web. Voici comment faire :
 
 vous pouvez changer aussi le fichier mais il faust recommencer Apache2
# systemctl restart apache2
Testing your PHP installation

# nano /var/www/html/testmyphp.php
<?php
phpinfo();
?>
                                 
                                 3️⃣   Etape3 : Install Mysql

A cette etape Vous devez configurer le mot de passe de votre conteneur mysql

$ sudo apt install mysql-server

Accédez à votre conteneur

$ mysql -u root -p
mysql> CREATE DATABASE wikidb;
mysql> CREATE USER 'mw-admin'@'localhost' IDENTIFIED BY 'mypassword';
mysql> GRANT ALL PRIVILEGES ON wikidb.* TO 'mw-admin'@'localhost'
                         IDENTIFIED BY 'mypassword';
mysql> FLUSH PRIVILEGES;
mysql> exit


 NOTE Si votre machine ne vous donne pas le droit de configurer le mot de passe dont vous avez besoin pour exécuter cette commande afin de configurer manuellement:



 




