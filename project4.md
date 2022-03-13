# Project 4
## Title: MEAN STACK IMPLEMENTATION IN AWS CLOUD
### TASK: 
Implement a simple Book Register Web Form using MEAN stack.

MEAN -MONGODB + EXPRESSJS + ANGULARJS + NODE.JS

* MongoDB: This is a document based, No-SQL database. It will store data and allow data retrieval.
* ExpressJS: This is back-end, server-side Application framework that will make requests to the MongoDB for Reads and Writes.
* AngularJS: This is the front-end application framework that will handle client and server requests.
* Node.js: This is the JavaScript runtime environment that will accept requests and display results to end users.

<!-- Horizontal Rule -->
----------------------------------------------------

### Implementation Steps:
* Step 1 - Install NodeJS
Node.js is used to setup the Express routes and AngularJS controllers.

<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository
![package update](./images4/update.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Upgrading package repository
![package upgrading](./images4/upgrade.png)

<!-- Code Blocks -->
```bash
$ sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

$ curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
Adding certificates
![Adding certificate](./images4/adding-certificate.png)

![Adding certificate](./images4/adding-certificate-2.png)

<!-- Code Blocks -->
```bash
$ sudo apt install -y nodejs
```
Installing NodeJS
![package upgrading](./images4/nodejs-install.png)

<!-- Horizontal Rule -->
----------------------------------------------------


* Step 2 - Install MongoDB
 
 <!-- Code Blocks -->
```bash
$ sudo apt sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

$ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
Pre-MongoDB Install
![Key-import](./images4/mongodb-key-import.png)

![Key-import](./images4/mongodb-install-1.png)

<!-- Code Blocks -->
```bash
$ sudo apt install -y mongodb
```
Installing MongoDB
![Mongodb install](./images4/mongodb-install-2.png)

<!-- Code Blocks -->
```bash
$ sudo service mongodb start
```
Starting the Server and verifying the service is up and running
![starting server](./images4/starting-mongodb-server.png)

<!-- Code Blocks -->
```bash
$ sudo apt install -y npm
```
Installing Node Package Manager (npm)
![starting server](./images4/installing-npm.png)

<!-- Code Blocks -->
```bash
$ sudo npm install body-parser

$ sudo npm install -g npm@8.5.4
```
Installing body-parser package
![starting server](./images4/installing-npm.png)

Upgrading npm to 8.5.4 version
![starting server](./images4/upgrading-npm.png)

<!-- Code Blocks -->
```bash
$ mkdir Books && cd Books
```
Creating 'Books' directory
![starting server](./images4/creating-books-directory.png)

<!-- Code Blocks -->
```bash
$ npm init
```
Initializing npm project
![initializing project](./images4/initializing-npm.png)

<!-- Code Blocks -->
```bash
$ vim server.js
```
Creating file server.js
![file server.js](./images4/creating-server.js-file.png)

<!-- Horizontal Rule -->
----------------------------------------------------

* Step 3 - Install Express and Setup routes to the server.

<!-- Code Blocks -->
```bash
$ sudo npm install express mongoose
```
Installing Express and Mongoose package
![Express and Mongoose Install](./images4/installing-express-mongoose.png)

<!-- Code Blocks -->
```bash
$ mkdir apps && cd apps
```
Creating apps folder in Books directory
![Creating apps directory](./images4/creating-apps-directory.png)

<!-- Code Blocks -->
```bash
$ vim routes.js
```
Creating and Editing routes.js file 
![Creating routes directory](./images4/editing-routes.js-file.png)

<!-- Code Blocks -->
```bash
$ mkdir models && cd models
```
Creating models directory in apps folder 
![Creating models directory](./images4/creating-models-directory.png)

<!-- Code Blocks -->
```bash
$ vim book.js
```
Creating book.js file in models directory 
![Creating book.js file](./images4/creating-books.js-file.png)

<!-- Horizontal Rule -->
----------------------------------------------------

* Step 4 - Access the routes with AngularJS. AngularJS will connect our web page with Express and perform actions on the book register

<!-- Code Blocks -->
```bash
$ mkdir public && cd public
```
Creating public directroy in Books folder 
![Creating public directory](./images4/creating-public-directory.png)

<!-- Code Blocks -->
```bash
$ vim script.js
```
Creating script.js file in public directory 
![Creating script.js file](./images4/creating-script.js-file.png)

<!-- Code Blocks -->
```bash
$ vim index.html
```
Creating index.html file in public directory 
![Creating index.html file](./images4/creating-index.html-1.png)

![Creating index.html file](./images4/creating-index.html-2.png)

<!-- Code Blocks -->
```bash
$ node server.js
```
Starting server from Books directory 
![starting server](./images4/starting-server.png)

<!-- Code Blocks -->
```bash
$ curl -s http://localhost:3300
```
Verifying Application from cmd 
![verifying application](./images4/verifying-application-cmd.png)

Verifying Application from url

http://107.20.15.5:3300
![verifying application](./images4/verifying-application-url.png)