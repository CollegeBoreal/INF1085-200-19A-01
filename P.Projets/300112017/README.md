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
 




