## YUM Module :

YUM Module is used to manage packages on Red Hat-based systems. It can be used to ```install, update, or remove``` packages.

### ``Write a Playbook To Install, Update and Remove Package in All Host Nodes using yum module``

* Create A YML File : `` vi yum.yml``

```
---
- hosts: all
  user: ansible
  become: true 
  connection: ssh

  tasks:
    - name: Install package
      yum:
        name: <PackageName>
        state: present 

    - name: Update package
      yum:
        name: <PackageName>
        state: latest 

    - name: Uninstall/Remove package
      yum:
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
      yum:
        name: httpd
        state: present

    - name: Update httpd package
      yum:
        name: httpd
        state: latest

    - name: Uninstall/Remove httpd package
      yum:
        name: httpd
        state: absent

```
* Execute Playbook Using Below Command :
    ```
    ansible-playbook <playbookName>.yml
    ```
