## :v: Securing network connections VPN `Chapter 10`

### :pushpin: Introduction

Virtual Private Networking (VPN) is used to set up a virtual network connection across another physical network connection.

### :pushpin: Installation of VPN

#### :one: STEP 1: INSTALL OPENVPN SOFTWARE

Firstly run the update 🧐

```
sudo apt update
```

Execute the following command to install OpenVPN 

```
sudo apt-get install network-manager-openvpn
```

#### :two: STEP 2:  VPN setting

Execute the folloning command and change user to nobody and group to nogroup
```
nano /etc/openvpn/server.conf
```
