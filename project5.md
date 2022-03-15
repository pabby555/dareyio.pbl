# Project 5
## Title: CLIENT/SERVER ARCHITECTURE USING MYSQL RDMS
### Description: 
This Project demonstrates how to implement a basic client-server architecture with MySQL database management system. 

<!-- Horizontal Rule -->
----------------------------------------------------

1. STEP ONE: Create and configure two Linux-based virtual servers with AWS Cloud.

* mysql server
* mysql client

<!-- Horizontal Rule -->
----------------------------------------------------

2. STEP TWO: On the mysql server, install MySQL server software

<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository
![package update](./images5/mysql-server-update.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Upgrading package repository
![package upgrade](./images5/mysql-server-upgrade.png)

<!-- Code Blocks -->
```bash
$ sudo apt install mysql-server
```
Installing mysql-server package
![package upgrade](./images5/mysql-server-install-1.png)

![package upgrade](./images5/mysql-server-install-2.png)

![package upgrade](./images5/mysql-server-install-3.png)

<!-- Code Blocks -->
```bash
$ sudo mysql_secure_installation
```
Configuring MySQL
![secure installation](./images5/mysql-secure-install-1.png)

![package upgrade](./images5/mysql-secure-install-4.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl status mysql
```
Checking the status of mysql service
![mysql service status](./images5/mysql-service-status.png)

<!-- Code Blocks -->
```bash
$ sudo mysql
```
Log into mysql
![login verification](./images5/mysql-login-verification.png)

<!-- Horizontal Rule -->
----------------------------------------------------

3. STEP THREE: On the mysql Client, install MySQL Client software

<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository in mysql-client
![package update in mysql client](./images5/mysql-client-update.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Updating package repository in mysql-client
![package update in mysql client](./images5/mysql-client-upgrade.png)

<!-- Code Blocks -->
```bash
$ sudo apt-get install mysql-client

$ mysql -V
```
Installing mysql-client package
![Installing mysql client](./images5/mysql-client-install.png)

![Verifying mysql client install](./images5/mysql-client-install-verification.png)

<!-- Code Blocks -->
```bash
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

Replace ‘127.0.0.1’ to ‘0.0.0.0’

```
Configuring mysql server to allow connections from remote hosts
![Editing mysql config file](./images5/editing-mysql-config-file-1.png)

Connecting to mysql server from mysql client
$sudo mysql -u root 
$mysql -u root -h 172.31.29.207 -p

root user is not allowed to connect. creating user in mysql server for this connection

![Connecting mysql server](./images5/connecting-to-mysqlserver.png)

<!-- Code Blocks -->
```bash
$ sudo mysql -u root -p
```
mysql> CREATE USER "ubuntu"@"%" IDENTIFIED BY "Sql@2022";

Reconnecting to mysql server from mysql client with ubuntu user
$sudo mysql -u root 

![Reconnecting to mysql server](./images5/connecting-to-mysqlserver-2.png)

$mysql -u root -h 172.31.29.207 -p

![Verifying Client-server connection](./images5/verifying-login-mysqlserver.png)