## Ansible Adoc Commands
* In ansible if you want to do single task we can use ansible adhoc commands
``` ansible -b localhost -m package -a "name=tree state=present" ```
* Go to ``` /etc/ansible/hosts``` add server deatails
![Preview](./Images/hosts.png)
* In order to run ubuntu and centos as a single playbook we can use ``` package ``` module
![Preview](./Images/package.png)
