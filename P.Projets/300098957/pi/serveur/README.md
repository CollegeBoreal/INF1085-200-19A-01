# Installation d'un Serveur VPN sur Raspberry Pi 4 :strawberry:


https://www.howtoforge.com/tutorial/how-to-install-openvpn-server-and-client-with-easy-rsa-3-on-centos-7/


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
# openvpn --genkey --secret ta.key
```

:pushpin:  Copier les fichiers dans la configuration `OpenVPN`

```
# cp /etc/openvpn/easy-rsa/pki/private/server.key /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/issued/server.crt /etc/openvpn
# cp /etc/openvpn/easy-rsa/pki/dh.pem /etc/openvpn/dh2048.pem
# cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn
```

:pushpin:  Configurer le serveur `OpenVPN`

```
# zcat \
  /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz \
  > /etc/openvpn/server.conf
```

```
# cd /etc/openvpn
```

:four: Générer la partie client

```
# ./easyrsa build-client-full client nopass

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.1.1c  28 May 2019
Generating a RSA private key
....................................................................................+++++
.............+++++
writing new private key to '/etc/openvpn/easy-rsa/pki/private/client.key.qALrHIxusP'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf
Enter pass phrase for /etc/openvpn/easy-rsa/pki/private/ca.key:
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'client'
Certificate is to be certified until Nov 20 23:58:11 2022 GMT (1080 days)

Write out database with 1 new entries
Data Base Updated
```

:pushpin: Copier les clés vers `/etc/openvpn`

```
# cp /etc/openvpn/easy-rsa/pki/private/client.key /etc/openvpn/client
# cp /etc/openvpn/easy-rsa/pki/issued/client.crt /etc/openvpn/client
# cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn/client
```

:pushpin: Assembler la configiuration des les clés du client

```
# cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf \
 /etc/openvpn/client
```



* Redemarrez 

```
# systemctl start openvpn
```
:pushpin: Si vous recevez le message suivant

```
Broadcast message from root@isaha (Fri 2019-12-06 23:02:21 EST):

Password entry required for 'Enter Private Key Password:' (PID 1366).
Please enter password with the systemd-tty-ask-password-agent tool:
```

```
# sudo systemd-tty-ask-password-agent
Enter Private Key Password: ********
```

* Verifiez

```
# journalctl -xe
Dec 06 23:03:57 isaha ovpn-server[1380]: library versions: OpenSSL 1.1.1c  28 May 2019, LZO 2.10
Dec 06 23:03:57 isaha systemd[1]: Started OpenVPN connection to server.
-- Subject: A start job for unit openvpn@server.service has finished successfully
-- Defined-By: systemd
-- Support: https://www.debian.org/support
-- 
-- A start job for unit openvpn@server.service has finished successfully.
-- 
-- The job identifier is 7155.
Dec 06 23:03:57 isaha ovpn-server[1380]: NOTE: your local LAN uses the extremely common subnet address 192.168.0.x or 192.168.1.x. 
Dec 06 23:03:57 isaha ovpn-server[1380]: Diffie-Hellman initialized with 2048 bit key
Dec 06 23:03:58 isaha sudo[1383]:     root : TTY=pts/0 ; PWD=/etc/openvpn ; USER=root ; COMMAND=/bin/systemd-tty-ask-password-agent
Dec 06 23:03:58 isaha sudo[1383]: pam_unix(sudo:session): session opened for user root by pi(uid=0)
Dec 06 23:04:01 isaha ovpn-server[1380]: WARNING: this configuration may cache passwords in memory -- use the auth-nocache option t
Dec 06 23:04:01 isaha sudo[1383]: pam_unix(sudo:session): session closed for user root
Dec 06 23:04:01 isaha ovpn-server[1380]: ROUTE_GATEWAY 192.168.1.1/255.255.255.0 IFACE=eth0 HWADDR=dc:a6:32:1a:61:32
Dec 06 23:04:01 isaha kernel: tun: Universal TUN/TAP device driver, 1.6
Dec 06 23:04:01 isaha ovpn-server[1380]: TUN/TAP device tun0 opened
Dec 06 23:04:01 isaha ovpn-server[1380]: TUN/TAP TX queue length set to 100
Dec 06 23:04:01 isaha ovpn-server[1380]: /sbin/ip link set dev tun0 up mtu 1500
Dec 06 23:04:01 isaha ovpn-server[1380]: /sbin/ip addr add dev tun0 local 10.8.0.1 peer 10.8.0.2
Dec 06 23:04:01 isaha ovpn-server[1380]: /sbin/ip route add 10.8.0.0/24 via 10.8.0.2
Dec 06 23:04:01 isaha ovpn-server[1380]: Could not determine IPv4/IPv6 protocol. Using AF_INET
Dec 06 23:04:01 isaha ovpn-server[1380]: Socket Buffers: R=[163840->163840] S=[163840->163840]
Dec 06 23:04:01 isaha ovpn-server[1380]: UDPv4 link local (bound): [AF_INET][undef]:1194
Dec 06 23:04:01 isaha ovpn-server[1380]: UDPv4 link remote: [AF_UNSPEC]
Dec 06 23:04:01 isaha ovpn-server[1380]: MULTI: multi_init called, r=256 v=256
Dec 06 23:04:01 isaha ovpn-server[1380]: IFCONFIG POOL: base=10.8.0.4 size=62, ipv6=0
Dec 06 23:04:01 isaha ovpn-server[1380]: IFCONFIG POOL LIST
Dec 06 23:04:01 isaha ovpn-server[1380]: Initialization Sequence Completed
```

```
# ip addr
[...]
4: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc [...]
    link/none
    inet 10.8.0.1 peer 10.8.0.2/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::2b86:e81c:b45f:a8b1/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever
```

:ab: Installer un 'firewall' par precaution

Continuer à [Pare-feu](firewall.md)
