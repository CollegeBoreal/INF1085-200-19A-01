# 2 Keys

:one: Authoriser l'acces à sa machine au professeur

* Connectez vous à votre machine avec `git bash` ou `putty`

```
$ ssh monID@monAdresseIP
```

* Ouvrir le fichier d'authorisation de clés publiques suivant avec Nano 

```
$ nano ~/.ssh/authorized_keys
```

* copier la clé publique ci-dessous dans une nouvelle ligne 

(attention copier la clé tel quel sans espace ou autre charactere, i.e. la clé est longue)

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD2pLhMqFGKffSdYvNCMAyM7598oBY+m/3q5AMXmb7IE6vq42+yGzqEUzZu9WrFckFD4Hq52rIU5DeOvi83DCF3uroXjNTEtCKdi+tY7cV18bHmsDsBHMqTnpuvroofgFWA0Pi++b2kGW2I5eyy1Qjv5rOp7y11Xe6XeZFEz7qQO1/xNiBMJEruG9Xldgooe4hkaOF39qnbqD4ui3LxYaTUTEulstw4wN70dSB8Zu9YQP7A7KU2zIEwJ1aw8whfO1CAM/AVvoDyqMtV8VXoaZSHOBgluMtinQfyyt473S2ZZeJlnmhK0F1gdOhO4SVZNRMj96m30ryYkYBFWvvLRP5N b300098957@ramena
```

## Identifiant

:two: Renseigner son utilisateur dans le tableau ci-dessous (i.e. MONUSER@10.13.237.126 => brice@10.13.237.126)

|:hash:| :id:      | Utilisateur à remplacer      |
|------|-----------|------------------------------|
| 01   | 300104524 | MONUSER@10.13.237.19         |
| 02   | 300104541 | MONUSER@10.13.237.:x:        |
| 03   | 300105201 | MONUSER@10.13.237.78         |
| 04   | 300106918 | MONUSER@10.13.237.18         |
| 05   | 300107361 |  toch90@10.13.237.99         |
| 06   | 300108234 | MONUSER@10.13.237.55         |
| 07   | 300110500 | MONUSER@10.13.237.75         |
| 08   | 300110529 | MONUSER@10.13.237.80         |
| 09   | 300111671 | MONUSER@10.13.237.63         |
| 10   | 300111766 | MONUSER@10.13.237.66         |
| 11   | 300112017 | orden@10.13.237.60           |
| 12   | 300112687 | nsomwe@10.13.237.87          |
| 13   | 300112917 | djumaster@10.13.237.79       |
| 14   | 300113775 | widby@10.13.237.77           |

:three: Installer Docker Engine sur sa machine Linux

Suivre le tutoriel suivant

https://github.com/CollegeBoreal/Tutoriels/tree/master/2.Cloud-Native/2.Docker/Linux


