* Create 2 Vm one is server and client.
* You add one hard -disk which will be `sdb`
* Do basic Configuration of below.
  
            systemctl stop firewalld
            systemctl disable firewalld
            setselinux 0
            vim /etc/selinux/config
                      =disabled
  
* Add hosts on `vim /etc/hosts` file.
* Whenever you want to add disk first you have to power off the machine and then add.

![Screenshot 2023-07-22 103534](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/3d702606-51a4-4b2b-a32a-c33a7bae6cf0)

# On Master Machine

 * `systemctl stop firewalld`
 * `systemctl disable firewalld`
 * `hostnamectl set-hostname master`
 * `yum install vim -y`
 * `vim /etc/selinux/config`
 * `reboot`
 * `ifup ens33`
 * `ip a`
 * `lsblk`
 * Remove disk with `fdisk` and make partition 1
   
      fdisk /dev/sdb
      mkfs.ext4 /dev/sdb1
      mkdir /mnt/disk1
      mount /dev/sdb1 /mnt/disk1
   
 * `df -Th`
   
 * `yum install vim wget -y`

 * Download BeeGFS Repository:
    To start with, we will download the BeeGFS repository to our system:

 `wget --no-check-certificate -O /etc/yum.repos.d/beegfs.repo https://www.beegfs.io/release/beegfs_7.2/dists/beegfs-rhel7.repo`


* Install BeeGFS packages with the following commands:


  `yum install beegfs-utils beegfs-client beegfs-meta beegfs-storage beegfs-mgmtd`


* Configure BeeGFS:
    Now, we will configure the metadata server (MDS), storage server (SS), and client. You will need to replace `<MDS IP>`, `<SS IP>`, and `<client IP>` with your server vm IPs.


  `mkdir -p /mnt/beegfs/meta && mkdir -p /mnt/beegfs/storage && mkdir -p /mnt/beegfs/beegfs_mgmtd`


   `/opt/beegfs/sbin/beegfs-setup-mgmtd -p /mnt/beegfs/beegfs_mgmtd`


* open this file `vim /etc/beegfs/beegfs-client.conf`

  
	add entry --> sysMgmtdHost = "your server VM IP"


           vim /etc/beegfs/beegfs-client.conf
		       sysMgmtdHost = "your server VM IP"


  `systemctl start beegfs-mgmtd`
  `systemctl enable beegfs-mgmtd`
  `systemctl status beegfs-mgmtd`


   `/opt/beegfs/sbin/beegfs-setup-meta -p /mnt/beegfs/meta -s 1 -m 192.168.206.164` (server ip)


   `systemctl start beegfs-meta`
   `systemctl enable beegfs-meta`
   `systemctl status beegfs-meta`



$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

# ON Client Machine

`hostnamectl set-hostname client`

`systemctl stop firewalld`

`systemctl disable firewalld`

`yum install vim -y`
    
 `ifup ens33`
 
 `yum install vim -y`
 
 `vim /etc/selinux/config`
 
 `vim /etc/hosts`
 
  `init 6`

* Download BeeGFS Repository:
    To start with, we will download the BeeGFS repository to our system:

 `yum install vim wget`
 
  `wget --no-check-certificate -O /etc/yum.repos.d/beegfs.repo https://www.beegfs.io/release/beegfs_7.2/dists/beegfs-rhel7.repo`
  
  `yum install -y kernel-devel beegfs-utils beegfs-client beegfs-mgmtd`
  
  `/opt/beegfs/sbin/beegfs-setup-client -m 192.168.206.164` (server ip)
				
				            [root@client ~]# /opt/beegfs/sbin/beegfs-setup-client -m 192.168.206.164
						        Updating config file: /etc/beegfs/beegfs-client.conf
 						        * Setting management host: 192.168.206.164
						        All done.
              
`vim /etc/beegfs/beegfs-client.conf`  here, we can see already server ip is set.

`systemctl start beegfs-client`  Due to some bugs beegfs-client service can't be start.

			




