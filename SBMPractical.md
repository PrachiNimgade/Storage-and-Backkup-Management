MACHINE 1 FOR RAID 0
====================================================================================================
        Set 2 hdd of 1 gb 
        Reboot
        Login in putty
        lsblk
        yum clean all
        yum update -y
        yum install mdadm -y
        mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc
        cat /proc/mdstat
        lsblk
        mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc
                    mdadm: chunk size defaults to 512K
                    mdadm: Defaulting to version 1.2 metadata
                    mdadm: array /dev/md0 started.
        
        cat /proc/mdstat
                   Personalities : [raid0]
                  md0 : active raid0 sdc[1] sdb[0]
                  2093056 blocks super 1.2 512k chunks
         
         mdadm --detail /dev/md0
         fdisk  /dev/md0
             m
             n
             p
             enter
             enter
             enter
             w
                     The partition table has been altered!
                    Calling ioctl() to re-read partition table.
                    Syncing disks.
        
        fdisk -l
        
        mdadm  --examine /dev/sdb
        mdadm  --examine /dev/sdc
        mdadm  --detail /dev/md0
        
        mkfs.ext4 /dev/md0p1
        
        mkdir /mnt/raid0
        mount /dev/md0p1 /mnt/raid0/
        df -h
        cd /mnt/raid0/
          touch test
          ls
        dd if=/dev/zero of=500mb.file bs=1024 count=502400
        df -h
#########################################################################################



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




