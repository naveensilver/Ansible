## APT Module :

APT module is used to manage packages on Debian-based systems. It can be used to `install, update, or remove` packages.

### ``Write a Playbook To Install, Update and Remove Package in All Host Nodes using apt module``

* Create A YML File : `` vi apt.yml``

```
---
- hosts: all
  user: ansible
  become: true 
  connection: ssh

  tasks:
    - name: Install package
      apt:
        name: <PackageName>
        state: present 

    - name: Update package
      apt:
        name: <PackageName>
        state: latest 

    - name: Uninstall/Remove package
      apt:
        name: <PackageName>
        state: absent      
```

* Execute Playbook Using Below Command :
    ```
    ansible-playbook yum.yml
    ```

    
Example : ``Write Ansible Playbook To install, Update & Remove Httpd Package in all Host Nodes using yum module ?``

```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh

  tasks:
    - name: Install httpd package
      apt:
        name: httpd
        state: present

    - name: Update httpd package
      apt:
        name: httpd
        state: latest

    - name: Uninstall/Remove httpd package
      apt:
        name: httpd
        state: absent

```
* Execute Playbook Using Below Command :
    ```
    ansible-playbook <playbookName>.yml
    ```
