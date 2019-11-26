# :strawberry: Static IP Change using `DHCP` Method

## :zero: Verifier son environnement

:pushpin: Récuperer l'adresse de la passerelle par défaut

```
$ ip route | grep default | awk '{print $3}'
```


## :one: Modifier le fichier `/etc/dhcpcd.conf`

:bookmark: En enlevant les commentaires devant les lignes de configurations suivantes

:pushpin: à la maison derrière son routeur

```
# Example static IP configuration:
interface eth0
static ip_address=192.168.1.10/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1 8.8.8.8
```

:pushpin: au Collège avec les adresses IP fixes `10.13.237.0/25` DNS `10.10.99.2` et `10.10.99.3`

```
# Example static IP configuration:
interface eth0
static ip_address=10.13.237.16/25
static routers=10.13.237.1
static domain_name_servers=10.10.99.2 10.10.99.3 8.8.8.8 
```

## :two: Redémarrer la machine

```
$ sudo reboot
```
