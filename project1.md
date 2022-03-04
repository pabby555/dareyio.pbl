# Project 1
## Title: LAMP STACK IMPLEMENTATION
### Description: 
This Project describes web stack implementation (lamp stack) in AWS. 

LAMP - LINUX + APACHE + MYSQL + PHP
### Implementation Steps:
* Step 1 - Installimg apache and updating the firewall
* Step 2 - Installing mysql
* Step 3 - Installing php
* Step 4 - Creating a virtual host for your website using apache
* Step 5 - Enable php on the website

<!-- Horizontal Rule -->
------------------------------------

1. STEP ONE: INSTALLING APACHE AND UPDATING THE FIREWALL
<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository

![update](https://i.imgur.com/ot6vtfZ.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Upgrading package repository

![update screen 3](https://i.imgur.com/f0D0Fan.png)

<!-- Code Blocks -->
```bash
$ sudo apt install apache2
```
Installing Apache

![Apache2 install screen](https://i.imgur.com/Zcz10Nn.png)

<!-- Code Blocks -->
```bash
$ sudo ufw app list
```
Checking firewall App list

![Firewall settings screen 2](https://i.imgur.com/X6ky2D6.png)

<!-- Code Blocks -->
```bash
$ sudo ufw allow 'Apache'
```
Allowing Apache traffic through firewall

![Firewall settings screen 3](https://i.imgur.com/QrBIL0R.png)

<!-- Code Blocks -->
```bash
$ sudo ufw status
$ sudo systemctl status apache2
```
Checking Status of Apache Service

![Apache status screen ](https://i.imgur.com/ajIgzRv.png)

<!-- Code Blocks -->
```bash
$ curl -4 ip-172-31-26-208

```
Verifying successful configuration of Apache server via CMD

![Apache web page screen](https://i.imgur.com/qDoZKAB.png)

Verifying successful configuration of Apache server via URL http://34.229.127.184/

![Apache web page screen 2](https://i.imgur.com/2xI2eZm.png)

<!-- Horizontal Rule -->
------------------------------------

2. STEP TWO: INSTALLING MYSQL
<!-- Code Blocks -->
```bash
$ sudo apt install mysql-server
```
Installing mysql server

![installing sql-server screen](https://i.imgur.com/upIywp7.png)
<!-- Code Blocks -->
```bash
$ sudo mysql -V
```
Verifying mysql Install

![mysql install confirmation screen](https://i.imgur.com/Pww0c90.png)

<!-- Code Blocks -->
```bash
$ sudo mysql_secure_installation
```

Verifying mysql secure deployment

![mysql secure installation 2 screen](https://i.imgur.com/YKJBQDL.png)
![mysql secure installation 3](https://i.imgur.com/NCmzeco.png)
![mysql secure installation 4](https://i.imgur.com/c1RC3KX.png)
![mysql secure installation 5](https://i.imgur.com/EbN9EnF.png)
![mysql secure installation 6](https://i.imgur.com/ChXsK2J.png)
![mysql secure installation 7](https://i.imgur.com/vNVVGpR.png)
![mysql secure installation 8](https://i.imgur.com/XdyHtOv.png)
<!-- Code Blocks -->
```bash
$ sudo systemctl status mysql
```
Verifying myql service is running

![mysql service status screen](https://i.imgur.com/QKL0C82.png)

<!-- Code Blocks -->
```bash
$ sudo mysql -u root
```
Logging into mysql server

![mysql login verification](https://i.imgur.com/wKKkwsk.png)
<!-- Horizontal Rule -->
------------------------------------

3. STEP THREE: INSTALLING PHP
<!-- Code Blocks -->
```bash
$ sudo apt install php libapache2-mod-php php-mysql
```
Installing php

![php install screen 2](https://i.imgur.com/PWmcu76.png)

<!-- Code Blocks -->
```bash
$ php -v
```
Verifying PHP install

![php install verification screen](https://i.imgur.com/fZedDX4.png)

<!-- Horizontal Rule -->
------------------------------------

4. STEP FOUR: CREATING A VIRTUAL HOST FOR WEBSITE USING APACHE
<!-- Code Blocks -->
```bash
$ sudo mkdir /var/www/projectlamp
```
Creating directory projectlamp under /var/www/ directory

![creating directory projectlamp](https://i.imgur.com/FQX1W2D.png)

<!-- Code Blocks -->
```bash
$ sudo chown -R /var/www/projectlamp
```
Changing directory ownership of /var/www/projectlamp from root to ubuntu user

  ![changing ownership to ubuntu user](https://i.imgur.com/Lnync7u.png)

 <!-- Code Blocks -->
```bash
$ sudo vim /etc/apache2/sites-available/projectlamp.conf

```
Creating projectlamp.conf configuration file

![projectlamp.conf config file](https://i.imgur.com/ComslH3.png)
![files under sites-available directory](https://i.imgur.com/Wrb5ZYY.png)

<!-- Code Blocks -->
```bash
$ sudo a2ensite projectlamp
```
Enabling new Virtual Host

![enabling virtual host](https://i.imgur.com/VXunU1Q.png)

<!-- Code Blocks -->
```bash
$ sudo a2ensite projectlamp
```
Disabling  default Apache default website

![disabling default website](https://i.imgur.com/cRzRo9w.png)

<!-- Code Blocks -->
```bash
$ sudo apache2ctl configtest
```
Verifying Apache Config file syntax

![checking config file syntax](https://i.imgur.com/0ioLZIs.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
Reloading Apache2 service

![reloading Apache2 service](https://i.imgur.com/Hzi2g0Z.png)

<!-- Code Blocks -->
```bash
$ sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
Verifying website is active

![verifying virtual host](https://i.imgur.com/qFPfIFl.png)![verifying website is active](https://i.imgur.com/1PB6NRi.png)

5. STEP FIVE: ENABLING PHP ON THE WEBSITE
<!-- Code Blocks -->
```bash
$ sudo vim /etc/apache2/mods-enabled/dir.conf
```
Editing the /etc/apache2/mods-enabled/dir.conf file to change the order in which the index.php file is listed within the DirectoryIndex directive

![default config settings](https://i.imgur.com/kJpcMuw.png)
![after change config file ](https://i.imgur.com/H4veGMR.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
Reloading Apache2 service

![reloading Apache2 service](https://i.imgur.com/4JnkZFP.png)

<!-- Code Blocks -->
```bash
$ vim /var/www/projectlamp/index.php
```
Creating a PHP script for testing

![php script](https://i.imgur.com/3LJyDjx.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
Reloading Apache2 service

![reloading Apache2 service](https://i.imgur.com/4Ywz6nP.png)

Verifying website is active
http://34.229.127.184/

![verifyig php website](https://i.imgur.com/GShpo6C.png)