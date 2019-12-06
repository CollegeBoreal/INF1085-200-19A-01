
## :loop:   MES PREMIERS PAS SUR RASBERRY PI
Le wiki le plus important est Wikipedia. C'est une immense encyclopédie (au même titre qu'une encyclopédie papier).
Elle possède des dizaines de milliers d'articles sur différents sujets (société, santé, mathématiques, informatique, littérature...) dans différentes langues (la majorité des articles sont en anglais, mais on en trouve aussi beaucoup en français, allemand, espagnole...)

### :pushpin: INSTALLATION DE MARIA DB ON YOUR RASBERRY PI

Avant toute installation, il convient de configurer votre raspberry pi en l'attribuant une adresse ip et un mot de passe.
Pour installer MARIA DB sur rasberry pi:
### :one: Ouvrir le terminal git bash et se connecter a votre rasberry pi  ssh pi@10.13.237.75
### :two: faire une mise a jour de votre rasberry pi en entrant la commande:  $ sudo apt-get update


- installer Mari DB avec la commande suivante:  # apt install mariadb-server

-creeation d'un utlisateur
 DROP USER 'root'@'localhost';
CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
DROP USER 'root'@'localhost';
CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

- Acceder a la base de donnees par la commande suivante:  $ mysql -u root -p
-creer des bases de donnees
MariaDB> use companydb
MariaDB> CREATE TABLE Contacts (
ID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);

mysql> CREATE DATABASE wikidb;

MariaDB> INSERT INTO Contacts (ID, LastName, FirstName, Address, City)
VALUES ('001', 'Torvalds', 'Linus', '123 Any St.', 'Newtown');


### :pushpin: INSTALLATION DE PHP ON YOR SERVER

 # apt install php
# apt install libapache2-mod-php

# nano /var/www/html/testmyphp.php
<?php
phpinfo();
?>

##### Ouvrir un explorateur et et taper
10.13.237.75/testmyphp.php


$ wget https://releases.wikimedia.org/mediawiki/1.30/\
mediawiki-1.30.0.tar.gz


$ tar xzvf mediawiki-1.30.0.tar.gz
$ ls
mediawiki-1.30.0 mediawiki-1.30.0.tar.gz
# cp -r mediawiki-1.30.0/* /var/www/html/
