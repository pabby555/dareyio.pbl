# Project 6
## Title: WEB SOLUTION WITH WORD PRESS
### Description: 
This Project describes implementing web solution with word press in AWS.

Web solutions are usually implemented with a three-tier architecture.

The three-tier architecture is a client-server software architecture with 3 separate layers:


* Presentation tier - This is the user interface such as a client server or browser.
* Application tier - This is the business layer. The backend program that implements business logic. it can be an application server or a web server.
* Data tier - This layer takes care of data storage and access. it can be a database server or a File system server such as an NFS server or an FTP server.

### Implementation Steps:
* STEP 1 - Prepare a web server with an EC2 instance runnung Red Hat Operating system, create three 10G volumes and add to instance 
![AWS EC2 instance creation](./images6/create-redhat-ec2-instance.png)

![AWS EC2 instance creation](./images6/add-storage.png)

![AWS EC2 instance creation](./images6/create-securitygroup.png)

![AWS EC2 instance creation](./images6/choose-keypair.png)

![AWS EC2 instance creation](./images6/connect-to-ec2-instance.1.png)

  * Connect to EC2 instance via Windows terminal

  ![Connecting to AWS EC2 instance](./images6/connect-to-ec2-instance.2.png)

  ![Connecting to AWS EC2 instance](./images6/connect-to-ec2-instance.3.png)

  * Use "lsblk" command to inspect block devices attached to the server and confirm the three newly created block devices are present.
<!-- Code Blocks -->
```bash
$ lsblk
```
  ![using lsblk command](./images6/lsblk.png)


  * Use "df -h" command to see all mount points and free spaces on the server
  <!-- Code Blocks -->
```bash
$ df -h
```
  ![using df -h command](./images6/df-h.png)

  * Use "gdisk" utility to create a single partition on each of the 3 disks
  <!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdb
```
  ![using gdisk command](./images6/gdisk-1.png)

![using gdisk command](./images6/gdisk-2.png)

 <!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdc
```
  ![using gdisk command](./images6/gdisk-3.png)

![using gdisk command](./images6/gdisk-4.png)

<!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdd
```
  ![using gdisk command](./images6/gdisk-5.png)

![using gdisk command](./images6/gdisk-6.png)

  * Use the "lsblk" utility to view new partitions on the three disks.
  <!-- Code Blocks -->
```bash
$ lsblk
```
  ![using lsblk command](./images6/verifying-with-lsblk.png)

  * Install the "lvm2" package and use the command "sudo lvmdiskscan" to check for available partitions.
  <!-- Code Blocks -->
```bash
$ sudo dnf install lvm2
```
  ![installing lvm2 package](./images6/lvm2-install.png)

  * Use the "pvcreate" utility to mark each of the three disks as physical volumes.
   <!-- Code Blocks -->
```bash
$ sudo pvcreate /dev/xvdb1
$ sudo pvcreate /dev/xvdc1
$ sudo pvcreate /dev/xvdd1
```
  ![using pvcreate utility](./images6/pvcreate.png)

  * Use the command "sudo pvs" to verify that your physical volume has been created successfully.
  <!-- Code Blocks -->
```bash
$ sudo pvs
```
  ![using pvcreate utility](./images6/pvs.png)

  * Use the "vgcreate" utility to add all 3 PVs to a volume group named "webdata-vg"
  <!-- Code Blocks -->
```bash
$ sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
  ![using vgcreate utility](./images6/vgcreate.png)

  * Use the command "sudo vgs" to verify that your volume group has been created successfully.
<!-- Code Blocks -->
```bash
$ sudo vgs
```
  ![using vgcreate utility](./images6/vgs.png)

  * Use the "lvcreate" utility to create two logical volumes named "apps-lv" [will be used to store data for the website] and "logs-lv" [will be used to store data for logs]
<!-- Code Blocks -->
```bash
$ sudo lvcreate -n apps-lv -L 14G webdata-vg
$ sudo lvcreate -n logs-lv -L 14G webdata-vg
```
  ![using lvcreate utility](./images6/lvcreate.png)

  * Use the command "sudo lvs" to verify that the logical volumes has been created successfully.
  <!-- Code Blocks -->
```bash
$ sudo lvs
```
  ![using lvs utility](./images6/lvs.png)

  * Use the command "sudo vgdisplay" to view complete setup.
  <!-- Code Blocks -->
```bash
$ sudo lvdisplay
```
  ![using lvdisplay utility](./images6/lvdisplay.png)

  * Format the logical volumes withext4 filesystem using the "mkfs.ext4" utility
   <!-- Code Blocks -->
```bash
$ sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
$ sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
  ![using mkfs utility](./images6/mkfs.png)

  * Create the directory "/var/www/html" to store website files
<!-- Code Blocks -->
```bash
$ sudo mkdir -p /var/www/html
```
  ![creating /var/www/html directory](./images6/creating-html-directory.png)


  * Create the directory "/home/recovery/logs" to store backup of log data.
  <!-- Code Blocks -->
```bash
$ sudo mkdir -p /home/recovery/logs
```
  ![creating /home/recovery/logs directory](./images6/creating-recovery-logs-directory.png)

  * Mount "/var/www/html" on apps-lv logical volume
<!-- Code Blocks -->
```bash
  $ sudo mount /dev/webdata-vg/apps-lv /var/www/html/
  ```
  ![creating /home/recovery/logs directory](./images6/mounting-apps-lv.png)

  * Use the "rsync" utility to backup all the files in the log directory /var/log/ into /home/recovery/logs
  <!-- Code Blocks -->
```bash
  $ sudo rsync -av /var/log/. /home/recovery/logs/
  ```
  ![Using rsync utility](./images6/rsync-1.png)

  ![Using rsync utility](./images6/rsync-2.png)


  * Mount /var/log on logs-lv logical volume.
   <!-- Code Blocks -->
```bash
  $ sudo mount /dev/webdata-vg/logs-lv /var/log
  ```
  ![Using mount utility](./images6/mounting-logs-lv.png)

   * Restore log files back into /var/log directory
     <!-- Code Blocks -->
```bash
  $ sudo rsync -av /home/recovery/logs/. /var/log
  ```
  ![Using rsync utility](./images6/rsync-3.png)

  ![Using rsync utility](./images6/rsync-4.png)

  * Update /etc/fstab file for persistence of mount configuration after restart of the server
  <!-- Code Blocks -->
```bash
  $ sudo blkid
  ```
  ![Using blkid utility](./images6/blkid.png)

![Using blkid utility](./images6/blkid-2.png)

![updating /etc/fstab file](./images6/fstab.png)
<!-- Horizontal Rule -->
----------------------------------------------------

* STEP 2 - Prepare the Database server with an EC2 instance runnung Red Hat Operating system, create three 10G volumes and add to instance 
![AWS EC2 instance creation](./images6/create-redhat-ec2-instance.png)

![AWS EC2 instance creation](./images6/add-storage.png)

![AWS EC2 instance creation](./images6/security-group-config-dbserver.png)

![AWS EC2 instance creation](./images6/choose-keypair.png)

![AWS EC2 instance creation](./images6/ec2-connect-dbserver.png)

  * Connect to EC2 instance via Windows terminal

  ![Connecting to AWS EC2 instance](./images6/ssh-connect-dbserver.png)

  ![Connecting to AWS EC2 instance](./images6/windows-term-connect.png)

  * Use "lsblk" command to inspect block devices attached to the server and confirm the three newly created block devices are present.

<!-- Code Blocks -->
```bash
$ lsblk
```
 ![using lsblk command](./images6/lsblk-2.png)


  * Use "df -h" command to see all mount points and free spaces on the server
  <!-- Code Blocks -->
```bash
$ df -h
```
  ![using df -h command](./images6/db-mountpoint-check.png)

  * Use "gdisk" utility to create a single partition on each of the 3 disks
  <!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdb
```
  ![using gdisk command](./images6/db-newpartition-1.png)

![using gdisk command](./images6/db-newpartition-2.png)

 <!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdc
```
  ![using gdisk command](./images6/db-newpartition-3.png)

![using gdisk command](./images6/db-newpartition-4.png)

<!-- Code Blocks -->
```bash
$ sudo gdisk /dev/xvdd
```
  ![using gdisk command](./images6/db-newpartition-5.png)

  * Use the "lsblk" utility to view new partitions on the three disks.
  <!-- Code Blocks -->
```bash
$ lsblk
```
  ![using lsblk command](./images6/db-lsblk.png)

  * Install the "lvm2" package and use the command "sudo lvmdiskscan" to check for available partitions.
  <!-- Code Blocks -->
```bash
$ sudo dnf install lvm2
```
  ![installing lvm2 package](./images6/db-lvm2-install.png)

* Use the "pvcreate" utility to mark each of the three disks as physical volumes.
   <!-- Code Blocks -->
```bash
$ sudo pvcreate /dev/xvdb1
$ sudo pvcreate /dev/xvdc1
$ sudo pvcreate /dev/xvdd1
```
  ![using pvcreate utility](./images6/db-pvcreate.png)

  * Use the command "sudo pvs" to verify that your physical volume has been created successfully.
  <!-- Code Blocks -->
```bash
$ sudo pvs
```
  ![using pvcreate utility](./images6/db-pvs.png)

  * Use the "vgcreate" utility to add all 3 PVs to a volume group named "webdata-vg"
  <!-- Code Blocks -->
```bash
$ sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
  ![using vgcreate utility](./images6/db-vgcreate.png)

  * Use the command "sudo vgs" to verify that your volume group has been created successfully.
<!-- Code Blocks -->
```bash
$ sudo vgs
```
  ![using vgcreate utility](./images6/db-vgs.png)

  * Use the "lvcreate" utility to create two logical volumes named "db-lv" [will be used to store data for the website] and "logs-lv" [will be used to store data for logs]
<!-- Code Blocks -->
```bash
$ sudo lvcreate -n db-lv -L 14G webdata-vg
$ sudo lvcreate -n logs-lv -L 14G webdata-vg
```
  ![using lvcreate utility](./images6/db-lvcreate.png)

  * Use the command "sudo lvs" to verify that the logical volumes has been created successfully.
  <!-- Code Blocks -->
```bash
$ sudo lvs
```
  ![using lvs utility](./images6/db-lvs.png)

  * Use the command "sudo vgdisplay" to view complete setup.
  <!-- Code Blocks -->
```bash
$ sudo lvdisplay
```
  ![using lvdisplay utility](./images6/db-lvdisplay.png)

  * Format the logical volumes with ext4 filesystem using the "mkfs.ext4" utility
   <!-- Code Blocks -->
```bash
$ sudo mkfs -t ext4 /dev/webdata-vg/db-lv
$ sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
  ![using mkfs utility](./images6/db-mkfs.png)

  * Create the directory "/db" to store website files
<!-- Code Blocks -->
```bash
$ sudo mkdir /db
```
  ![creating /db directory](./images6/db-directory.png)


  * Create the directory "/home/recovery/logs" to store backup of log data
  <!-- Code Blocks -->
```bash
$ sudo mkdir -p /home/recovery/logs
```
  ![creating /home/recovery/logs directory](./images6/db-logs-directory.png)

  * Mount "/db" on apps-lv logical volume
<!-- Code Blocks -->
```bash
  $ sudo mount /dev/webdata-vg/db-lv /db
  ```
  ![creating /home/recovery/logs directory](./images6/db-mount-directory.png)

  * Use the "rsync" utility to backup all the files in the log directory /var/log/ into /home/recovery/logs
  <!-- Code Blocks -->
```bash
  $ sudo rsync -av /var/log/. /home/recovery/logs/
  ```
  ![Using rsync utility](./images6/db-rsync.png)

  * Mount /var/log on logs-lv logical volume.
   <!-- Code Blocks -->
```bash
  $ sudo mount /dev/webdata-vg/logs-lv /var/log
  ```
  ![Using mount utility](./images6/db-mount-log.png)

   * Restore log files back into /var/log directory
     <!-- Code Blocks -->
```bash
  $ sudo rsync -av /home/recovery/logs/. /var/log
  ```
  ![Using rsync utility](./images6/db-rsync-log.png)

  * Update /etc/fstab file for persistence of mount configuration after restart of the server
  <!-- Code Blocks -->
```bash
  $ sudo blkid
  ```
  ![Using blkid utility](./images6/db-blkid.png)

![updating /etc/fstab file](./images6/db-fstab.png)

<!-- Horizontal Rule -->
----------------------------------------------------

* STEP 3 - Install WordPress on your Web Server EC2 Instance

 * Install wget, Apache and its dependencies
  <!-- Code Blocks -->
```bash
  $ sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
  ```
  ![Apache and dependencies Install](./images6/web-apache-install.png)

  * Start Apache Service
  <!-- Code Blocks -->
```bash
  $ sudo systemctl start httpd
  $ sudo systemctl enable httpd
  ```
  ![Apache service](./images6/web-apache-service.png)

  * Install PHP and its dependencies
  <!-- Code Blocks -->
```bash
  $ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

  $ sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

  $ sudo yum module list php

  $ sudo yum module reset php

  $ sudo yum module enable php:remi-7.4

  $ sudo yum install php php-opcache php-gd php-curl php-mysqlnd

  ```
  ![PHP Install](./images6/web-php-install-1.png)

![PHP Install](./images6/web-php-install-2.png)

![PHP Install](./images6/web-php-install-3.png)

![PHP Install](./images6/web-php-install-4.png)

![PHP Install](./images6/web-php-install-5.png)

![PHP Install](./images6/web-php-install-6.png)

![PHP Install](./images6/web-php-install-7.png)

* Start PHP Service, 
  <!-- Code Blocks -->
```bash
  $ sudo systemctl start php-fpm

  $ sudo systemctl enable php-fpm

  $ sudo setsebool -P httpd_execmem 1

  ```
  ![Apache service](./images6/web-php-service.png)

* Configure SElinux boolean and restart Apache Service
 <!-- Code Blocks -->
```bash
$ sudo setsebool -P httpd_execmem 1

$ sudo systemctl restart httpd
```

![restart Apache service](./images6/web-apache-restart.png)

* Download wordpress and copy wordpress to /var/www/html
 <!-- Code Blocks -->
```bash
$ mkdir wordpress

$ cd wordpress

$ sudo wget http://wordpress.org/latest.tar.gz

$ sudo tar xzvf latest.tar.gz

$ sudo rm -rf latest.tar.gz

$ sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php

$ sudo cp -R wordpress /var/www/html/
```

![Wordpress config](./images6/web-wordpress-config-1.png)

![Wordpress config](./images6/web-wordpress-config-2.png)

![Wordpress config](./images6/web-wordpress-config-4.png)

* Configure SELinux Policies
 <!-- Code Blocks -->
```bash
$ sudo chown -R apache:apache /var/www/html/wordpress

$ sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R

$ sudo systemctl restart httpd
```
![Wordpress config](./images6/web-wordpress-config-5.png)

<!-- Horizontal Rule -->
----------------------------------------------------

* STEP 4 - Install MySQL on your DB Server EC2 Instance

* Install mysql-server package and ensure mysqld service is started and ensured.
 <!-- Code Blocks -->
```bash
$ sudo yum install mysql-server

$ sudo systemctl start mysqld

$ sudo systemctl enable mysqld

$ sudo systemctl status mysqld
```
![Wordpress config](./images6/db-mysql-install.png)

![Wordpress config](./images6/db-mysql-service.png)

<!-- Horizontal Rule -->
----------------------------------------------------


* STEP 5 - Configure DB to work with WordPress. Web Server IP Address (AWS Private IP Address) is used to create user and grant privileges.

<!-- Code Blocks -->
```bash
$ sudo mysql

mysql> CREATE DATABASE wordpress;

mysql> CREATE USER `apache`@`172.31.80.153` IDENTIFIED BY 'password';

mysql> GRANT ALL ON wordpress.* TO 'apache'@'172.31.80.153';

mysql> FLUSH PRIVILEGES;

mysql> SHOW DATABASES;

mysql> exit
```
![Wordpress config](./images6/wordpress-access.png)

<!-- Horizontal Rule -->
----------------------------------------------------

* STEP 6 - Configure WordPress to connect to remote database.

      Open MySQL port 3306 on DB Server EC2.

      Allow access to the DB server from only the Web Server`s IP address for extra security. 

      Inbound Rule configuration should specify /32.

![Wordpress config](./images6/db-security-rules.png)

* Install mysql client on Web Server and test that you can connect from the web server to the DB Server.

<!-- Code Blocks -->
```bash
$ sudo yum install mysql

$ sudo mysql -u apache -p -h 172.31.82.220     (this is DB Private IP Address)
```
![Wordpress config](./images6/web-mysql-install.png)

![Wordpress config](./images6/db-access.png)

* Change permissions and configuration so Apache could use WordPress: Edit the file wp-config.php to modify the values to correspond to the database hostname, database user and password.

<!-- Code Blocks -->
```bash
$ sudo vi /var/www/html/wordpress/wp-config.php
```
![Wordpress config](./images6/web-wordpress-config-connect.png)

* Change permission for wordpress directory under /var/www/html

<!-- Code Blocks -->
```bash
$ sudo chmod -Rf 775 /
```
![Wordpress config](./images6/wordpress-directory.png)

* Create Apache WordPress VirtualHost File.

<!-- Code Blocks -->
```bash
$ sudo vi /etc/httpd/conf.d/wordpress.conf
```
![Wordpress config](./images6/wordpress-virtualhost.png)

* Restart Apache

<!-- Code Blocks -->
```bash
$ sudo systemctl restart httpd
```
![Wordpress config](./images6/wordpress-restart-httpd.png)


* Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 

![Wordpress config](./images6/web-security-rule.png)


* Test access from browser to WordPress http://<Web-Server-Public-IP-Address><184.73.144.139>/wordpress/

![Wordpress config](./images6/wordpress-access-1.png)

![Wordpress config](./images6/wordpress-access-2.png)

