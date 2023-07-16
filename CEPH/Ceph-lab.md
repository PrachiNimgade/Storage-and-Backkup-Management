* Make 5 Vm machines of Centos
* Set hostname: monitor
                controller
                compute-0
                compute-01
                client
* Systemctl stop firewalld
* Systemctl disabled firewalld
* vim /etc/selinux/config' = enforcing= disabled
* Set all hosts in vim /etc/hosts file
* Add 3 hard-disk in compute0 and compute01 machine.
* If we run lsblk command we can see.....sdb, sdc, sdd

  Compute-0:
  ![1](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/7eabbbcd-19df-454c-934f-2a3fdd43441b)

  Compute-01:
  ![2](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/a3241e02-6e4a-4d6c-91d6-d003aebd9b36)
  
