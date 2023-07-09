  
# [root@localhost ~]# lsblk
    NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda               8:0    0   20G  0 disk
    ├─sda1            8:1    0    1G  0 part /boot
    └─sda2            8:2    0   19G  0 part
      ├─centos-root 253:0    0   17G  0 lvm  /
      └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
    sdb               8:16   0   20G  0 disk
    sdc               8:32   0   20G  0 disk
    sr0              11:0    1  973M  0 rom


# [root@localhost ~]# pvcreate /dev/sdb /dev/sdc
      Physical volume "/dev/sdb" successfully created.
      Physical volume "/dev/sdc" successfully created.


# [root@localhost ~]# vgcreate HPCSA /dev/sdb /dev/sdc
      Volume group "HPCSA" successfully created


# [root@localhost ~]# lvcreate -n hpcsa_lab --size 1G HPCSA
      Logical volume "hpcsa_lab" created.


# [root@localhost ~]# fdisk /dev/mapper/HPCSA-hpcsa_lab
    Welcome to fdisk (util-linux 2.23.2).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.
    
    Device does not contain a recognized partition table
    Building a new DOS disklabel with disk identifier 0x96b763f8.
    
    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1):
    First sector (2048-2097151, default 2048):
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151):
    Using default value 2097151
    Partition 1 of type Linux and of size 1023 MiB is set
    
    Command (m for help): w
    The partition table has been altered!
    
    Calling ioctl() to re-read partition table.
    
    WARNING: Re-reading the partition table failed with error 22: Invalid argument.
    The kernel still uses the old table. The new table will be used at
    the next reboot or after you run partprobe(8) or kpartx(8)
    Syncing disks.


# [root@localhost ~]# mkfs.ext4 /dev/mapper/HPCSA-hpcsa_lab
    mke2fs 1.42.9 (28-Dec-2013)
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    65536 inodes, 262144 blocks
    13107 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=268435456
    8 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (8192 blocks): done
    Writing superblocks and filesystem accounting information: done



# [root@localhost ~]# mkfs.ext4 /dev/mapper/HPCSA-hpcsa_lab
    mke2fs 1.42.9 (28-Dec-2013)
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    65536 inodes, 262144 blocks
    13107 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=268435456
    8 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (8192 blocks): done
    Writing superblocks and filesystem accounting information: done
    

# [root@localhost ~]# mkdir lab

# [root@localhost ~]# mount /dev/mapper/HPCSA-hpcsa_lab lab


# [root@localhost ~]# fdisk -l

    Disk /dev/sda: 21.5 GB, 21474836480 bytes, 41943040 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x00098520
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sda1   *        2048     2099199     1048576   83  Linux
    /dev/sda2         2099200    41943039    19921920   8e  Linux LVM
    
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes, 41943040 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    
    
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes, 41943040 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    
    
    Disk /dev/mapper/centos-root: 18.2 GB, 18249416704 bytes, 35643392 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    
    
    Disk /dev/mapper/centos-swap: 2147 MB, 2147483648 bytes, 4194304 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    
    
    Disk /dev/mapper/HPCSA-hpcsa_lab: 1073 MB, 1073741824 bytes, 2097152 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes

# [root@localhost ~]# pvs
      PV         VG     Fmt  Attr PSize   PFree
      /dev/sda2  centos lvm2 a--  <19.00g      0
      /dev/sdb   HPCSA  lvm2 a--  <20.00g <19.00g
      /dev/sdc   HPCSA  lvm2 a--  <20.00g <20.00g


# [root@localhost ~]# lvs 
      LV        VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
      hpcsa_lab HPCSA  -wi-ao----   1.00g
      root      centos -wi-ao---- <17.00g
      swap      centos -wi-ao----   2.00g


# [root@localhost ~]# lvextend -L +2G /dev/mapper/HPCSA-hpcsa_lab
      Size of logical volume HPCSA/hpcsa_lab changed from 1.00 GiB (256 extents) to 3.00 GiB (768 extents).
      Logical volume HPCSA/hpcsa_lab successfully resized.

# [root@localhost ~]# lvs
      LV        VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
      hpcsa_lab HPCSA  -wi-ao----   3.00g
      root      centos -wi-ao---- <17.00g
      swap      centos -wi-ao----   2.00g


# [root@localhost ~]# resize2fs /dev/mapper/HPCSA-hpcsa_lab
    resize2fs 1.42.9 (28-Dec-2013)
    Filesystem at /dev/mapper/HPCSA-hpcsa_lab is mounted on /root/lab; on-line resizing required
    old_desc_blocks = 1, new_desc_blocks = 1
    The filesystem on /dev/mapper/HPCSA-hpcsa_lab is now 786432 blocks long.


# [root@localhost ~]# lvcreate -L 1GB -s -n hpcsa_lab_snap /dev/mapper/HPCSA-hpcsa_lab
      Logical volume "hpcsa_lab_snap" created.


# [root@localhost ~]# lvconvert --merge /dev/mapper/HPCSA-hpcsa_lab
      HPCSA/hpcsa_lab cannot be merged because --trackchanges was not used.'


# [root@localhost ~]# blkid
    /dev/sda1: UUID="103f2703-2fa9-456d-a973-d6a6d187543e" TYPE="xfs"
    /dev/sda2: UUID="X4SfM1-CBwZ-rFpp-dbMj-uqDu-b4NQ-ODhIyH" TYPE="LVM2_member"
    /dev/sdb: UUID="BSTKhK-v5dK-J9OH-y3c4-iMmA-5USn-LQQDC9" TYPE="LVM2_member"
    /dev/sr0: UUID="2020-11-03-14-55-29-00" LABEL="CentOS 7 x86_64" TYPE="iso9660" PTTYPE="dos"
    /dev/sdc: UUID="fCDL2K-bKJG-EbCd-2aLa-LvoH-vnoD-QAyPZx" TYPE="LVM2_member"
    /dev/mapper/centos-root: UUID="f346ee40-7522-449f-89df-3b34af5f8ccf" TYPE="xfs"
    /dev/mapper/centos-swap: UUID="7426d544-2b20-475c-b478-2d6311083a04" TYPE="swap"
    /dev/mapper/HPCSA-hpcsa_lab: UUID="41c833f1-bcdb-4d5f-80d6-f55ce724c900" TYPE="ext4"
    /dev/mapper/HPCSA-hpcsa_lab_snap: UUID="41c833f1-bcdb-4d5f-80d6-f55ce724c900" TYPE="ext4"
    [root@localhost ~]# vim /etc/fstab
    -bash: vim: command not found


# [root@localhost ~]# yum install vim -y


# [root@localhost ~]# vim /etc/fstab
        #
        # /etc/fstab
        # Created by anaconda on Sun Jul  9 07:27:01 2023
        #
        # Accessible filesystems, by reference, are maintained under '/dev/disk'
        # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
        #
        /dev/mapper/centos-root /                       xfs     defaults        0 0
        UUID=103f2703-2fa9-456d-a973-d6a6d187543e /boot                   xfs     defaults        0 0
        UUID=41c833f1-bcdb-4d5f-80d6-f55ce724c900 /lab  ext4 defaults 0 0
        /dev/mapper/centos-swap swap                    swap    defaults        0 0
        ~

			
# [root@localhost ~]# lvconvert --merge /dev/mapper/HPCSA-hpcsa_lab
      HPCSA/hpcsa_lab cannot be merged because --trackchanges was not used.


# [root@localhost ~]# mount -a
    mount: mount point /lab does not exist

# [root@localhost ~]# ls
    anaconda-ks.cfg  lab

#  [root@localhost ~]# cd /lab
    -bash: cd: /lab: No such file or directory

# [root@localhost ~]# cd lab

# [root@localhost lab]# ls
    lost+found

# [root@localhost lab]# lvconvert --merge /dev/mapper/HPCSA-hpcsa_lab
      HPCSA/hpcsa_lab cannot be merged because --trackchanges was not used.

# [root@localhost lab]# lvconvert --merge /dev/mapper/HPCSA-hpcsa_lab lab
      HPCSA/hpcsa_lab cannot be merged because --trackchanges was not used.
      Volume group "lab" not found
      Cannot process volume group lab


# [root@localhost lab]# lvcreate --activate y --type raid1 -m 3 -n split_tracking -L 500M split_image
      Volume group "split_image" not found
      Cannot process volume group split_image


# [root@localhost lab]# cd


# [root@localhost ~]# lvconvert --merge /dev/mapper/HPCSA-hpcsa_lab_snap
      Delaying merge since origin is open.
      Merging of snapshot HPCSA/hpcsa_lab_snap will occur on next activation of HPCSA/hpcsa_lab.
#################################################################################################



