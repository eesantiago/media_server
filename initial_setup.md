### Partitioning External Hardrive larger than 2TB 

sudo lsblk

* be sure to restart the services when you make changes to permissions of the accounts that run the services (i.e restart radarr after making permission changes for the radarr user)
* 

Verify Filesystem Type
df -Th

make it ext4
mkfs.ext4 /dev/sda1

Copyinbg from another disk 

dd if of bs=64K status=progress

# add a user to the group
sudo usermod -aG media debian-transmission

Add group with permissions to read_write to the media drive

sudo groupadd media

set owner of mnt media to media group 
set all files and dirs with read write for group

Add users for each of the services you will be usung:

sudo useradd -m /var/lib/radarr -s /usr/sbin/nologin radarr
use this one

to do 
-change radarr home directory, delete old
-

create the media group

```
sudo addgroup media
cat /etc/group | grep media
```
Change group ownership of the files in the media drive recursivley (must change the owner and then change permissions)
```
sudo chown -R :media /mnt/media
ls -la /mnt/media
```
Give the group RW access (do not change the owber of the file) to the mounted media drive and sub directories:
```
sudo chmod -R g+rw /mnt/media

3DO INSTEAD
sudo chmod -R 774 /mnt/media
ls -la /mnt/media
```

## install Radarr

use the fuide here: https://wiki.servarr.com/radarr/installation/linux

create user radarr 

```
sudo adduser radarr
password: radarr

getent XXXX
```
Add directory /var/lib/radarr and mak the media group and radarr user rw permission rw permissions:
```
sudo mkdir /var/lib/radarr
sudo chown -R radarr:media /var/lib/radarr
sudo chmod -R u=rw /var/lib/radarr
sudo chmod -R g=rw /var/lib/radarr
ls -la /var/lib | grep radarr
```
