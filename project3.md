# Project 3
## Title: MERN STACK IMPLEMENTATION IN AWS CLOUD
### TASK: 
Deploy a simple TO_DO Application that creates a TO_DO list

![to-do application](./images/to-do-application.png)

MERN -MONGODB + EXPRESSJS + REACTJS + NODE.JS

* MongoDB: This is a document based, No-SQL database used to store application data in a form of documents.
* ExpressJS: A server-side Web Application framework for node.js.
* ReactJS: A frontend framework developed by facebook. it is based on Javascript, used to build User Interface (UI) components.
* Node.js: A JavaScript runtime environment. It is used to run Javascript on a machine rather than in a browser.


### Implementation Steps:
* Step 1 - Backend configuration
* Step 2 - Frontend Creation
 
 <!-- Horizontal Rule -->
----------------------------------------------------

1. STEP ONE: backend configuration
<!-- Code Blocks -->
```bash
$ sudo apt update
```
Updating package repository
![package update](./images/updating-packages.png)

<!-- Code Blocks -->
```bash
$ sudo apt upgrade
```
Upgrading package repository
![package upgrading](./images/upgrading-packages.png)

<!-- Code Blocks -->
```bash
$ curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
Getting location of Node.js from ubuntu repository
![locating nodejs software](./images/locating-node.js-software.png)

<!-- Code Blocks -->
```bash
$ sudo apt-get install -y nodejs
```
Installing node.js 
![installing node.js](./images/node.js-install.png)

<!-- Code Blocks -->
```bash
$ node -v
$ npm -v
```
Verifying node installation 
![verifying node install](./images/verifying-node-install.png)

<!-- Code Blocks -->
```bash
$ mkdir Todo
```
Creating directory for To-Do project
![creating to-do directory](./images/creating-todo-directory.png)

<!-- Code Blocks -->
```bash
$ cd Todo
$ npm init
$ ls
```
Initializing To-Do project
![initializing project](./images/initializing-todo-project.png)

![verifying package.json file](./images/verifying-package.json-file.png)

<!-- Code Blocks -->
```bash
$ npm install express
```
Installing ExpressJS
![installing ExpreesJS](./images/installing%20expressjs.png)

<!-- Code Blocks -->
```bash
$ touch index.js
$ ls
```
Creating file index.js
![Creating index.js file](./images/creating-index.js-file.png)

<!-- Code Blocks -->
```bash
$ npm install dotenv
```
Installing dotenv module
![Installing dotenv module](./images/installing-dotenv-module.png)

<!-- Code Blocks -->
```bash
$ sudo vim index.js
```
Editing index.js file
![Editing index.js file](./images/editing-index.js-file.png)

<!-- Code Blocks -->
```bash
$ node index.js
```
Starting server
![starting server](./images/running-server.png)

Verifying Server is running via URL
![Verifying running server](./images/verifying-running-server-URL.png)

<!-- Horizontal Rule -->
----------------------------------------------------

TO-DO Application need to carry out the following actions.

1. Create a new task using "POST" HTTP request method.

2. Display list of all tasks using "GET" HTTP request method

3. Delete a completed task using "DELETE" HTTP request method.

<!-- Code Blocks -->
```bash
$ mkdir routes 
```
Creating routes directory to be used for creating routes for each task that will define various endpoints that the To-do application will depend on
![creating routes directory](./images/creating-routes-directory.png)

<!-- Code Blocks -->
```bash
$ touch api.js
$ sudo vim api.js
```
Creating and Editing api.js file
![Creating and Editing api.js file](./images/creating-editing-api.js.png)

<!-- Horizontal Rule -->
----------------------------------------------------

<!-- Code Blocks -->
```bash
$ npm install mongoose
```
Installing mongoose, which is a node.js package
![Installing mongoose package](./images/installing-mongoose.png)

<!-- Code Blocks -->
```bash
$ mkdir models && cd models && touch todo.js
```
Creating models directory and todo.js file
![Creating models directory](./images/creating-models-dir-todo.js-file.png)
![Editing todo.js file](./images/editing-todo.js-file.png)

<!-- Code Blocks -->
```bash
$ mkdir models && cd models && touch todo.js
```
Updating api.js file in routes directory to make use of new model
![Updating api.js file in routes directory](./images/updated-api.js-file.png)

<!-- Horizontal Rule -->
----------------------------------------------------

<!-- Code Blocks -->
```bash
$ touch .env
$ vim .env
```

Updating .env file with database information
![Updating .env file](./images/creating-.env-file.png)

<!-- Code Blocks -->
```bash
$ touch .env
$ vim .env
```
Updating index.js file for Node.js to be able to connect to database
![Updating index.js file for database access](./images/edited-index.js-file.png)

<!-- Code Blocks -->
```bash
$ node index.js
```
Starting server to verify database connected successfully
![starting server ](./images/starting-server-db-connecting.png)

<!-- Horizontal Rule -->
----------------------------------------------------

Testing Backend Code without frontend using Restful API
![testing on POSTMAN ](./images/testing-on-postman.png)

Testing Backend Code on MongoDB
![testing on mongoDB ](./images/testing-on-mongodb.png)

<!-- Horizontal Rule -->
----------------------------------------------------

1. STEP TWO: Frontend creation
<!-- Code Blocks -->
```bash
$ sudo npm cache clean -f
$ sudo npm install -g n
$ sudo n stable
$ sudo n latest
```
Updating node.js to latest version
![creating client folder](./images/scafolding-app.png)

<!-- Code Blocks -->
```bash
$  npx create-react-app client
```
Creating a new folder called client to add all react code
![creating client folder](./images/scafolding-app.png)

![client folder created](./images/client-directory-created.png)

<!-- Code Blocks -->
```bash
$  npm install concurrently --save-dev
```
Installing concurrently to run more than one command simultaneously from the same terminal window
![installing concurently](./images/installing-concurrently.png)

<!-- Code Blocks -->
```bash
$  npm install nodemon --save-dev
```
Installing nodemon to run and monitor the server, restarting the server automatically and loading new changes when required
![installing nodemon](./images/installing-nodemon.png)

<!-- Code Blocks -->
```bash
$  npm install nodemon --save-dev
```
Before editing package.json file
![before editing package.json file](./images/before-editing-package.json-file.png)

After editing package.json file
![after editing package.json file](./images/after-editing-package.json-file.png)

<!-- Code Blocks -->
```bash
$ cd client
$ vi package.json
```
Configuring proxy in package.json by adding "proxy": "http://localhost:5000"
![before editing package.json file](./images/editing-client-package.json-file.png)

<!-- Code Blocks -->
```bash
$ npm run dev
```
Opening and starting app
![opening and starting app](./images/app-started-successfully.png)

verifying from web browser at http://3.95.241.241:3000
![verifying app](./images/verifying-app-browser.png)

<!-- Horizontal Rule -->
----------------------------------------------------

<!-- Code Blocks -->
```bash
$ cd client
$ cd src
$ mkdir components
```
Creating components directory
![components directory](./images/creating-components-directory.png)

<!-- Code Blocks -->
```bash
$ cd components
$ touch Input.js ListTodo.js Todo.js
```
Creating three files under components directory
![creating files in components directory](./images/creating-three-files-components.png)

<!-- Code Blocks -->
```bash
$ vim Input.js
```
Editing Input.js 
![editing input.js file](./images/editing-input.js-file-1.png)

![editing input.js file](./images/editing-input.js-file-2.png)

<!-- Code Blocks -->
```bash
$ cd client
$ touch Input.js ListTodo.js Todo.js
```
Creating three files under components directory
![creating files in components directory](./images/creating-three-files-components.png)

<!-- Code Blocks -->
```bash
$ cd client
$ sudo npm install axios
```
Installing axios
![installing axios](./images/installing-axios.png)

<!-- Code Blocks -->
```bash
$ cd src/components
$ vim ListTodo.js
```
Editing ListTodo.js file
![Editing listTodo](./images/editing-listtodo-file.png)


<!-- Code Blocks -->
```bash
$ cd src
$ vim App.js
```
Editing App.js file
![Editing Todolist](./images/editing-app.js-file.png)

<!-- Code Blocks -->
```bash
$ cd src
$ vim App.cs
```
Editing App.cs file
![Editing app.cs file](./images/editing-app.cs-file-1.png)

![Editing app.cs file](./images/editing-app.cs-file-2.png)

![Editing app.cs file](./images/editing-app.cs-file-3.png)

<!-- Code Blocks -->
```bash
$ cd src
$ vim index.css
```
Editing index.css file
![Editing index.css file](./images/editing-index.css-file.png)

<!-- Code Blocks -->
```bash
$ cd Todo
$ npm run dev
```
Testing Todo App
![testing Todo App](./images/verifying-todo-app.png)

![testing Todo App](./images/verifying-todo-app-browser.png)




























