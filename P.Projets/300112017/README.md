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
 
Apache web server

#apt update
#apt install mariadb-server
#systemctl status mysql

          Hardening SQL
          
#mysql_secure_installation
$ mysql -u root -p

mysql> CREATE DATABASE wikidb;

mysql> CREATE USER 'mw-admin'@'localhost' IDENTIFIED BY 'mypassword';

mysql> GRANT ALL PRIVILEGES ON wikidb.* TO 'mw-admin'@'localhost'IDENTIFIED BY 'mypassword';

mysql> FLUSH PRIVILEGES;

mysql> exit

Installing PHP

#apt install php

#apt install libapache2-mod-php

#systemctl restart apache2

#nano /var/www/html/testmyphp.php
<?php
phpinfo();
?>

Installing and configuring MediaWiki

$ wget https://releases.wikimedia.org/mediawiki/1.30/\mediawiki-1.30.0.tar.gz

$ tar xzvf mediawiki-1.30.0.tar.gz
$ ls
mediawiki-1.30.0 mediawiki-1.30.0.tar.gz

#cp -r mediawiki-1.30.0/* /var/www/html/


$ sudo apt search mbstring
Sorting... Done
Full Text Search... Done
php-mbstring/xenial 1:7.0+35ubuntu6 all
MBSTRING module for PHP [default]
php-patchwork-utf8/xenial 1.3.0-1build1 all
UTF-8 strings handling for PHP
php7.0-mbstring/xenial-updates 7.0.18-0ubuntu0.16.04.1 amd64
MBSTRING module for PHP

et après

$ sudo apt install php7.0-mbstring php7.0-xml

$ sudo systemctl restart apache2

$ sudo apt install php-mysql php-apcu php-imagick

$ sudo systemctl restart apache2

Connecting MediaWiki to the database




