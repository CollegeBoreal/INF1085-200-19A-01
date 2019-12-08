# Installation d'un Serveur VPN sur [AWS](https://aws.amazon.com/console/)

:zero: Connection a la machine avec `docker-machine`

:pushpin: Connection à la machine Ubuntu 18.0.4 LTS

```
$ export AWS_PROFILE=aws # Choisir son profil AWS
$ docker-machine ssh cb-dev
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

:warning: apt-get installe une vieille version 2.2 de Easy-RSA 

```
# sudo apt-get install --simulate easy-rsa
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  easy-rsa
0 upgraded, 1 newly installed, 0 to remove and 21 not upgraded.
Inst easy-rsa (2.2.2-2 Ubuntu:18.04/bionic [all])
Conf easy-rsa (2.2.2-2 Ubuntu:18.04/bionic [all])```
```

:bookmark: Installer manuellement (i.e. la version 3.0.4), beaucoup plus recente

```
#  wget -qO- https://github.com/OpenVPN/easy-rsa/releases/download/v3.0.4/EasyRSA-3.0.4.tgz \
  | tar --transform 's/^EasyRSA-3.0.4/easy-rsa/' -xvz -C /usr/share
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

:pushpin: remplacer la section du certificat avec le contenu suivant

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
**************************************************************
  No /etc/openvpn/easy-rsa/openssl.cnf file could be found
  Further invocations will fail
**************************************************************
NOTE: If you run ./clean-all, I will be doing a rm -rf on /etc/openvpn/easy-rsa/keys
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
# ./easyrsa build-ca nopass

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019
Generating RSA private key, 2048 bit long modulus (2 primes)
..........................................................................+++++
............................................................+++++
e is 65537 (0x010001)
Can't load /etc/openvpn/easy-rsa/pki/.rnd into RNG
3069456400:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:98:Filename=/etc/openvpn/easy-rsa/pki/.rnd
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

```
# ./easyrsa build-server-full server nopass

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019
Generating a RSA private key
.........................................................................................+++++
......+++++
writing new private key to '/etc/openvpn/easy-rsa/pki/private/server.key.qFBe6EBR49'
-----
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'server'
Certificate is to be certified until Nov 21 22:17:59 2022 GMT (1080 days)

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

:pushpin:  Gérérer une signature HMAC permettant d'augmenter les capacités de vérification du serveur d'intégrité TLS

```
# openvpn --genkey --secret pki/ta.key
```

:pushpin:  Copier les fichiers dans la configuration `OpenVPN`

```
# cp /etc/openvpn/easy-rsa/pki/private/server.key /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/issued/server.crt /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/ta.key /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/dh.pem /etc/openvpn/dh2048.pem
```

:pushpin:  Configurer le serveur `OpenVPN`

```
# zcat \
  /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz \
  > /etc/openvpn/server.conf
```

* Verifier les fichiers de configurations dans :

```
# ls -lt /etc/openvpn/
total 52
-rw-r--r-- 1 root root 10853 Dec  8 15:43 server.conf
-rw------- 1 root root   424 Dec  8 15:42 dh2048.pem
-rw------- 1 root root   636 Dec  8 15:42 ta.key
-rw------- 1 root root  1184 Dec  8 15:42 ca.crt
-rw------- 1 root root  4586 Dec  8 15:42 server.crt
-rw------- 1 root root  1704 Dec  8 15:42 server.key
[...]
```

* Activer et Démarrer OpenVPN

```
# systemctl enable openvpn
# systemctl start openvpn
```


:bulb: si Erreur de configuration rectifier et recharger la configuration

```
# systemctl reload openvpn
```

* Verifier le démarrage dans le log

```
# journalctl -xe
Dec 08 15:45:35 cb-dev ovpn-server[11226]: Outgoing Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC au
Dec 08 15:45:35 cb-dev ovpn-server[11226]: Incoming Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC au
Dec 08 15:45:35 cb-dev ovpn-server[11226]: ROUTE_GATEWAY 172.31.32.1/255.255.240.0 IFACE=eth0 HWADDR=0e:0f:c3:98:60:93
Dec 08 15:45:35 cb-dev ovpn-server[11226]: TUN/TAP device tun0 opened
Dec 08 15:45:35 cb-dev ovpn-server[11226]: TUN/TAP TX queue length set to 100
Dec 08 15:45:35 cb-dev ovpn-server[11226]: do_ifconfig, tt->did_ifconfig_ipv6_setup=0
Dec 08 15:45:35 cb-dev ovpn-server[11226]: /sbin/ip link set dev tun0 up mtu 1500
Dec 08 15:45:35 cb-dev systemd-networkd[608]: tun0: Gained carrier
Dec 08 15:45:35 cb-dev systemd-networkd[608]: tun0: Gained IPv6LL
Dec 08 15:45:35 cb-dev systemd-timesyncd[481]: Network configuration changed, trying to establish connection.
Dec 08 15:45:35 cb-dev ovpn-server[11226]: /sbin/ip addr add dev tun0 local 10.8.0.1 peer 10.8.0.2
Dec 08 15:45:35 cb-dev networkd-dispatcher[832]: WARNING:Unknown index 4 seen, reloading interface list
Dec 08 15:45:35 cb-dev ovpn-server[11226]: /sbin/ip route add 10.8.0.0/24 via 10.8.0.2
Dec 08 15:45:35 cb-dev ovpn-server[11226]: Could not determine IPv4/IPv6 protocol. Using AF_INET
Dec 08 15:45:35 cb-dev ovpn-server[11226]: Socket Buffers: R=[212992->212992] S=[212992->212992]
Dec 08 15:45:35 cb-dev ovpn-server[11226]: UDPv4 link local (bound): [AF_INET][undef]:1194
Dec 08 15:45:35 cb-dev ovpn-server[11226]: UDPv4 link remote: [AF_UNSPEC]
Dec 08 15:45:35 cb-dev ovpn-server[11226]: MULTI: multi_init called, r=256 v=256
Dec 08 15:45:35 cb-dev ovpn-server[11226]: IFCONFIG POOL: base=10.8.0.4 size=62, ipv6=0
Dec 08 15:45:35 cb-dev ovpn-server[11226]: IFCONFIG POOL LIST
Dec 08 15:45:35 cb-dev ovpn-server[11226]: Initialization Sequence Completed
Dec 08 15:45:35 cb-dev systemd-udevd[11228]: link_config: autonegotiation is unset or enabled, the speed and duplex are not writa
Dec 08 15:45:35 cb-dev systemd-timesyncd[481]: Synchronized to time server 91.189.89.198:123 (ntp.ubuntu.com).
lines 2586-2608/2608 (END)
```

* Vérifier let tunnel dans la configuration réseau `tun0`

```
# ip addr
[...]
4: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 100
    link/none 
    inet 10.8.0.1 peer 10.8.0.2/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::5fc1:cc53:f773:9597/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever
```

:b: Générer la partie client

```
$ sudo -i
# cd /etc/openvpn/easy-rsa
```

* Générer la cle

```
# ./easyrsa build-client-full client nopass
**************************************************************
  No /etc/openvpn/easy-rsa/openssl.cnf file could be found
  Further invocations will fail
**************************************************************
NOTE: If you run ./clean-all, I will be doing a rm -rf on /etc/openvpn/easy-rsa/keys

Note: using Easy-RSA configuration from: ./vars
Can't load /etc/openvpn/easy-rsa/pki/.rnd into RNG
139647652704704:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/etc/openvpn/easy-rsa/pki/.rnd
Generating a RSA private key
...................................................+++++
..................+++++
writing new private key to '/etc/openvpn/easy-rsa/pki/private/client.key.FoxazfwZuV'
-----
Using configuration from ./openssl-easyrsa.cnf
Can't load /etc/openvpn/easy-rsa/pki/.rnd into RNG
140648370180544:error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/etc/openvpn/easy-rsa/pki/.rnd
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'client'
Certificate is to be certified until Dec  5 15:52:44 2029 GMT (3650 days)

Write out database with 1 new entries
Data Base Updated
```

:pushpin: Copier les clés vers `/etc/openvpn/client`

```
# cp /etc/openvpn/easy-rsa/pki/private/client.key /etc/openvpn/client
# cp /etc/openvpn/easy-rsa/pki/issued/client.crt /etc/openvpn/client
# cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn/client
# cp /etc/openvpn/easy-rsa/pki/ta.key /etc/openvpn/client
```

:pushpin: Assembler la configuration des clés du client

```
# cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf \
 /etc/openvpn/client
```

```
# zip client.zip client/*
```

```
# chown pi:pi client.zip
# mv client.zip ~pi/Desktop
```


:ab: Installer un 'firewall' par precaution

Continuer à [Pare-feu](firewall.md)
