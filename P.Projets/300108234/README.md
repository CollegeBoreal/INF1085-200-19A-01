# ARCHIVE 

scp .ssh/id_rsa.pub \
ubuntu@10.0.3.142:/home/ubuntu/.ssh/authorized_keys


the c tells tar to create a new archive, v
sets the screen output to verbose so I’ll get updates, and f points to the filename I’d
like the archive to get:
$ tar cvf archivename.tar *

to back up files:
$ tar cvf archivename.tar *.mp4

to compress the archive:
$ tar czvf archivename.tar.gz *.mp4


This example adds all the
.mp4 files in the /home/myuser/Videos/ directory:
$ tar czvf archivename.tar.gz /home/myuser/Videos/*.mp4


In this example, -b tells Linux to split the archivename.tar.gz file into 1 GB-sized
parts;

$ split -b 1G archivename.tar.gz "archivename.tar.gz.part"


redirect the output to a new file called
archivename.tar.gz:
$ cat archivename.tar.gz.part* > archivename.tar.gz


to show you how to create an archive image
of a working Linux installation and stream it to a remote storage location

tar czvf - --one-file-system / /usr /var \
--exclude=/home/andy/ | ssh username@10.0.3.141 \
"cat > /home/username/workstation-backup-Apr-10.tar.gz"


examples to explore it
one piece at a time. Let’s create an archive of the contents of a directory called importantstuff
that’s filled with, well, really important stuff:

$ tar czvf - importantstuff/ | ssh username@10.0.3.141 \
<linearrow /> "cat > /home/username/myfiles.tar.gz"


copy the archive file
to a USB drive. It should make sense to you now:
# tar czvf - --one-file-system / /usr /var \
--exclude=/home/andy/ | ssh username@10.0.3.141 \
"cat > /home/username/workstation-backup-Apr-10.tar.gz"


locate will look for files whose names end with the string video.mp4:
$ locate *video.mp4

running updatedb:
# updatedb

lists the contents of a directory
$ ls -l /bin | grep zcat
-rwxr-xr-x 1 root root 1937 Oct 27 2014 zcat
