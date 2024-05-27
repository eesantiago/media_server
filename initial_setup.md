# Plex Media Server with Automation

Helpful links:
* [Servarr Wiki](https://wiki.servarr.com/)
* [Creating a Partition Greater than 2TB](https://www.cyberciti.biz/tips/fdisk-unable-to-create-partition-greater-2tb.html)

<br/>

## Partitioning and Mounting a new Harddrive

After the disk is plugged in, find the name:
```
sudo lsblk

sda               8:0    0   7.3T  0 disk
└─sda1            8:1    0   7.3T  0 part  /mnt/media

# verify
fdisk -l /dev/sda
```
Verify filesystem type:
```
df -Th

/dev/mapper/data-root ext4      431G   31G  379G   8% /
```
Create a partition:
```
sudo parted /dev/sda

# complete the following in parted
mklabel gpt
unit TB
mkpart primary 0.00TB 8.00TB
print
quit
```
Format the file system:
```
sudo mkfs.ext4 /dev/sda1
```
Mount the new drive:
```
mkdir /mnt/media
mount /dev/sda1 /mnt/media

# verify
df -H
```
If copying over from another disk:
```
dd if of bs=64K status=progress
```

<br/>

## Users, Groups, and Permissions


# add a user to the group
sudo usermod -aG media debian-transmission

Add group with permissions to read_write to the media drive

sudo groupadd media

set owner of mnt media to media group 
set all files and dirs with read write for group

Add users for each of the services you will be usung:

sudo useradd -m /var/lib/radarr -s /usr/sbin/nologin radarr

# if the direcotry already exists
sudo useradd --home /var/lib/radarr -s /usr/sbin/nologin radarr

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
Change the setting in radarr permissions to 774 and click yes to set permissions 
settings - media management - show advanced - permissions
744
cmhmod group = media

## ensuer that all services place files in the media drive with the permission 774, the group must be able to read write and execute (watch)
- ensure when moving to a new radarr instance the MEdiaCover files are copied over, this is not done automatically when restoring from backup
### Partitioning External Hardrive larger than 2TB 


jackett 
transmission
lidarr
* be sure to restart the services when you make changes to permissions of the accounts that run the services (i.e restart radarr after making permission changes for the radarr user)
