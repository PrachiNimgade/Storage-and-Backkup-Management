
MACHINE 2 FOR RAID 1
===========================================================================================
      Set 2 hdd of 1 gb 
      Reboot
      lsblk
      yum clean all
      yum update -y
      yum install mdadm -y
      mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
      cat /proc/mdstat
       mdadm --detail /dev/md1
       fdisk  /dev/md1
           m
           n
           p
           enter
           enter
           enter
           w-detail /dev/md1
      fdisk -l
      mkfs.ext4 /dev/md0p1
      mkdir /mnt/raid1
      mount /dev/md1p1 /mnt/raid1/
      df -h
      blkid   (we get uuid that will copied on below file)
      vim /etc/fstab
                UUID=4e36c3e0-b6bd-4946-83cf-eb2abb10f3ba /mnt/raid1 ext4 defaults 0 0
      mount -a
      vim test.txt
          matter...
                  Machine halt  power off, then remove one  hdd from machine 
                  Power on that machine
      cd /mnt/raid1
      ls
                  Again power off and add one hdd with 1 GB
                  see, state = clean, degraded
                  Power on
      lsblk
              Check which hdd  is blank where sdc or sdb.... in my machine sdb is blank
      mdadm --detail /dev/md1
      mdadm --add /dev/md1 dev/sdb    (here we add raid1)
      cat /proc/mdstat
      mdadm --detail /dev/md1




