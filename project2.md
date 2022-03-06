# Project 2
## Title: LEMP STACK IMPLEMENTATION
### Description: 
This Project describes web stack implementation (lemp stack) in AWS. 

LAMP - LINUX + NGINX + MYSQL + PHP
### Implementation Steps:
* Step 1 - Installimg nginx web server
* Step 2 - Installing mysql
* Step 3 - Installing php
* Step 4 - Configuring nginx to use php
* Step 5 - Testing php with nginx
* Step 6 - Retrieving data from mysql database with php

<!-- Horizontal Rule -->
----------------------------------------------------

1. STEP ONE: INSTALLING NGINX WEB SERVER
<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository
![update screen](https://i.imgur.com/D5ywvZt.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Upgrading package repository
![Upgrade screen](https://i.imgur.com/JsNVQVx.png)

<!-- Code Blocks -->
```bash
$ sudo apt install nginx
```
Installing nginx
![nginx install](https://i.imgur.com/cL910Ly.png)


<!-- Code Blocks -->
```bash
$ sudo systemctl status nginx
```
Checking Status of Nginx Service
![nginx service status](https://i.imgur.com/MbvkMQV.png)

<!-- Code Blocks -->
```bash
$ curl -4 ip-172-31-26-208

```
Verifying successful configuration of nginx server via CMD
![nginx webpage verification](https://i.imgur.com/csqtn9g.png)

Verifying successful configuration of nginx server via URL http://3.84.218.163/
![nginx wepage via URL](https://i.imgur.com/4KgPgar.png)

<!-- Horizontal Rule -->
----------------------------------------------------

2. STEP TWO: INSTALLING MYSQL
<!-- Code Blocks -->
```bash
$ sudo apt install mysql-server
```
Installing mysql server
![mysql install ](https://i.imgur.com/YsgYAbB.png)
<!-- Code Blocks -->
```bash
$ sudo mysql -V
```
Verifying mysql Install
![mysql install verification](https://i.imgur.com/kt8GGRv.png)

<!-- Code Blocks -->
```bash
$ sudo mysql_secure_installation
```

Verifying mysql secure deployment
![mysql secure install screen 1](https://i.imgur.com/DeJp9eA.png)
![mysql secure install screen 2](https://i.imgur.com/M7c0ltk.png)
![mysql secure install screen 3](https://i.imgur.com/7hdc951.png)
![mysql secure install screen 4](https://i.imgur.com/vsp6Ejk.png)
![mysql secure install screen 5](https://i.imgur.com/4LVexYp.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl status mysql
```
Verifying mysql service is running
![mysql service status](https://i.imgur.com/BMGtPz7.png)

<!-- Code Blocks -->
```bash
$ sudo mysql -u root
```
Logging into mysql server
![mysql login verification](https://i.imgur.com/APNG5BQ.png)

<!-- Horizontal Rule -->
----------------------------------------------------

3. STEP THREE: INSTALLING PHP
<!-- Code Blocks -->
```bash
$ sudo apt install php-fpm php-mysql
```
Installing php
![php install screen](https://i.imgur.com/s1CqYR0.png)

<!-- Code Blocks -->
```bash
$ php -v
```
Verifying PHP install
![php install verification](https://i.imgur.com/AttnR2j.png)

<!-- Horizontal Rule -->
----------------------------------------------------

4. STEP FOUR: CONFIGURING NGINX TO USE PHP PROCESSOR
<!-- Code Blocks -->
```bash
$ sudo mkdir /var/www/projectLEMP
```
Creating root web directory projectLEMP under /var/www/ directory
![creating directory projectLEMP](https://i.imgur.com/fnrGwm2.png)

<!-- Code Blocks -->
```bash
$ sudo chown ubuntu:ubuntu -R /var/www/projectLEMP
```
Changing directory ownership of /var/www/projectLEMP from root to ubuntu user
![changing ownership of projectLEMP](https://i.imgur.com/HNoVcGe.png)

 <!-- Code Blocks -->
```bash
$ sudo vim /etc/nginx/sites-available/projectLEMP.conf

```
Creating projectLEMP.conf configuration file
![nginx config file screen](https://i.imgur.com/xGQtMAo.png)

<!-- Code Blocks -->
```bash
$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
Activating configuration by linking the nginx config file from nginx`s sites-enabled directory
![linking nginx config file screen](https://i.imgur.com/dkGQR78.png)

<!-- Code Blocks -->
```bash
$ sudo nginx -t
```
Testing nginx config file for syntax errors
![testing nginx config for syntax errors](https://i.imgur.com/Im7a9l1.png)

<!-- Code Blocks -->
```bash
$ sudo unlink /etc/nginx/sites-enabled/default
```
Disabling default nginx host listening on port 80
![unlinking default nginx host](https://i.imgur.com/SkYw7rp.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload nginx
```
Reloading nginx service to apply changes
![reloading nginx service](https://i.imgur.com/nKBhqPH.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload nginx
```
Creating index.html file in the web root /var/www/projectLEMP
![creating index.html file in projectLEMP Directory](https://i.imgur.com/J6268Fc.png)


<!-- Code Blocks -->
```bash
$ sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
Verifying website is active
![nginx webpage verifying config](https://i.imgur.com/FtxQrte.png)

<!-- Horizontal Rule -->
----------------------------------------------------

5. STEP FIVE: TESTING PHP WITH NGINX
<!-- Code Blocks -->
```bash
$ sudo vim /var/www/projectLEMP/info.php
```
Creating test PHP file info.php in document root to validate that nginx can correctly hand .php files off to the PHP processor
![creating php test file](https://i.imgur.com/gTYRmr7.png)


Verifying website is active
http://34.203.226.84/info.php
![verifying nginx webpage](https://i.imgur.com/szS74dc.png)

<!-- Horizontal Rule -->
----------------------------------------------------

6. STEP FIVE: RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
<!-- Code Blocks -->
```bash
$ sudo mysql
```
Connecting to the MYSQL console
![connecting to mysql console](https://i.imgur.com/6PKPQvG.png)

<!-- Code Blocks -->
```bash
> CREATE DATABASE example_database
```
Creating database "example_database"
![creating database](https://i.imgur.com/07N6HtH.png)

<!-- Code Blocks -->
```bash
> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Pass;$222'
```
Creating user "example_user"
![creating user in new database ](https://i.imgur.com/89zeraY.png)

<!-- Code Blocks -->
```bash
> GRANT ALL ON example_database.* TO 'example_user'@'%';
```
Granting user "example_user" privilege
![granting new user privilege](https://i.imgur.com/xYTSHm2.png)

exit mysqlshell (>exit)

<!-- Code Blocks -->
```bash
> mysql -u example_user -p
```
Logging into MySQL console with user "example_user" credentials to test permissions.
![logging into mysql as example_user](https://i.imgur.com/vVpEI1Q.png)

<!-- Code Blocks -->
```bash
> GRANT ALL ON example_database.* TO 'example_user'@'%';
```
Granting user example_user access to example_database
![granting new user access to new database](https://i.imgur.com/urEtfDN.png

<!-- Code Blocks -->
```bash
> exit
> mysql -u example_user -p
> SHOW DATABASES;
```
log in as example_user and display databases
![displaying databases as  example_user ](https://i.imgur.com/COQuteu.png)

<!-- Code Blocks -->
```bash
> CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));
> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
> INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
> INSERT INTO example_database.todo_list (content) VALUES ("My third important item");
> INSERT INTO example_database.todo_list (content) VALUES ("And this is one more");
> exit
```
Creating table todo_list and inserting values
![creating table and inserting values](https://i.imgur.com/zgEyHmt.png)

<!-- Code Blocks -->
```bash
$ vim /var/www/projectLEMP/todo_list.php
```
Creating test PHP file info.php in document root to connect mysql database
![creating php test file to connect database](https://i.imgur.com/9xxwY4C.png)


Verifying website is active
http://34.203.226.84/todo_list.php
![verifying nginx webpage to connect mysql database](https://i.imgur.com/8AXcgnx.png)
























