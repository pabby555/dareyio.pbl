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
######Updating package repository
![](2022-03-03-18-22-46.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
######Upgrading package repository
![](2022-03-03-18-27-30.png)

<!-- Code Blocks -->
```bash
$ sudo apt install apache2
```
######Installing Apache
![](2022-03-03-18-31-16.png)

<!-- Code Blocks -->
```bash
$ sudo ufw app list
```
######Checking firewall App list
![](2022-03-03-18-33-37.png)

<!-- Code Blocks -->
```bash
$ sudo ufw allow 'Apache'
```
######Allowing Apache traffic through firewall
![](2022-03-03-18-37-24.png)

<!-- Code Blocks -->
```bash
$ sudo ufw status
$ sudo systemctl status apache2
```
######Checking Status of Apache Service
![](2022-03-03-18-39-14.png)

<!-- Code Blocks -->
```bash
$ curl -4 ip-172-31-26-208

```
######Verifying successful configuration of Apache server via CMD
![](2022-03-03-18-40-37.png)


######Verifying successful configuration of Apache server via URL http://34.229.127.184/
![](2022-03-03-18-41-47.png)

<!-- Horizontal Rule -->
------------------------------------

2. STEP TWO: INSTALLING MYSQL
<!-- Code Blocks -->
```bash
$ sudo apt install mysql-server
```
######Installing mysql server
![](2022-03-03-18-48-51.png)
<!-- Code Blocks -->
```bash
$ sudo mysql -V
```
######Verifying mysql Install
![](2022-03-03-18-46-52.png)

<!-- Code Blocks -->
```bash
$ sudo mysql_secure_installation
```

######Verifying mysql secure deployment
![](2022-03-03-18-56-46.png)

![](2022-03-03-18-57-24.png)

![](2022-03-03-18-57-57.png)

![](2022-03-03-18-58-24.png)

![](2022-03-03-18-58-53.png)

![](2022-03-03-18-59-28.png)

![](2022-03-03-18-59-58.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl status mysql
```
######Verifying myql service is running
![](2022-03-03-19-02-02.png)

<!-- Code Blocks -->
```bash
$ sudo mysql -u root
```
######Logging into mysql server
![](2022-03-03-19-03-38.png)

<!-- Horizontal Rule -->
------------------------------------

3. STEP THREE: INSTALLING PHP
<!-- Code Blocks -->
```bash
$ sudo apt install php libapache2-mod-php php-mysql
```
######Installing mysql server
![](2022-03-03-19-12-05.png)

<!-- Code Blocks -->
```bash
$ php -v
```
######Verifying PHP install
![](2022-03-03-19-13-36.png)

<!-- Horizontal Rule -->
------------------------------------

4. STEP FOUR: CREATING A VIRTUAL HOST FOR WEBSITE USING APACHE
<!-- Code Blocks -->
```bash
$ sudo mkdir /var/www/projectlamp
```
######Creating directory projectlamp under /var/www/ directory
![](2022-03-03-19-25-25.png)

<!-- Code Blocks -->
```bash
$ sudo chown -R /var/www/projectlamp
```
######Changing directory ownership of /var/www/projectlamp from root to ubuntu user
 ![](2022-03-03-19-31-27.png)

 <!-- Code Blocks -->
```bash
$ sudo vim /etc/apache2/sites-available/projectlamp.conf

```
######Creating projectlamp.conf configuration file
![](2022-03-03-19-41-15.png)
![](2022-03-03-19-42-23.png)

<!-- Code Blocks -->
```bash
$ sudo a2ensite projectlamp
```
######Enabling new Virtual Host
![](2022-03-03-19-47-13.png)

<!-- Code Blocks -->
```bash
$ sudo a2ensite projectlamp
```
######Disabling  default Apache default website
![](2022-03-03-19-50-04.png)

<!-- Code Blocks -->
```bash
$ sudo apache2ctl configtest
```
######Verifying Apache Config file syntax
![](2022-03-03-19-52-10.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
######Reloading Apache2 service
![](2022-03-03-19-56-43.png)

<!-- Code Blocks -->
```bash
$ sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
######Verifying website is active
![](2022-03-03-20-09-15.png)
![](2022-03-03-20-10-24.png)

5. STEP FIVE: ENABLING PHP ON THE WEBSITE
<!-- Code Blocks -->
```bash
$ sudo vim /etc/apache2/mods-enabled/dir.conf
```
######Editing the /etc/apache2/mods-enabled/dir.conf file to change the order in which the index.php file is listed within the DirectoryIndex directive
![](2022-03-03-20-19-39.png)
![](2022-03-03-20-20-07.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
######Reloading Apache2 service
![](2022-03-03-19-56-43.png)

<!-- Code Blocks -->
```bash
$ vim /var/www/projectlamp/index.php
```
######Creating a PHP script for testing
![](2022-03-03-20-32-58.png)

<!-- Code Blocks -->
```bash
$ sudo systemctl reload apache2
```
######Reloading Apache2 service
![](2022-03-03-19-56-43.png)

######Verifying website is active
http://34.229.127.184/

![](2022-03-03-21-02-38.png)
