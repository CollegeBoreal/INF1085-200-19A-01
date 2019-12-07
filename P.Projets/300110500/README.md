
## :loop:   MES PREMIERS PAS SUR RASBERRY PI

Le Raspberry pi est un nano ordinateur de la taille d'une carte de crédit que l'on peut brancher à un écran et utilisé comme un ordinateur standard. Sa petite taille, et son prix intéressant fait du Raspberry pi un produit idéal pour tester différentes 
choses, et notamment la création d'un serveur Web chez soi. Évidemment, pour sa taille il ne faut pas s'attendre à des performances incroyables, mais pour mettre en ligne des projets à montrer au client ou expérimenter avec linux c.


### :pushpin: INSTALLATION DE MARIA DB ON YOUR RASBERRY PI

Avant toute installation, il convient de configurer votre raspberry pi en l'attribuant une adresse ip et un mot de passe.
Pour installer MARIA DB sur rasberry pi:
### :one: Ouvrir le terminal git bash et se connecter a votre rasberry pi  
ssh pi@10.13.237.75

### :two: faire une mise a jour de votre rasberry pi en entrant la commande:  
$ sudo apt-get update


### :three: installer Mari DB avec la commande suivante:

~#   apt install mariadb-server

### :four: creeation d'un utlisateur 

- CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
- GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

### :five: Acceder a la base de donnees par la commande suivante: 
$ mysql -u root -p

- creer des bases de donnees

. MariaDB> use companydb

MariaDB> CREATE TABLE Contacts (
ID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);

mysql> CREATE DATABASE wikidb;

- Insert statements
MariaDB> INSERT INTO Contacts (ID, LastName, FirstName, Address, City)
VALUES ('001', 'Torvalds', 'Linus', '123 Any St.', 'Newtown');


### :pushpin: INSTALLATION DE PHP ON YOR SERVER

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

##### Tester la connectivite de php
- Ouvrir un explorer et taper 10.13.237.75/testmyphp.php

- Configurer en ouvrant un explorer mediawiki 10.13.237.75/index.php

 -- configuring your wiki link: http://10.13.237.75/mw-config/index.php?page=Options
 
### :pushpin: INSTALLER MEDIAWIKI SUR RASBERRY PI

Le wiki le plus important est Wikipedia. C'est une immense encyclopédie (au même titre qu'une encyclopédie papier).
Elle possède des dizaines de milliers d'articles sur différents sujets (société, santé, mathématiques, informatique, littérature...) dans différentes langues (la majorité des articles sont en anglais, mais on en trouve aussi beaucoup en français, allemand, espagnole...)

- pour installer wiki, il faut excecuter la commande suivante dans le terminal git bash
$ wget https://releases.wikimedia.org/mediawiki/1.30/\
mediawiki-1.30.0.tar.gz


$ tar xzvf mediawiki-1.30.0.tar.gz
-
$ ls
mediawiki-1.30.0 mediawiki-1.30.0.tar.gz
# cp -r mediawiki-1.30.0/* /var/www/html/
