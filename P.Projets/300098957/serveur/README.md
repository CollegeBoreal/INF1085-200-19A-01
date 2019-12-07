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

:two: Installer le service `OpenVPN`

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

:three: Gestion des clés avec `easy-rsa`

:pushpin: Installer l'utilitaire de création des clés

```
$ sudo apt-get install easy-rsa
```

:pushpin: Copier les fichiers d'examples d'`easy-rsa` dans la configuration d'`OpenVPN`


<image src ="images/manageKeys.png" width="737" height="427"></image>

```
$ sudo cp -r /usr/share/easy-rsa/ /etc/openvpn
$ sudo -i
# cd /etc/openvpn/easy-rsa
```

:pushpin: ouvrir le fichier `vars`

```
# nano vars
```

:pushpin: ajouter le contenu suivant

```
export KEY_COUNTRY="CA"
export KEY_PROVINCE="ON"
export KEY_CITY="Toronto"
export KEY_ORG="Bootstrap IT"
export KEY_EMAIL="300098957@collegeboreal.ca"
export KEY_OU="IT"
```

:pushpin: ajouter le fichier `vars` dans les variables d'environnement

```
# source vars
```

:m: Génerer les clés

```
# ./easyrsa clean-all

Note: using Easy-RSA configuration from: ./vars

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /etc/openvpn/easy-rsa/pki
```

:pushpin: Donner le `password` au New CA Key Passphrase (ne peux etre vide)

```
# ./easyrsa build-ca

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019

Enter New CA Key Passphrase:
Re-Enter New CA Key Passphrase:
Generating RSA private key, 2048 bit long modulus (2 primes)
.........................+++++
...................+++++
e is 65537 (0x010001)
Can't load /etc/openvpn/easy-rsa/pki/.rnd into RNG
3069857808:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:98:Filename=/etc/openvpn/easy-rsa/pki/.rnd
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:isaha

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/etc/openvpn/easy-rsa/pki/ca.crt

```

:pushpin:  build-server-full server

* Donner le `password` au `PEM pass` phrase (ne peux etre vide)

* Donner le `password` au `ca.key` (ne peux etre vide)

```

# ./easyrsa build-server-full server

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019
Generating a RSA private key
....+++++
....+++++
writing new private key to '/etc/openvpn/easy-rsa/pki/private/server.key.f1TfVHDXUv'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf
Enter pass phrase for /etc/openvpn/easy-rsa/pki/private/ca.key:
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'server'
Certificate is to be certified until Nov 20 23:39:04 2022 GMT (1080 days)

Write out database with 1 new entries
Data Base Updated
```

:pushpin:  Générer les fichiers Diffie-Hellman pour négocier l'authentification

```
# ./easyrsa gen-dh

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019
Generating DH parameters, 2048 bit long safe prime, generator 2
This is going to take a long time
..............................................................+...++*++*++*++*

DH parameters of size 2048 created at /etc/openvpn/easy-rsa/pki/dh.pem
```

:pushpin:  Copier les fichiers dans la configuration `OpenVPN`

```
# cp /etc/openvpn/easy-rsa/pki/issued/server.crt /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/dh.pem /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn
```



:ab: Installer un 'firewall' par precaution

Continuer à [Pare-feu](firewall.md)
