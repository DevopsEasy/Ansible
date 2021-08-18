### Basic Software Needed for Ansible

* Git Bash: [Refer Here](https://git-scm.com/downloads)
* Visual Studio Code: [Refer Here](https://code.visualstudio.com/download)
* Create AWS Free Tier Account

### Ansible Master & Node
* In Yesterday we 2 ubuntu machines one we converted as Master and other as Node.
* Let us take one more machine (Centos).

## Manual Steps of Installation 

## Ubuntu Steps
```
sudo apt install git -y
sudo apt install elinks -y
```

## Centos Steps
```
sudo yum install git -y
sudo yum install elinks -y
```

### Convert Ubuntu Steps into Ansible Playbook

```yaml
---
- hosts: ubuntu
  become: yes
  tasks:
    - name: install a software git
      apt:
        name: git
        state: present
    - name: install a software elinks
      apt:
        name: elinks
        state: present
```

### Convert Centos Steps into Ansible Playbook
```yaml
---
- hosts: ubuntu
  become: yes
  tasks:
    - name: install a software git
      yum:
        name: git
        state: present
    - name: install a software elinks
      yum:
        name: elinks
        state: present
```

### Inventory FIle (hosts)
```
[ubuntu]
ip_address
[centos]
ip_address
```

## Now run the playbook
```
ansible-playbook -i hosts ubuntu.yml
ansible-playbook -i hosts centos.yml
```

## Login to node to verify

### Next class topics
* is it possible to write single playbook for ubuntu and centos?
* one opensourse application deplyment using Ansible.
