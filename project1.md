# Project1
## Title: LAMP STACK IMPLEMENTATION
### Description: 
This Project describes Web stack implementation (lamp stack) in aws.
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
$ sudo apt install apache2
$ sudo ufw app list
$ sudo ufw allow 'Apache'
$ sudo ufw status
$ sudo systemctl status apache2
$ curl -4 FQDN
$ http://
