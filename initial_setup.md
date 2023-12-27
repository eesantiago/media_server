### Partitioning External Hardrive larger than 2TB 

sudo lsblk



Verify Filesystem Type
df -Th

make it ext4
mkfs.ext4 /dev/sda1
