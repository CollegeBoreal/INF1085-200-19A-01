# Installation d'un Serveur VPN sur Raspberry Pi 4 :strawberry:


:zero: Connection a la machine avec `docker-machine`

:pushpin: Connection à la machine

```
$ docker-machine ssh isaha
```

:pushpin: Mis à jour du `package manager` poue les futures installations

```
$ sudo apt-get update
```


:one: Permettre le routage interne entre les interfaces de réseaux sur le serveur

:pushpin: Modifier le fichier de configuration Système `/etc/sysctl.conf`


```
$ sudo nano /etc/sysctl.conf
```

:pushpin: Modifier le fichier Systeme

* Enlever le commentaire pour la variable `net.ipv4.ip_forward`

```
# Uncomment the next line to enable packet forwarding for IPv4
#net.ipv4.ip_forward=1
```

* Prendre en compte la modification

```
$ sudo sysctl -p
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
```

:two: Installer `OpenVPN`

:pushpin: Installer

```
$ sudo apt-get install openvpn
```

:pushpin: Vérifier

```
$ systemctl status openvpn
● openvpn.service - OpenVPN service
   Loaded: loaded (/lib/systemd/system/openvpn.service; enabled; vendor preset: enabled)
   Active: active (exited) since Fri 2019-12-06 10:10:15 EST; 1h 0min ago
```

:three: Installer `OpenVPN` et Gérer les clés avec `easy-RSA`

```
$ sudo apt-get install openvpn easy-rsa
```




:ab: Installer un 'firewall' par precaution

Continuer à [Pare-feu](firewall.md)
