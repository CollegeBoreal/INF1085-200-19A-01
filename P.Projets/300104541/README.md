# SAMBA

## 1.Introduction

### 2.Notre but 


## 1. Create a Samba user account on the Linux server. 

```
$ sudo apt-get update
$ sudo apt-get install samba
$ sudo -i
```
##  2.Designate a share directory 

```# mkdir -p /samba/sharehome 
   # touch /samba/sharehome/myfile
   # chmod 777 /samba/sharehome
   # nano /etc/samba/smb.conf
```

##  3.Define the share through the edit smb.conf.file

```
[sharehome] path = /samba/sharehome writable = yes
```
## 4. Test the configuration. 

```
#testparm
$ su sambauser
# smbclient //localhost/sharehome 
```








