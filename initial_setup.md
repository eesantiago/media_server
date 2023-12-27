### Partitioning External Hardrive larger than 2TB 

sudo lsblk



Verify Filesystem Type
df -Th

make it ext4
mkfs.ext4 /dev/sda1

Copyinbg from another disk 

dd if of bs=64K status=progress



Add group with permissions to read_write to the media drive

sudo groupadd media

set owner of mnt media to media group 
set all files and dirs with read write for group

Add users for each of the services you will be usung:

sudo useradd -m /var/lib/radarr -s /usr/sbin/nologin radarr

