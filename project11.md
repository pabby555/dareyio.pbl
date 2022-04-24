# Project 11
## Title: ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10
### TASK: 
This Project will use Ansible Configuration Management to automate routine tasks by writing code using the YAML declarative language.

Ansible Client as a Jump Server (Bastion Host)
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface.

The task will be to Install and configure Ansible client to act as a Jump Server/Bastion Host, and create a simple Ansible playbook to automate servers configuration

The Architecture will change, having the Virtual Private Network (VPC) divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

![architecture](https://i.imgur.com/a0prL3P.png)

#### Implementation
1.  - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

* Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

![jenkins](https://i.imgur.com/p1tOlT1.png)

* Create a new repository and name it "ansible-config-mgt" in GitHub.

![repo](https://i.imgur.com/wBP7PX4.png)

![repo-2](https://i.imgur.com/HSwaMrn.png)

![repo-3](https://i.imgur.com/daPMMv0.png)

* Install Ansible on "Jenkins-Ansible" Instance and check its version.
<!-- Code Blocks -->
```bash
$ sudo apt install ansible
$ sudo ansible --version
```
![ansible-install](https://i.imgur.com/4DM6MGs.png)

![ansible-version](https://i.imgur.com/Y0d1GyK.png)

* Configure Jenkins build job to save your repository content every time you change it. Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

![jenkins-project](https://i.imgur.com/8yHG20b.png)

![jenkins-project-2](https://i.imgur.com/1XLsn0S.png)

![jenkins-project-3](https://i.imgur.com/X4P5fcg.png)

* Configure Webhook in GitHub and set webhook to trigger ansible build.

![jenkins-project-4](https://i.imgur.com/70Bp3cD.png)

![jenkins.webhook](https://i.imgur.com/NJkQDsv.png)

![jenkins.webhook-2](https://i.imgur.com/76HdVpk.png)

![jenkins-project-5](https://i.imgur.com/rxB6Cw5.png)

![jenkins-webhook-3](https://i.imgur.com/4Zw5S6y.png)

![jenkins-webhook-4](https://i.imgur.com/fTHpg72.png)


* This build does not produce anything and it runs only when we trigger it manually. we need to configure. Click "Configure" your job/project and add these two configurations:

1. Configure triggering the job from GitHub webhook:

2. Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

![jenkins-config](https://i.imgur.com/YqmQsON.png)

![jenkins-config-2](https://i.imgur.com/pwv0CHd.png)

![jenkins-config-3](https://i.imgur.com/m2YdPNl.png)

* Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

![jenkins-test](https://i.imgur.com/RJIKpCd.png)

* Test the setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder.
<!-- Code Blocks -->
```bash
$ ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```

![jenkins-job](https://i.imgur.com/ce72gqW.png)

Present Architecture:

![architecture-2](https://i.imgur.com/ZtXLclC.png)

* Prepare your development environment using Visual Studio Code, After you have successfully installed VSC, configure it to connect to your newly created GitHub repository, clone down your ansible-config-mgt repo to your Jenkins-Ansible instance.

![visual-studio](https://i.imgur.com/l3ZD7K5.png)

* Begin Ansible Development, create a new branch that will be used for development of a new feature.

![automate](https://i.imgur.com/f4EKPEW.png)

* make changes in jenkins to reflect this new branch.

![jenkins-update](https://i.imgur.com/O68upxK.png)

* Create a directory and name it "playbooks" – it will be used to store all your playbook files.

![jenkins-build](https://i.imgur.com/wwy1DAM.png)

![jenkins-build-3](https://i.imgur.com/5nUQo79.png)

![jenkins-build-4](https://i.imgur.com/S468oKu.png)

![jenkins-build-8](https://i.imgur.com/GuRuN3E.png)



* Create a directory and name it inventory – it will be used to keep your hosts organised.

![jenkins-build-5](https://i.imgur.com/Jt4aNCN.png)

![jenkins-build-6](https://i.imgur.com/flM2Vmv.png)

![jenkins-build-7](https://i.imgur.com/5xsQhFT.png)

![jenkins-build-8](https://i.imgur.com/pWYleQE.png)

* Within the playbooks folder, create your first playbook, and name it common.yml

![jenkins-build-9](https://i.imgur.com/W9797Nm.png)

![jenkins-build-10](https://i.imgur.com/bHXq6Vg.png)

![jenkins-build-11](https://i.imgur.com/SK9svQw.png)

![jenkins-build-12](https://i.imgur.com/SUmrDzq.png)

* Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

![jenkins-build-13](https://i.imgur.com/PrCOpj8.png)

![jenkins-build-14](https://i.imgur.com/4C6hL7n.png)

![jenkins-build-15](https://i.imgur.com/YfhlqyL.png)

![jenkins-build-16](https://i.imgur.com/N2UkN3y.png)

* Setup SSH agent and connect VS Code to your Jenkins-Ansible instance, Confirm the key has been added, ssh into your Jenkins-Ansible server using ssh-agent.
<!-- Code Blocks -->
```
$ ssh-add <path-to-private-key>

$ ssh-add -l

$ ssh -A ubuntu@public-ip-of-jenkins-servers

```

![ssh-add](https://i.imgur.com/cgL8CtZ.png)

* Update your inventory/dev.yml file 

[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'

![jenkins-dev-yml](https://i.imgur.com/sxlbRW7.png)

* Create a Common Playbook, give Ansible the instructions on what needs to be performed on all servers listed in inventory/dev.yml. In common.yml playbook, write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with this code.

---
- name: update web and nfs servers
  hosts: webservers, nfs
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb, db
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt:
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest


* Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

![github-push-1](https://i.imgur.com/nChYUhm.png)

![github-push-2](https://i.imgur.com/2IX1W2c.png)

![github-push-3](https://i.imgur.com/FWbXEok.png)

![github-push-4](https://i.imgur.com/tBrck1e.png)

![github-push-5](https://i.imgur.com/COQoxyL.png)

![github-push-6](https://i.imgur.com/5aykl2p.png)

![github-push-7](https://i.imgur.com/CstI9xs.png)

![github-push-8](https://i.imgur.com/DYRfOjE.png)

![github-push-9](https://i.imgur.com/upOQYwM.png)

![github-push-10](https://i.imgur.com/qw2hfaH.png)

* run playbook from /var/lib/jenkins/workspace/ansible. make sure private key is present in /home/ubuntu/.ssh/id_rsa in jenkins server.

![jenkins-ssh](https://i.imgur.com/JcXja2S.png)

<!-- Code Blocks -->
```
$ ansible-playbook -i inventory/dev.yml playbooks/common.yml --syntax-check

$ ansible-playbook -i inventory/dev.yml playbooks/common.yml --check

$ ansible-playbook -i inventory/dev.yml playbooks/common.yml --check
```
![ansible-check](https://i.imgur.com/srKRc4i.png)

![ansible-playbook-1](https://i.imgur.com/jptHYHF.png)

![ansible-playbook-2](https://i.imgur.com/v9LEkkQ.png)

![ansible-playbook-3](https://i.imgur.com/SsKj1bK.png)

![ansible-playbook-4](https://i.imgur.com/6aIpUtR.png)

* Go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version

![wireshark](https://i.imgur.com/xg446Jx.png)

Current Architecture
![ansible_architecture-2](https://i.imgur.com/nqABhFb.png)

























