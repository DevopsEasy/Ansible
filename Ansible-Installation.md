### Ansible Installation 

## Installing Ansible
* Ansible is written in Python, To run ansible we need python to be installed on Ansible Control Server (CM Server) and also nodes.

* Ansible Control Server can be installed on

Linux
macOS

* Nodes can be

Linux
MacOS
Windows
Network Switches
Routers

## Master Commands
```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
python --version
ansible --version
```

## Node Commands

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install python -y
```

## Steps to Follow:

* Create two ec2 machines with t2.micro in AWS
* Tag one machine as ACS(Master) and other as Node-1
* Login into ACS(Master) and become root user user
* Enable Password Based Authentication
```
vi /etc/ssh/sshd_config
change the contents
# PasswordAuthentication no  => PasswordAuthentication yes
service sshd restart
```
## Create a user devops and give sudo permissions
```
adduser devops
visudo
 # in the file opened
 devops  ALL=(ALL:ALL) NOPASSWD:ALL

# To test this
su devops
sudo apt-get update
# It should not ask for password and it should complete the command execution
```
## Login into ACS(Master) as devops and configure password less login into node-1
```
ssh devops@<acspublicip>
ssh-keygen
ssh-copy-id devops@<node1privateip>
```
* Do the following to test connectivity
```
sudo mv /etc/ansible/hosts /etc/ansible/hosts.orig
sudo vi /etc/ansible/hosts

# add node1-private ip and save the file
# check by executing
ansible -m ping all
```
## Sample Playbook
```
---
- name: installing apache server
  hosts: all
  become: yes
  tasks:
    - name: install apache2
      apt:
        name: apache2
        update_cache: yes
        state: present
```