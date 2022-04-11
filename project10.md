# Project 10
## Title: LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS
### TASK: 
In this project, website will be registered with LetsEnrcypt Certificate Authority. Certbot, recommended by LetsEncrypt will be used to automate certificate issuance you will use a shell client recommended by LetsEncrypt – cetrbot.

When data is moving between a client (browser) and a Web Server over the Internet – it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-In-The-Middle (MIMT) attack.

This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by cybercriminals.

SSL and its newer version, TSL – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used.

SSL/TLS uses digital certificates to identify and validate a Website. A browser reads the certificate issued by a Certificate Authority (CA) to make sure that the website is registered in the CA so it can be trusted to establish a secured connection.

This project consists of two parts:

Configure Nginx as a Load Balancer
Register a new domain name and configure secured connection using SSL/TLS certificates.


![architecture](./images10/architecture.png)

<!-- Horizontal Rule -->
----------------------------------------------------
#### Implementation
1.  - CONFIGURE NGINX AS A LOAD BALANCER

* Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)

![firewall](./images10/security-group.png)

* Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

 <!-- Code Blocks -->
```bash
$ sudo vi /etc/hosts
```
  ![update](./images10/local-dns.png)

* Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

 <!-- Code Blocks -->
```bash
$ sudo apt update

$ sudo apt upgrade

$ sudo apt install nginx
```
  ![update](./images10/update.png)

  ![upgrde](./images10/upgrade.png)

  ![nginx install](./images10/nginx-install.png) 

  * Configure Nginx LB using Web Servers’ names defined in /etc/hosts

  <!-- Code Blocks -->
```bash
$ sudo vi /etc/nginx/nginx.conf

#insert following configuration into http section

 upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;
```
  ![nginx](./images10/nginx-config.png)



* Restart Nginx and make sure the service is up and running

<!-- Code Blocks -->
```bash
$ sudo systemctl restart nginx

$ sudo systemctl status nginx
```
  ![nginx service](./images10/nginx-service.png)

1.  -REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

Register a new domain

  ![domain](./images10/domain.png)

  Assign an Elastic IP to the Nginx LB server and associate the domain name (tinatech.co) with the Elastic IP (34.193.237.59)

   ![domain](./images10/elastic-ip.png)


   ![domain](./images10/elastic-ip-2.png)


   ![domain](./images10/elastic-ip-3.png)


   ![domain](./images10/elastic-ip-4.png)


   ![domain](./images10/elastic-ip-5.png)

   * Update A record with the domain registrar to point to Nginx LB using Elastic IP address


      ![domain](./images10/dns-record.png)

* Edit Nginx config file to reflect new domain name

<!-- Code Blocks -->
```bash
$ sudo vi /etc/nginx/nginx.conf

$ sudo systemctl restart nginx

$ sudo systemctl status nginx
```
  ![nginx service](./images10/nginx-edit.png)

  ![nginx service](./images10/nginx-restart.png)


* Check that the Web Servers can be reached from your browser using new domain name using HTTP protocol – http://tinatech.co

 ![domain](./images10/tinatech-webpage.png)

 ![domain](./images10/tinatech-webpage-2.png)

* Install certbot and request for an SSL/TLS certificate.

 * Make sure snapd service is active and running

 <!-- Code Blocks -->
```bash
$ sudo systemctl status snapd
```
  ![snapd service](./images10/snapd.png)

* Install certbot
<!-- Code Blocks -->
```bash
$ sudo snap install --classic certbot
```
  ![snapd service](./images10/certbot.png)

  * Request your certificate (follow the certbot instructions – you will need to choose tinatech.co as the domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file. update the file.)

  <!-- Code Blocks -->
```bash
$ sudo ln -s /snap/bin/certbot /usr/bin/certbot

$ sudo certbot --nginx
```
 ![certbot](./images10/certbot-2.png)

 ![certbot](./images10/certbot-3.png)  

 ![certbot](./images10/certbot-4.png) 

* Test secured access to your Web Solution by trying to reach https://tinatech.co

![certbot](./images10/certbot-5.png) 

* Set up periodical renewal of your SSL/TLS certificate

* By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently. You can test renewal command in dry-run mode

<!-- Code Blocks -->
```bash
$ sudo certbot renew --dry-run

```
 ![certbot](./images10/certbot-6.png)

* Best pracice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.

<!-- Code Blocks -->
```bash
$ sudo crontab -e

```


 Add following line:

* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1










