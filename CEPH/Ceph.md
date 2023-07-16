
* CEPH is defined as hardware defined storage.
* It is a propritory based.
* No to required RAID card, with no specific size of disk.
* Need same speed of disk.
* It is controlled by the software.
* Objects based storage. 
* Highly distributed.
* The Ceph Storage Cluster is the foundation for all Ceph deployments. Based upon RADOS , Ceph Storage Clusters consist of several types of daemons.
* RADOS:- Reliable Autonomic Distributed Object Store
* OSD : Object storage deamon, ceph-osd is the object storage daemon for the Ceph distributed file system. It is responsible for storing objects on a local file system and providing access to them over the network.
* CRUSH  ensure that kis host me or osd me konsa pool jayega tat is decided by the CRUSH.
*  RADOS is bridgd between iscsi, nfs, ceph
*  Object storage: S3, Swift
*  Block Storage: iSCSI
*  File Storage: NFS
