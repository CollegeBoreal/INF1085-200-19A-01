 # 🐱‍🏍`Samba`  Chapitre 12 : Sharing data over a private network
 
 ## :pushpin: How to set up Samba in Ubuntu/Linux, and access it in Windows ?
 
 ### :one: what is samba ?
``` 
Samba is a free software re-implementation of the SMB networking protocol, and was originally developed by    Andrew Tridgell 🕵️‍♂️
Samba allows to share files and printers with other computers remotely, regardless their operating system (linux, windows, Mac, ...). 
```

##### This guide show how to intall and configure the Samba service in a Ubuntu machine and access it through windows 

## :pushpin: Setting up the Samba File Server on Ubuntu/Linux:

### :two: Installing Samba

Firstly run the update

```
sudo apt update
```

Install samba with the following command:   

```
sudo apt-get install samba
```
you can check if the installation was successful by running:

```
whereis samba
```
The following should be its output:

```
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```

### :three: Configuring Samba

Now that Samba is installed, you need to create a directory for it to share:

Before that make sure you are connecting on rout, by typing:

```
sudo -i
```

Create the share folder: 

```
mkdir /home/<username>/fileshare/

```
The command above creates a new folder "fileshare" in your home directory which you will share later.

The configuration file for Samba is located at /etc/samba/smb.conf. To add the new directory as a share, you edit the file by running:

```
vi /etc/samba/smb.conf
```

set your share folder by adding the following lines:

```
[fileshare]                                --> name of your share file/ optional
    comment = Samba on Ubuntu              --> A brief description of the share/ optional
    path = /home/username/sambashare       --> where you create your share file/ The directory of your share.
    read only = no                         --> Permission to modify the contents of the share folder
    browsable = yes                        --> file managers will list this share under "Network"
```
Then press `ESC` and type `:wq` to save and exit from the vi text editor.

Now that we have you new share configured, save it and restart Samba for it to take effect:

```
sudo service smbd restart
```

Update the firewall rules to allow Samba traffic:

```
sudo ufw allow samba
```

## :pushpin: Setting up User Accounts

### :four: set up user account

Since Samba doesn't use the system account password, we need to set up a Samba password for our user account:

```
sudo smbpasswd -a username
```
## :pushpin: Connecting to Share

### :five: Accessing to Samba Server Files

##### 🌈 On Windows:
```
Start button -> Run
Type: \\<your-ip-address-to-ubuntu>\<your-share-folder>
```
All set

##### 🍏 On Mac OS:
```
Open finder
Menu Go -> Connect to server (command-k)
In the "Server Address" textbox, type: smb://<your-ip-address-to-ubuntu>
Connect
Select guest and OK
```
Your all set, you'll be able to see /<your-share-folder> from here.
