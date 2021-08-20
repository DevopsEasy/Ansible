## Ansible Adoc Commands
* In ansible if you want to do single task we can use ansible adhoc commands
``` ansible -b localhost -m package -a "name=tree state=present" ```
* Go to ``` /etc/ansible/hosts``` add server deatails
```
[ubuntu]
172.31.30.1
```
* In order to run ubuntu and centos as a single playbook we can use ``` package ``` module
```
---
- hosts: all
  become: yes
  tasks:
    - name: install a software git
      package:
        name: git
        state: present
    - name: install a software elinks
      package:
        name: elinks
        state: present

```
