## Call Playbook from Another Playbook
```yaml
---
- import_playbook: util.yml
- hosts: all
  become: yes
  tasks:
    - name: install apache
      package:
        name: apache2
        state: present
      notify: 
        - restart apache
  handlers:
    - name: restart apache
      service:
        name: apache2
        enabled: yes
        state: restarted

```
```yaml
---
- hosts: all
  become: yes
  vars:
    packages:
    - git
    - tree
    - nano
    - elinks
  tasks:
    - name: install packages
      package:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"

```

## Ansible Galaxy
Ansible Galaxy: [Refer Here](https://galaxy.ansible.com/)


## Ansible Role using Ansible Galaxy


# Sample Playbook of Role
```
---
- hosts: all
  become: yes
  roles:
    - geerlingguy.nodejs
```
``` ansible-playbook -i hosts role.yaml ```



## Creation of Role Structure using Ansible Galaxy

``` ansible-galaxy init <role_name> ```