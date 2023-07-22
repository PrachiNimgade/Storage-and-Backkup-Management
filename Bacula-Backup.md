
yum install -y bacula-director bacula-storage bacula-console bacula-client mariadb-server nano httpd

systemctl enable mariadb && systemctl start mariadb && systemctl status mariadb

/usr/libexec/bacula/grant_mysql_privileges

![bacula1](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/5345d9ce-718e-42c6-9e90-992e32fa89b0)


/usr/libexec/bacula/create_mysql_database -u root

          [root@bacula ~]# /usr/libexec/bacula/create_mysql_database -u root
          Creation of bacula database succeeded.

/usr/libexec/bacula/make_mysql_tables -u bacula
          [root@bacula ~]# /usr/libexec/bacula/make_mysql_tables -u bacula
          Creation of Bacula MySQL tables succeeded.


mysql_secure_installation


![bacula2](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/6afa7ee7-8ca7-4823-b2b7-ea1f3bc8c461)

alternatives --config libbaccats.so
        select mysql

![bacula3](https://github.com/PrachiNimgade/Storage-and-Backkup-Management/assets/113961419/a127c96c-7eb1-4635-ba47-7d155608e297)

mkdir -p /bacula/backup /bacula/restore

chown -R bacula:bacula /bacula
chmod -R 700 /bacula

add host file `/etc/hosts`


