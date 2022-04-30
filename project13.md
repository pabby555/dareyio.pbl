Project 13
## Title: ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES

### TASK: 
introduce dynamic assignments by using include module. The module that enables dynamic assignments is include. 

Hence,

import = Static
include = Dynamic

When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to its dynamic nature. However, you can use dynamic assignments for environment specific variables as we will be introducing in this project.

* Step 1: Introduce Dynamic Assignment Into the current structure. 

* Start a new branch named dynamic-assignments.

![new-branch](https://i.imgur.com/WCYiz7N.png)

* It is recommended to have all the codes managed and tracked in GitHub. Create a new folder, named "dynamic-assignments". Then inside this folder, create a new file and name it "env-vars.yml". Recreate the structure below manually.

 <!-- Code Blocks -->
```bash

├── dynamic-assignments
│   └── env-vars.yml
├── inventory
│   └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
└── roles (optional folder)
    └──...(optional subfolders & files)
└── static-assignments
    └── common.yml
```

![new-dir](https://i.imgur.com/7EcYFJC.png)

![new-dir-2](https://i.imgur.com/F3Xrobs.png)

![tree](https://i.imgur.com/0WU1KIb.png)


* Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc. create a new folder env-vars, then for each environment, create new YAML files which we will use to set variables.

The layout should now look like this.

<!-- Code Blocks -->
```bash
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```
![new-dir-3](https://i.imgur.com/HdgqH4k.png)

![tree-2](https://i.imgur.com/PyjULLG.png)

* Paste the instruction below into the env-vars.yml file.

<!-- Code Blocks -->
```bash
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ /var/lib/jenkins/workspace/ansible }}/../env-vars"
      tags:
        - always
```
![env-var-file](https://i.imgur.com/XLVQo6l.png)


NOTE:
1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:
include_role
include_tasks
include_vars
In the same version, variants of import were also introduces, such as:

import_role
import_tasks

2. We made use of a special variables { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.

3. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.

* Step 2: Update site.yml with dynamic assignments

site.yml should like this:

<!-- Code Blocks -->
```bash
---
- hosts: all
- name: Include dynamic variables 
  tasks:
  import_playbook: ../static-assignments/common.yml 
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml
  ```
  ![site-file](https://i.imgur.com/b6bguWW.png)


  * Create a role for MySQL database using community roles – it should install the MySQL package, create a database and configure users. 
  
  * Community roles are tons of roles that have already been developed by other open source engineers out there. These roles are actually production ready, and dynamic to accomodate most of Linux flavours. They can be downloaded with Ansible Galaxy. 
  * We will be using a MySQL role developed by geerlingguy.

  * push changes to "dynamic-assignmnts"

<!-- Code Blocks -->
```bash

git init
git add .
git status
git commit -m "dynamic addition"
git push
```

![git-dynamic](https://i.imgur.com/zEVwDxw.png)

![git-dynamic-2](https://i.imgur.com/QgvU8cf.png)

* Inside roles directory create your new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql

<!-- Code Blocks -->
```bash
$ cd /var/lib/jenkins/workspace/ansible/roles
$ ansible-galaxy install geerlingguy.mysql

$ mv geerlingguy.mysql/ mysql
```
![mysql-role](https://i.imgur.com/EfDUn0X.png)

* Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website.

![mysql-edit](https://i.imgur.com/cEGuMHM.png)


* Upload the changes into your GitHub and create .

<!-- Code Blocks -->
```bash
git init
git add .
git checkout -b roles-feature
git commit -m "Commit new my sql role files into GitHub"
git push --set-upstream origin roles-feature
```
![git-push](https://i.imgur.com/wTbGfwD.png)

![git-push-2](https://i.imgur.com/Uzgju8K.png)

![git-push-3](https://i.imgur.com/zYUaU7L.png)

![git-push-4](https://i.imgur.com/sLJK7L4.png)
* Create a Pull Request and merge "dynamic-assignments" and "roles-feature" branches to main branch on GitHub.

![git-roles](https://i.imgur.com/dHeC8WH.png)

![git-roles-2](https://i.imgur.com/oEeItSO.png)

![git-roles-3](https://i.imgur.com/qoG8gtp.png)

![git-roles-4](https://i.imgur.com/ROr2Avn.png)

![git-roles-5](https://i.imgur.com/O9uGTWv.png)

![git-roles-6](https://i.imgur.com/Q5GDVb0.png)

![git-roles-7](https://i.imgur.com/QkNVklr.png)

![git-roles-8](https://i.imgur.com/ne0TMa2.png)

![git-roles-9](https://i.imgur.com/Orobumt.png)


* Step 3: Load Balancer roles.
We want to be able to choose which Load Balancer to use, Nginx or Apache, so we need to have two roles respectively:

1. Nginx
2. Apache

![roles-create](https://i.imgur.com/ZCSfkDU.png)

Important Hints:

1. Since you cannot use both Nginx and Apache load balancer, you need to add a condition to enable either one – this is where you can make use of variables.

2. Declare a variable in defaults/main.yml file inside the Nginx and Apache roles. Name each variables enable_nginx_lb and enable_apache_lb respectively.

3. Set both values to false like this enable_nginx_lb: false and enable_apache_lb: false.

4. Declare another variable in both roles load_balancer_is_required and set its value to false as well

5. Update both assignment and site.yml files respectively

loadbalancers.yml file
<!-- Code Blocks -->
```bash
- hosts: lb
  roles:
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }
```

site.yml file
<!-- Code Blocks -->
```bash
- name: Loadbalancers assignment
       hosts: lb
         - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required 
```

* Make use of env-vars\uat.yml file to define which loadbalancer to use in UAT environment by setting respective environmental variable to true.

* Activate load balancer, and enable nginx by setting these in the respective environment’s env-vars file.

<!-- Code Blocks -->
```bash
enable_nginx_lb: true
load_balancer_is_required: true
```

NOTE: The same must work with apache LB, so you can switch it by setting respective environmental variable to true and other to false.

![site-edit](https://i.imgur.com/ziNop3V.png)

![apache-role](https://i.imgur.com/zDbYI0S.png)

![apache-role-2](https://i.imgur.com/z6LJkTI.png)

![haproxy-config](https://i.imgur.com/Va4ZqAR.png)
* To test this, you can update inventory for each environment and run Ansible against each environment.

![playbook-check](https://i.imgur.com/gT4fgao.png)

![playbook-1](https://i.imgur.com/qkYhH50.png)

* updated config file

![haproxy-config-1](https://i.imgur.com/8rruJ9B.png)

![playbook-2](https://i.imgur.com/25uHw5Q.png)














