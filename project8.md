# Project 8
## Title: LOAD BALANCER SOLUTION WITH APACHE
### TASK: 
Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 instance. Make sure users can be served by Web servers through the Load Balancer. This solution will be implemented with 2 Web Servers.

#### Implementation
The following servers are already installed and configured from Project-7:

Two RHEL8 Web Servers
One MySQL DB Server (based on Ubuntu 20.04)
One RHEL8 NFS server

One Ubuntu Apache Load Balancer Server (20.04) will be configured.

####Architecture

 ![using lsblk command](./image8/architecture.png)
  ![using lsblk command](./image8/architecture-2.png)

  Initial Configuration

  * Apache (httpd) process is up and running on both Web Server 1 and Web Server 2.

  Web Server1
  ![httpd-service](https://i.imgur.com/ta0knyW.png)

  Web Server2
  ![update tooling-db script](./image8/httpd-service.png)

  * '/var/www' directories of both Web Servers are mounted to '/mnt/apps' of NFS server.

  Web Server1
  ![webserver mount point](./image8/web-nfs-1.png)

  Web Server2
  ![httpd service](./image8/web-nfs-2.png)


  * All neccessary TCP/UDP ports are open Web Server 1, Web Server2, DB Server, and NFS Servers.

  * Client browser can access both Web Servers by their respective Public IP Addresses or Public DNS names and can open the Tooling Website e.g http://<Public-IP-Address or Public-DNS-Name>/index.php)

  Web Server1
  ![httpd service](./image8/web-page-1.png)

  Web Server2
   ![httpd service](./image8/web-page-2.png)


* Open TCP port 80 by creating an Inbound Rule in Security Group on AWS

![security group](./image8/security-group.png)

* Configure and Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

* Install apache2

<!-- Code Blocks -->
```bash
$ sudo apt update

$ sudo apt upgrade

$ sudo apt install apache2 -y

$ sudo apt-get install libxml2-dev
```
![update](./image8/update.png)

 ![upgrade](./image8/upgrade.png) 

![apache install](./image8/apache-install.png) 

![libxml2 install](./image8/apache-install.png) 
 
* Enable following modules:

<!-- Code Blocks -->
```bash
$ sudo a2enmod rewrite

$ sudo a2enmod proxy

$ sudo a2enmod proxy_balancer

$ sudo a2enmod proxy_http

$ sudo a2enmod headers

$ sudo a2enmod lbmethod_bytraffic
```
![update](./image8/module-install.png)

* Restart apache2 service
<!-- Code Blocks -->
```bash
$ sudo systemctl restart apache2
```
![update](./image8/apache-restart.png)

* Configure Local DNS Name Resolution for Web Servers
<!-- Code Blocks -->
```bash
$ sudo vi /etc/hosts
```
![update](./image8/dns.png)


* Configure load balancing
<!-- Code Blocks -->
```bash
$ sudo vi /etc/apache2/sites-available/000-default.conf
```
![update](./image8/proxy-config.png)

* Restart apache server
<!-- Code Blocks -->
```bash
$ sudo systemctl restart apache2
```
![update](./image8/restart-apache.png)

* Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser(54.235.19.149)

![update](./image8/webpage-proxy-1.png)

* Refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers receive HTTP GET requests from your LB – new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers

![update](./image8/get-http-1.png)

![update](./image8/get-http-2.png)