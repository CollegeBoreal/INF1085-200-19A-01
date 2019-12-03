## Introduction VPN

Réseau privé virtuel est un type de réseau privé qui utilise les télécommunications publiques, telles que l'internet, au lieu de lignes louées pour communiquer.
Devenu populaire au fur et à mesure que plus d'employés travaillaient dans les secteurs suivants les endroits éloignés.

## Bref aperçu de son fonctionnement

- Deux connexions - l'une est faite à l'Internet et la seconde est faite au VPN.
- Datagrammes - contient les données, la destination et l'adresse et l'information source.
- Pare-feu - Les VPN permettent aux utilisateurs autorisés de passer à travers les pares-feux.
- Protocoles - les protocoles créent les tunnels VPN

## Comment installer !

:one: Nous allons récupérer les informations sur les paquets qui peuvent etre installés. y compris les mises à jour des paquets actuellement installés qui sont disponibles, à partir des sources Internet.
```
$ sudo apt-get update
```
![](pj(1).png)

Et nous installerons également le paquet easy-rsa, qui nous aidera à mettre en place une CA (autorité de certification) interne pour une utilisation avec notre VPN
```
$ sudo apt-get install openvpn easy-rsa
```
![](pj(2).png)

:two:
