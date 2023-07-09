MIRRORING IN LVM
=================================================================================================

# [root@localhost ~]# lvcreate -L 1G -m1 -n hpcsa_lab3 HPCSA
      Logical volume "hpcsa_lab3" created.

# [root@localhost ~]# lvs
      LV         VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
      hpcsa_lab  HPCSA  Owi-aos---   3.00g
      hpcsa_lab3 HPCSA  rwi-a-r---   1.00g                                    100.00
      root       centos -wi-ao---- <17.00g
      swap       centos -wi-ao----   2.00g
# [root@localhost ~]# pvs
      PV         VG     Fmt  Attr PSize   PFree
      /dev/sda2  centos lvm2 a--  <19.00g     0
      /dev/sdb   HPCSA  lvm2 a--  <20.00g 14.99g
      /dev/sdc   HPCSA  lvm2 a--  <20.00g 18.99g


# [root@localhost ~]# lsblk
    NAME                        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda                           8:0    0   20G  0 disk
    ├─sda1                        8:1    0    1G  0 part /boot
    └─sda2                        8:2    0   19G  0 part
      ├─centos-root             253:0    0   17G  0 lvm  /
      └─centos-swap             253:1    0    2G  0 lvm  [SWAP]
    sdb                           8:16   0   20G  0 disk
    ├─HPCSA-hpcsa_lab-real      253:3    0    3G  0 lvm
    │ ├─HPCSA-hpcsa_lab         253:2    0    3G  0 lvm  /root/lab
    │ └─HPCSA-hpcsa_lab_snap    253:5    0    3G  0 lvm
    ├─HPCSA-hpcsa_lab_snap-cow  253:4    0    1G  0 lvm
    │ └─HPCSA-hpcsa_lab_snap    253:5    0    3G  0 lvm
    ├─HPCSA-hpcsa_lab3_rmeta_0  253:6    0    4M  0 lvm
    │ └─HPCSA-hpcsa_lab3        253:10   0    1G  0 lvm
    └─HPCSA-hpcsa_lab3_rimage_0 253:7    0    1G  0 lvm
      └─HPCSA-hpcsa_lab3        253:10   0    1G  0 lvm
    sdc                           8:32   0   20G  0 disk
    ├─HPCSA-hpcsa_lab3_rmeta_1  253:8    0    4M  0 lvm
    │ └─HPCSA-hpcsa_lab3        253:10   0    1G  0 lvm
    └─HPCSA-hpcsa_lab3_rimage_1 253:9    0    1G  0 lvm
      └─HPCSA-hpcsa_lab3        253:10   0    1G  0 lvm
    sr0                          11:0    1  973M  0 rom


# [root@localhost ~]# lvs
      LV         VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
      hpcsa_lab  HPCSA  Owi-aos---   3.00g
      hpcsa_lab3 HPCSA  rwi-a-r---   1.00g                                    100.00
      root       centos -wi-ao---- <17.00g
      swap       centos -wi-ao----   2.00g
    


# [root@localhost ~]# lvcreate -L 1GB -s -n hpcsa_lab_snap3 /dev/mapper/HPCSA-hpcsa_lab3
      Logical volume "hpcsa_lab_snap3" created.


# [root@localhost ~]# lvs
      LV              VG     Attr       LSize   Pool Origin     Data%  Meta%  Move Log Cpy%Sync Convert
      hpcsa_lab       HPCSA  Owi-aos---   3.00g
      hpcsa_lab3      HPCSA  owi-a-r---   1.00g                                        100.00
      hpcsa_lab_snap3 HPCSA  swi-a-s---   1.00g      hpcsa_lab3 0.00
      root            centos -wi-ao---- <17.00g
      swap            centos -wi-ao----   2.00g


# [root@localhost ~]# pvs
      PV         VG     Fmt  Attr PSize   PFree
      /dev/sda2  centos lvm2 a--  <19.00g     0
      /dev/sdb   HPCSA  lvm2 a--  <20.00g 13.99g
      /dev/sdc   HPCSA  lvm2 a--  <20.00g 18.99g
    
