 * Step 1
    * Make 4 VM machines with 1GB RAM, and Only 1 Core
    * First 3 machines hostname is
    *                                  * node1.hpcsa.in
      		                           * node2.hpcsa.in
    		                           * node3.hpcsa.in
    * Last machines hostname is
    *                                * client.hpcsa.in
    
    * Add one Extra hard-disk in 3 Node machines of 20 GB.


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
* # Create and start a Gluster volume: This command creates a Gluster volume named 'gdisk1' with a replication factor of 3. It then starts the volume:
* Below command is run only on Node 1 Machine
*                         [root@node1 ~]# gluster volume create gdisk2 replica 3 node1.hpcsa.in:/mnt/disk1/diskvol/gdisk2  node2.hpcsa.in:/mnt/disk1/diskvol/gdisk2                                
                                       node3.hpcsa.in:/mnt/disk1/diskvol/gdisk2
	                     volume create: gdisk2: success: please start the volume to access data

*                        [root@node1 ~]# gluster
                           gluster               glusterd              glusterfind           glusterfs             glusterfsd            gluster-setgfid2path
* gluster volume start gdisk2   , this command will start the gdisk.
*                         [root@node1 ~]# gluster volume start gdisk2
                           volume start: gdisk2: success
                      
* # Display information about the Gluster volume: This command displays information about the 'gdisk1' volume. Here, you can see in the type of glusterfs
   
   
                           gluster volume info gdisk1
		           [root@node1 ~]# gluster volume info gdisk2
						Volume Name: gdisk2
						Type: Replicate
						Volume ID: 1c9b9440-9896-47ad-8f41-6a81c23b59fe
						Status: Started
						Snapshot Count: 0
						Number of Bricks: 1 x 3 = 3
						Transport-type: tcp
						Bricks:
						Brick1: node1.hpcsa.in:/mnt/disk1/diskvol/gdisk2
						Brick2: node2.hpcsa.in:/mnt/disk1/diskvol/gdisk2
						Brick3: node3.hpcsa.in:/mnt/disk1/diskvol/gdisk2
						Options Reconfigured:
						cluster.granular-entry-heal: on
						storage.fips-mode-rchecksum: on
						transport.address-family: inet
						nfs.disable: on
						performance.client-io-threads: off
  # ON CLIENT MACHINE
  * On the client, install glusterfs-fuse package: This package allows GlusterFS volumes to be mounted via fuse:
  * 				[root@client ~]#  systemctl disable firewalld
							Removed symlink /etc/systemd/system/multi-user.target.want                                                             s/firewalld.service.
							Removed symlink /etc/systemd/system/dbus-org.fedoraproject                                                             .FirewallD1.service.
				     [root@client ~]#  systemctl stop firewalld
				     [root@client ~]# setenforce 0
*					root@client ~]# yum install glusterfs-fuse
* Create a directory for mounting the Gluster volume:
   
  					mkdir /mnt/gdrive
* Mount the Gluster volume on the client: This command mounts the 'gdisk1' volume to the newly created directory:
  					mount -t glusterfs node1.hpcsa.in:/gdisk2 /mnt/gdrive
* 					[root@client ~]# df -h
								Filesystem               Size  Used Avail Use% Mounted on
								devtmpfs                 475M     0  475M   0% /dev
								tmpfs                    487M     0  487M   0% /dev/shm
								tmpfs                    487M  7.6M  479M   2% /run
								tmpfs                    487M     0  487M   0% /sys/fs/cgroup
								/dev/mapper/centos-root   17G  1.5G   16G   9% /
								/dev/sda1               1014M  138M  877M  14% /boot
								tmpfs                     98M     0   98M   0% /run/user/0
								node1.hpcsa.in:/gdisk2    20G  238M   20G   2% /mnt/gdrive
* Create an empty file to replicate it in all server
* 					       [root@client ~]# dd if=/dev/zero of=file.data bs=1024 count=15
						15+0 records in
						15+0 records out
						15360 bytes (15 kB) copied, 0.000106289 s, 145 MB/s

# Disperse & Redundancy in GlusterFs
* The redundancy value is used in the same way as for a dispersed volume.
* The disperse count is simply the sum of the data bricks and redundancy bricks.
# Do the Following Commands on Node 1 Only, which is our main server.
* You just need to change gdisk number and run below commands.

* 		gluster volume create gdisk3 disperse 3 redundancy 1 node1.hpcsa.in:/mnt/disk1/diskvol/gdisk3  node2.hpcsa.in:/mnt/disk1/diskvol/gdisk3  node3.hpcsa.in:/mnt/disk1/diskvol/gdisk3
*		[root@node1 ~]#  gluster volume create gdisk3 disperse 3 redundancy 1 node1.hpcsa.in:/mnt/disk1/diskvol/gdisk3  node2.hpcsa.in:/mnt/disk1/diskvol/gdisk3  		 
				node3.hpcsa.in:/mnt/disk1/diskvol/gdisk3
		volume create: gdisk3: success: please start the volume to access data
*  gluster volume info gdisk3 (you can check type of glusterfs)
* 		[root@node1 ~]# gluster volume info gdisk3

				Volume Name: gdisk3
				Type: Disperse
				Volume ID: 3d902fb0-e4be-4fc1-a681-d704e69809de
				Status: Created
				Snapshot Count: 0
				Number of Bricks: 1 x (2 + 1) = 3
				Transport-type: tcp
				Bricks:
				Brick1: node1.hpcsa.in:/mnt/disk1/diskvol/gdisk3
				Brick2: node2.hpcsa.in:/mnt/disk1/diskvol/gdisk3
				Brick3: node3.hpcsa.in:/mnt/disk1/diskvol/gdisk3
				Options Reconfigured:
				storage.fips-mode-rchecksum: on
				transport.address-family: inet=
				nfs.disable: on
  # Run following commands on client machine
* You need to create new drive, mount that drive.
* 		[root@client ~]# mkdir /mnt/gdrive5

		[root@client ~]# mount -t glusterfs node1.hpcsa.in:/gdisk5 /mnt/gdrive5

		[root@client ~]# cd /mnt/gdrive5
* Create empty file to dispers the file within nodes.
* 		[root@client gdrive5]# dd if=/dev/zero of=file1.data bs=1024 count=204652
					146986+0 records in
					146986+0 records out
					150513664 bytes (151 MB) copied, 5.66048 s, 26.6 MB/s
*		[root@client gdrive5]# ll
				total 2046
				-rw-r--r--. 1 root root 2095104 Jul 15 22:39 file1.data
				[root@client gdrive5]# df -h
				Filesystem               Size  Used Avail Use% Mounted on
				devtmpfs                 475M     0  475M   0% /dev
				tmpfs                    487M     0  487M   0% /dev/shm
				tmpfs                    487M  7.7M  479M   2% /run
				tmpfs                    487M     0  487M   0% /sys/fs/cgroup
				/dev/mapper/centos-root   17G  1.5G   16G   9% /
				/dev/sda1               1014M  138M  877M  14% /boot
				tmpfs                     98M     0   98M   0% /run/user/0
				node1.hpcsa.in:/gdisk2    20G  239M   20G   2% /mnt/gdrive
				node1.hpcsa.in:/gdisk5    40G  478M   40G   2% /mnt/gdrive5
		[root@client gdrive5]# ll -h
				total 2.0M
				-rw-r--r--. 1 root root 2.0M Jul 15 22:39 file1.data
* You will check on Node 1, 2, 3 that file is disperse with parity.
* 		ON NODE 1
				[root@node1 ~]# cd /mnt/disk1/diskvol/gdisk5
				[root@node1 gdisk5]# ll -h
				total 1.1M
				-rw-r--r--. 2 root root 1023K Jul 15 22:39 file1.data
*               ON NODE 2
				[root@node2 ~]# cd /mnt/disk1/diskvol/gdisk5
				[root@node2 gdisk5]# ll
				total 1028
				-rw-r--r--. 2 root root 1047552 Jul 15 22:39 file1.data
				[root@node2 gdisk5]# ll -h
				total 1.1M
				-rw-r--r--. 2 root root 1023K Jul 15 22:39 file1.data
* 		ON NODE 3

		                [root@node3 ~]# cd /mnt/disk1/diskvol/gdisk5
		                [root@node3 gdisk5]# ll -h
		                total 1.1M
		                -rw-r--r--. 2 root root 1023K Jul 15 22:39 file1.data
		
		 		

 
