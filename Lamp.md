## Ansible LAMP apllication deployment

## Manual Commands
```
sudo apt update
sudo apt install apache2
sudo systemctl restart apache2
sudo apt install php libapache2-mod-php php-mysql
sudo nano /etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
sudo systemctl restart apache2
sudo apt install php-cli
sudo nano /var/www/html/info.php
<?php
phpinfo();
?>
sudo systemctl restart apache2
```
## Convert into ansible playbook
```
---
- hosts: webservers
  become: yes
  vars:
    package_apache: apache2
    packages_php_modules:
      - php
      - libapache2-mod-php 
      - php-mysql
      - php-cli
  tasks:
    - name: fail playbook on unsupported platforms
      fail:
        msg: 'This playbook is currently developed only for ubuntu 18'
      when: ansible_distribution != 'Ubuntu'
    - name: update and install apache
      apt: # https://docs.ansible.com/ansible/latest/modules/apt_module.html
        name: "{{ package_apache }}"
        update_cache: yes
        state: present
      notify:
        - debug message for apache installation
        - restart and enable apache2
    - name: install php modules
      apt:
        name: "{{ item }}"
        state: present
      loop: "{{ packages_php_modules}}"
      notify:
        - debug message for php modules
        - restart and enable apache2
    - name: copy info.php
      copy:
        src: info.php
        dest: /var/www/html/info.php
      notify:
        - restart and enable apache2
  handlers:
    - name: restart and enable apache2
      service:
        name: "{{ package_apache }}" 
        enabled: yes 
        state: restarted
    - name: debug message for apache installation
      debug:
        msg: "{{ package_apache }} is installed"
    - name: debug message for php modules
      debug:
        msg: "php modules are installed"
      
```

## Templates in Ansible 

* try to create a file with ``` readme.txt.j2 ```
```
Hello,
This file is created using Ansible Automation.

The operating system family = {{ ansible_os_family }} 

Operating system  = {{ ansible_distribution }} {{ ansible_distribution_version }} 

Thanks from {{ test_user }}
```
## Ansible Playbook
```
---
- hosts: all
  vars:
    test_user: devops team
  tasks:
    - name: copy the template with dynamic
      template:
        src: readme.txt.j2
        dest: /home/devops/readme.txt
```