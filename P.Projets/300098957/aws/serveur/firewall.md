# Parefeu

:one: Installer un 'firewall' par precaution

:pushpin: installer `ufw` (Uncomplicated firewall)

```
$ sudo apt-get install ufw
```

:pushpin: installer les règles parefeux pour les services `ssh`, `docker` et `OpenVPN` (eventuellement `MySQL` )  

```
$ sudo ufw allow 22
$ sudo ufw allow 1194
$ sudo ufw allow 2376
```

:pushpin: Installer `ufw`

```
$ sudo ufw enable
```

:pushpin: Vérifier l'installation de `ufw` au démarrage de la machine

```
$ systemctl status ufw
● ufw.service - Uncomplicated firewall
   Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
   Active: active (exited) since Fri 2019-12-06 09:50:20 EST; 6min ago
```

```
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere                  
1194                       ALLOW       Anywhere                  
2376                       ALLOW       Anywhere                  
22 (v6)                    ALLOW       Anywhere (v6)             
1194 (v6)                  ALLOW       Anywhere (v6)             
2376 (v6)                  ALLOW       Anywhere (v6)
```

:pushpin: Redémarrer la machine

```
$ sudo reboot -h now
```
