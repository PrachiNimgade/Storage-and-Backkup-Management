
# Set 4 hdd 

# lsblk
# yum clean all
# yum update -y
# yum install mdadm -y
# mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
# mdadm --create --verbose /dev/md2 --level=1 --raid-devices=2 /dev/sdd /dev/sde
# mdadm --create --verbose /dev/md1 --level=0 --raid-devices=2 /dev/md1 /dev/md2
# cat /proc/mdstat
# mdadm --detail /dev/md0
   fdisk  /dev/md0
       m
       n
       p
       enter
       enter
       enter
       w-detail /dev/md1
  # fdisk -l
  # mkfs.ext4 /dev/md0p1
  # mkdir /mnt/raid10
  # mount /dev/md0p1 /mnt/raid10/
  # df -h
  # blkid   (we get uuid that will copied on below file)
  # vim /etc/fstab
            UUID=4e36c3e0-b6bd-4946-83cf-eb2abb10f3ba /mnt/raid10 ext4 defaults 0 0
  # mount -a
  # cd /mnt/raid10
  ls

# mdadm --stop  /dev/md0
# mdadm --remove /dev/md0
# mdadm --zero-superblock /dev/sd[c-e]


################################################################################################











