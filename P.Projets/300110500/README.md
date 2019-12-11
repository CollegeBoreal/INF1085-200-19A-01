
## :loop:   MES PREMIERS PAS SUR RASPBERRY PI

### Utilite de raspberry pi

Le Raspberry pi est un nano ordinateur que l'on peut brancher à un écran et utilisé comme un ordinateur standard. Il est utilise 
la creation d'un serveur Web chez soi. Pour sa pour sa taille il ne faut pas s'attendre a des performances incroyables, mais pour mettre en ligne des projets a montrer au client ou experimenter avec linux.

### :pushpin: ETAPES DE CONFIGURATION LAMP SUR RASPBERRY PI
LAMP= Linux, Apache, Mysql et PHP

### :pushpin: INSTALLATION LINUX
Rasppberry integre deja le systeme linux tout commentaire serait une perte de temps.
Il convient de configurer votre raspberry pi en l'attribuant une adresse ip et un mot de passe.
La commande pour configurer l'adress est la suivante: if config

### :pushpin: INSTALLATION DE MARIA DB ON YOUR RASPBERRY PI

Pour installer MARIA DB sur rasberry pi:

### :one: Ouvrir le terminal git bash et se connecter a votre rasberry pi  
ssh pi@10.13.237.75

### :two: faire une mise a jour de votre rasberry pi en entrant la commande:  
$ sudo apt-get update

### :three:installation preeliminaire

 $ apt install php 
 
 $ apt install libapache2-mod-php   

### :four: installer Mari DB 
Nous pouvons installer MariaDB avec la commande suivante:

~#   apt install mariadb-server

### Creation de la base des bases de donnes comanydb et wikidb

mysql> CREATE DATABASE wikidb;
mysql> CREATE DATABASE wikidb;

### :four: Creation d'un utlisateur et des privileges 
- utilisateur et des privileges pour la base de donnes companydb
mysql> CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

- utilisateur et des privileges pour la base de donnes wikibd
mysql> CREATE USER 'mw-admin'@'localhost' IDENTIFIED BY 'mypassword';
mysql> GRANT ALL PRIVILEGES ON wikidb.* TO 'mw-admin'@'localhost' IDENTIFIED BY 'mypassword';
mysql> FLUSH PRIVILEGES; 

### :five: Acceder a la base de donnees par la commande suivante: 

$ mysql -u root -p

### :six: creer des de tables pour companydb 

. MariaDB> use companydb puis inserrrer les donnnees ci-apres

MariaDB> CREATE TABLE Contacts (
ID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);

### :7:Insert statements dans companydb
MariaDB> INSERT INTO Contacts (ID, LastName, FirstName, Address, City)
VALUES ('001', 'Torvalds', 'Linus', '123 Any St.', 'Newtown');


### :pushpin: INSTALLATION DE PHP SUR RASBERRY PI

### :one: Enter la commande suivante dans git bash pour installer
 # apt install php
 ### Specifier la version version php a installer
 # apt install libapache2-mod-php
### :two Creer un nouveau fichier avec la commande ci-dessous:
# nano /var/www/html/testmyphp.php
### :three: Copier l'extension PHP dans via puis sauvegarder
<?php
phpinfo();
?>

#### :four: Comment Tester la connectivite de php

- Ouvrir un explorer et taper l'adresse suivante sur un explorer 

10.13.237.75/testmyphp.php

 - copier l'adress et configurer wiki sur un explorer: 
 
 10.13.237.75/index.php
 
http://10.13.237.75/index.php/Main_Page
 
### :pushpin: INSTALLER MEDIAWIKI SUR RASBERRY PI

Le wiki le plus important est Wikipedia. C'est une immense encyclopedie (au meme titre qu'une encyclopédie papier).
Elle possede des dizaines de milliers d'articles sur différents sujets (societe, sante, mathematiques, informatique, litterature...) dans differentes langues (la majorite des articles sont en anglais, mais on en trouve aussi beaucoup en français, allemand, espagnole...)

- pour installer wiki, il faut excecuter la commande suivante dans le terminal git bash

$ wget https://releases.wikimedia.org/mediawiki/1.30/\
mediawiki-1.30.0.tar.gz      

Pour cree un nouveau repertoire contenant tous les fichiers et repertoires en copiant toute cette hierarchie de repertoires a l'emplacement du système de fichiers ou il fera son travail.
Pour mon cas MediaWiki va etre la seule application Web hebergee sur notre rasberry pi, ce qui  signifie qu'il est notre repertoire racine Web. Pour creer un nouveau repertoire il excecuter la commande suivante:

 .$ tar xzvf mediawiki-1.30.0.tar.gz

Une fois que l'archive a ete cree excecuter la commande ci desoous pour s'assurer de sa creation. 

$ ls

mediawiki-1.30.0 mediawiki-1.30.0.tar.gz

 vous souhaiterez peut-etre creer un sous-repertoire à la racine du document qui exposera le service de maniere
 pratique et previsible. ceci place les fichiers dans un repertoire appele / var / www / html / mediawiki /. La commande ci dessous 

~# cp -r mediawiki-1.30.0/* /var/www/html

Je vais installer les deux packages et utiliser systemctl pour redémarrer Apache:

~#  apt install php7.0-mbstring php7.0-xml

~# systemctl restart apache2




