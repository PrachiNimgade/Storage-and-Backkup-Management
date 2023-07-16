 * Step 1
    * Make 4 VM machines with 1GB RAM, and Only 1 Core
    * First 3 machines hostname is
    *                                  * node1.hpcsa.in
      		                           * node2.hpcsa.in
    		                           * node3.hpcsa.in
    * Last machines hostname is
    *                                * client.hpcsa.in
    
    * Add one Extra hard-disk in 3 machines.


 * Step 2 ON NODE 1, 2, 3

                        [root@node1 ~]# systemctl stop firewalld
                        [root@node1 ~]# systemctl disable firewalld
                        [root@node1 ~]# setenforce 0
                        [root@node1 ~]# lsblk
                                        NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
                                        sda           8:0    0   20G  0 disk
                                        ├─sda1        8:1    0    1G  0 part /boot
                                        └─sda2        8:2    0   19G  0 part
                                          ├─centos-root
                                                    253:0    0   17G  0 lvm  /
                                          └─centos-swap
                                                    253:1    0    2G  0 lvm  [SWAP]
                                        sdb           8:16   0   20G  0 disk
                                        sr0          11:0    1  973M  0 rom
* In above command when we add extra disk in machine so we get sdb hard-drive, we have to  erase that disk.

                          [root@node1 ~]# fdisk /dev/sdb
                                                  Welcome to fdisk (util-linux 2.23.2).
                                                  
                                                  Changes will remain in memory only, until you decide to write them.
                                                  Be careful before using the write command.
                                                  
                                                  Device does not contain a recognized partition table
                                                  Building a new DOS disklabel with disk identifier 0xc932b878.
                                                  
                                                  Command (m for help): n
                                                  Partition type:
                                                     p   primary (0 primary, 0 extended, 4 free)
                                                     e   extended
                                                  Select (default p): p
                                                  Partition number (1-4, default 1):
                                                  First sector (2048-41943039, default 2048):
                                                  Using default value 2048
                                                  Last sector, +sectors or +size{K,M,G} (2048-41943039, default 41943039):
                                                  Using default value 41943039
                                                  Partition 1 of type Linux and of size 20 GiB is set
                                                  
                                                  Command (m for help): w
                                                  The partition table has been altered!
                                                  
                                                  Calling ioctl() to re-read partition table.
                                                  Syncing disks.
  
* Format in XFS and create a filesystem on the new hard disk: To do this, you will have to partition the hard disk, format it with a file syste then mount it.


      
                       [root@node1 ~]# mkfs.xfs /dev/sdb1
                                                  meta-data=/dev/sdb1              isize=512    agcount=4, agsize=1310656 blks
                                                           =                       sectsz=512   attr=2, projid32bit=1
                                                           =                       crc=1        finobt=0, sparse=0
                                                  data     =                       bsize=4096   blocks=5242624, imaxpct=25
                                                           =                       sunit=0      swidth=0 blks
                                                  naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
                                                  log      =internal log           bsize=4096   blocks=2560, version=2
                                                           =                       sectsz=512   sunit=0 blks, lazy-count=1
                                                  realtime =none                   extsz=4096   blocks=0, rtextents=0
* mkdir /mnt/disk1
        
* mount /dev/sdb1 /mnt/disk1

* yum install wget centos-release-gluster epel-release glusterfs-server -y
* systemctl start glusterd
* systemctl enable glusterd
* This peer commands has to be run only on node1
*                     gluster peer probe node2.hpcsa.in  
                      gluster peer probe node3.hpcsa.in
* You can check nodes are connected or not on each node.
*                     gluster peer status  
                      gluster pool list
                                 [root@node1 ~]# gluster pool list
                                                         UUID                                    Hostnam                                                                                                                                                                                              e       State
                                                         20b22352-b3b3-4e93-a8c7-9c9961f5f977    node2.h                                                                                                                                                                                              pcsa.in Connected
                                                         8fa9f26f-a498-469a-a186-fd1592fb6450    node3.h                                                                                                                                                                                              pcsa.in Connected
                                                         93df272f-30e5-4503-a63b-4e0fea7d6249    localho                                                                                                                                                                                              st      Connected
* Run Below command on all nodes.
*                      mkdir /mnt/disk1/diskvol
* Create and start a Gluster volume: This command creates a Gluster volume named 'gdisk1' with a replication factor of 3. It then starts the volume:
*                      [root@node1 ~]# gluster volume create gdisk2 replica 3 node1.hpcsa.in:/mnt/disk1/diskvol/gdisk2  node2.hpcsa.in:/mnt/disk1/diskvol/gdisk2                                
                                       node3.hpcsa.in:/mnt/disk1/diskvol/gdisk2
	                     volume create: gdisk2: success: please start the volume to access data

*                        [root@node1 ~]# gluster
                           gluster               glusterd              glusterfind           glusterfs             glusterfsd            gluster-setgfid2path
* gluster volume start gdisk2   , this command will start the gdisk.
*                         [root@node1 ~]# gluster volume start gdisk2
                           volume start: gdisk2: success
                      
* Display information about the Gluster volume: This command displays information about the 'gdisk1' volume:
   
                           gluster volume info gdisk1
   
