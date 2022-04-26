# Project 12
## Title: ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)
### TASK: 
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.
In this project, things will be moved around a little bit in the code, but the overal state of the infrastructure remains the same. Work will continue with ansible-config-mgt repository, improvements will be made on the present code. The ansible code will be refactored, assignments will be created, and will also learn how to use the imports functionality. The purpose of Imports is to allow effective re-use of previously created playbooks in a new playbook – it will also allow proper organization of tasks and they can be reused them when needed.

* Step 1 – Jenkins job enhancement. Make some changes to the Jenkins job. Presently, every new change in the codes creates a separate directory which is not very convenient when you want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. We will enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin. 

* In Jenkins-Ansible server, create a new directory called "ansible-config-artifact". This is where all artifacts will be stored after each build.

<!-- Code Blocks -->
```bash
$ sudo mkdir /home/ubuntu/ansible-config-artifact
```
  ![artifact-folder](https://i.imgur.com/CA34gPd.png)

* Change permissions to this directory, so Jenkins could save files there

<!-- Code Blocks -->
```bash
$ sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact
```
  ![artifact-folder-2](https://i.imgur.com/kSLbelr.png)

* Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![jenkins-1](https://i.imgur.com/qAdKjI7.png)

![jenkins-2](https://i.imgur.com/LqYe7Zy.png)

![jenkins-3](https://i.imgur.com/zzUEYxc.png)

* Create a new Freestyle project and name it save_artifacts.

![jenkins-4](https://i.imgur.com/ITWDiK4.png)

![jenkins-new](https://i.imgur.com/iP2mNLq.png)

![jenkins-new-2](https://i.imgur.com/Vw1AAOi.png)

![jenkins-new-3](https://i.imgur.com/RA3GyQU.png)

* Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

* The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

![jenkins-build](https://i.imgur.com/w1XsxKw.png)

* Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

![visual-code](https://i.imgur.com/U15v4Zc.png)

![visual-code-2](https://i.imgur.com/zkpXZYw.png)

![visual-code-3](https://i.imgur.com/nwAaD3b.png)![visual-code-4](https://i.imgur.com/3qXa0zk.png)

![visual-code-5](https://i.imgur.com/jeybXdR.png)

* If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

![jenkins-build-4](https://i.imgur.com/sFgD54H.png)

* Step 2: Refactor Ansible code by importing other playbooks into site.yml.

* Create a new branch, name it "refactor".

* Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously.

<!-- Code Blocks -->
```bash
$ sudo touch /var/lib/jenkins/workspace/ansible/playbooks/site.yml
```
![jenkins-playbook](https://i.imgur.com/bfUvgAE.png)

* Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization. It is not an Ansible specific concept.

<!-- Code Blocks -->
```bash
$ sudo mkdir /var/lib/jenkins/workspace/ansible/static-assignments
```
![jenkins-playbook-2](https://i.imgur.com/p2Xvwuq.png)

* Move common.yml file into the newly created static-assignments folder.

<!-- Code Blocks -->
```bash
$ sudo mv /var/lib/jenkins/workspace/ansible/playbooks/common.yml /var/lib/jenkins/workspace/ansible/static-assignments
```

![jenkins-playbook-3](https://i.imgur.com/BgVP7qC.png)

![jenkins-playbook-4](https://i.imgur.com/jz8eIUf.png)

* Inside site.yml file, import common.yml playbook.
<!-- Code Blocks -->
```bash
---
- hosts: all
- import_playbook: ../static-assignments/common.yml
```

* Run ansible-playbook command against the dev environment. Create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.
<!-- Code Blocks -->
```bash
---
- name: update web and nfs servers
  hosts: webservers, nfs
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: delete wireshark
      yum:
        name: wireshark
        state: absent

- name: update db server
  hosts: db
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: delete wireshark
      yum:
        name: wireshark
        state: absent
        autoremove: yes
        purge: yes
        autoclean: yes


- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: absent
        autoremove: yes
        purge: yes
        autoclean: yes

...
```

![jenkins-playbook-7](https://i.imgur.com/BKum5Ta.png)

* update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:
<!-- Code Blocks -->
```bash
---
- hosts: all
- import_playbook: ../static-assignments/common-del.yml
```

<!-- Code Blocks -->
```bash
$ cd /var/lib/jenkins/workspace/ansible/

$ ansible-playbook -i inventory/dev.yml playbooks/site.yml --syntax-check

$ ansible-playbook -i inventory/dev.yml playbooks/site.yml --check

$ ansible-playbook -i inventory/dev.yml playbooks/site.yml
```
![jenkins-playbook-8](https://i.imgur.com/k5DDSlz.png)

![jenkins-playbook-9](https://i.imgur.com/JKqVSBt.png)

![jenkins-playbook-10](https://i.imgur.com/uUalvWS.png)

![jenkins-playbook-11](https://i.imgur.com/qFWWyb4.png)

* Make sure that wireshark is deleted on all the servers by running wireshark --version

![wireshark](https://i.imgur.com/5NmG38K.png)

![wireshark-2](https://i.imgur.com/e3ejVwr.png)


* Step 3: Configure UAT Webservers with a role ‘Webserver’

* Launch 2 fresh EC2 instances using RHEL 8 image, uat servers with names – Web1-UAT and Web2-UAT.

* create a directory called roles in jenkins server in path "/var/lib/jenkins/workspace/ansible"

<!-- Code Blocks -->
```bash
$ sudo mkdir var/lib/jenkins/workspace/ansible/roles
$ cd var/lib/jenkins/workspace/ansible/roles
```
![roles](https://i.imgur.com/qBHLIhZ.png)

* create a role named "webserver"
<!-- Code Blocks -->
```bash
$ ansible-galaxy init webserver
```
![roles](https://i.imgur.com/SeBgHGy.png)

![roles-2](https://i.imgur.com/pygEPXu.png)

![roles-3](https://i.imgur.com/5SukMeD.png)

![roles-4](https://i.imgur.com/2gv9Yhq.png)

* Update your inventory ansible/inventory/uat.yml file with IP addresses of your 2 UAT Web servers

![roles-5](https://i.imgur.com/YiBXKc7.png)

* Ensure you are using ssh-agent to ssh into the Jenkins-Ansible instance

* In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory.roles_path    = /var/lib/jenkins/workspace/ansible/roles, so Ansible could know where to find configured roles.

![roles-6](https://i.imgur.com/rOkSEOI.png)

Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

- Install and configure Apache (httpd service)
- Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
- Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers. 
- Make sure httpd service is started

main.yml file:

<!-- Code Blocks -->
```bash
---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present

- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present

- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/<your-name>/tooling.git
    dest: /var/www/html
    force: yes

- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/

- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started

- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent
```

* Step 4: Reference ‘Webserver’ role

* Within the static-assignments folder, create a new assignment for uat-webservers "uat-webservers.yml". This is where you will reference the role.


<!-- Code Blocks -->
```bash
---
- hosts: uat-webservers
  roles:
     - webserver
```

![roles-ref](https://i.imgur.com/pyVN4xK.png)

* The entry point to our ansible configuration is the site.yml file. Therefore, uat-webservers.yml role need to be referenced inside site.yml.

* Step 5: Commit the changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to the Jenkins-Ansible server into /home/ubuntu/ansible-config-mgt/ directory. 

![git-commit](https://i.imgur.com/rDqXa6U.png)

![git-commit-2](https://i.imgur.com/c0wxujN.png)

![git-commit-3](https://i.imgur.com/H1ytN3T.png)

![git-commit-4](https://i.imgur.com/epRVTnt.png)

![git-commit-5](https://i.imgur.com/2V15VJw.png)

![git-commit-6](https://i.imgur.com/lEFL9ph.png)

![git-jenkins](https://i.imgur.com/0EaGQQE.png)

* Run the playbook against your uat inventory.

<!-- Code Blocks -->
```bash
$ sudo ansible-playbook -i /var/lib/jenkins/workspace/ansible/inventory/uat.yml /var/lib/jenkins/workspace/ansible/playbooks/site.yml --syntax-check

$ sudo ansible-playbook -i /var/lib/jenkins/workspace/ansible/inventory/uat.yml /var/lib/jenkins/workspace/ansible/playbooks/site.yml --check

$ sudo ansible-playbook -i /var/lib/jenkins/workspace/ansible/inventory/uat.yml /var/lib/jenkins/workspace/ansible/playbooks/site.yml
```
![playbook](https://i.imgur.com/LYOf7NP.png)

![playbook-5](https://i.imgur.com/yJ9M2SZ.png)

![playbook-6](https://i.imgur.com/6pMMB4O.png)

* Try to reach UAT Web servers on web browser with public IP Addresses or public DNS names.

http://Web1-UAT-Server-Public-IP-or-Public-DNS-Name/index.php

http://Web1-UAT-Server-Public-IP-or-Public-DNS-Name/index.php

![web-browser](https://i.imgur.com/3I9GghT.png)

![web-browser-2](https://i.imgur.com/PXqmEXx.png)

Current Architecture:

![project12_architecture](https://i.imgur.com/2jFPwHg.png)


















