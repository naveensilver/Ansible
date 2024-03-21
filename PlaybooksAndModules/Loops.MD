## Loops :
Loops are a powerful feature in Ansible that allow you to perform repetitive tasks more efficiently and with less code. They allow you to iterate over a list of items and execute a set of tasks for each item in the list.

Loops are used to perform multiple users, groups and files.. etc

### Sample loops playbook :
```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: installing multiple tools using loops
      action: yum name='{{item}}' state=present
      with_items:    #pre define module 
         - git       
         - java
         - tree
```
```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: create multiple users using loops
      user: name='{{item}}' state=present
      with_items:    #pre define module 
         - jaanu       
         - smiley
         - cutie
```

Note : In the above playbook, the packages will install one by one and 'with_items' is pre-defined module.

### In Ansible, loops can be defined in different ways:

1. Using the "loop" keyword:

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  vars:
    packages:
      - httpd
      - nginx
      - mysql

  tasks:
    - name: Install packages
      yum:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"

```

2. Using the "with_items" keyword:

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  vars:
    directories:
      - /tmp/dir1
      - /tmp/dir2
      - /tmp/dir3

  tasks:
    - name: Create directory
      file:
        path: "{{ item }}"
        state: directory
      with_items: "{{ directories }}"

```

3. Using a loop control variable:

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  vars:
    users:
      - name: user1
        password: password1
      - name: user2
        password: password2
      - name: user3
        password: password3

  tasks:
    - name: Create user
      user:
        name: "{{ item.name }}"
        password: "{{ item.password }}"
      loop: "{{ users }}"
      loop_control:
        label: "{{ item.name }}"

```


