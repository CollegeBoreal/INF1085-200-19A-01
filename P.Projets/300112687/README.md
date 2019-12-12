
 #  #SAMBA PROJET
 




 # :one: INSTALLATION
 
 ![image]( Sudo.png)
 
 ```
 -$ sudo apt-get intstall samba samba-common-bin
 ```
       
 
 # :two: Creation du repertoire et Modification du fichier 
           
      
      - $ mkdir /home/pi/shared
        
        
      - $ sudo nano /etc/samba/smb.conf
       
  Ajout du fichier dans la configuration nano afin de 
  permettre l'acces à different utilisateur,le fichier sera 
  ajouter à la fin de la page .
  
    - [pimylifeupshare]
     writeable=Yes
     create mask=0777
     directory mask=0777
     public=no
   
   ![image]( creation.png)
   ![image]( pimylifeupshare.png )
      
      
      
   # :three:SET PASSWORD
   
   ```
 $ sudo smbpasswd -a pi
 ```
    -$password:joseph
 
![image]( password.png)


# :4: CONNECT TO A SERVER

 ![image](Sc.png)
 
 
 # :5: PI connection
 
  ![image]( pi.png)


 # :six:SERVER

 ![image](Server.png)
 
 
