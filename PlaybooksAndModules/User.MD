## User Module :

The user module in Ansible is used to manage user accounts on remote hosts

### Sample Playbook using User Module :

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: create user
      user: name="silver" state=present

    - name: delete user 
      user: name="silver" state=absent remove=yes 
 
      #remote=yes is used to delete the user folder in "/home/"

```
Note : We can varify user in remote user `cat /etc/passwd`

### 1. Playbook to create a user using variables:
```
---
- name: Create user with variables
  hosts: all
  become: true

  vars:
    username: johndoe
    password: "{{ 'secretpassword' | password_hash('sha512', 65534) }}"
    comment: John Doe
    shell: /bin/bash
    createhome: yes

  tasks:
    - name: Create user account
      user:
        name: "{{ username }}"
        password: "{{ password }}"
        comment: "{{ comment }}"
        shell: "{{ shell }}"
        createhome: "{{ createhome | default(false) }}"

```

### 2. Playbook to create a user With-out variables:

```
---
- name: Create user without variables
  hosts: all
  become: true

  tasks:
    - name: Create user account
      user:
        name: johndoe
        password: "{{ 'secretpassword' | password_hash('sha512', 65534) }}"
        comment: John Doe
        shell: /bin/bash
        createhome: yes

```

### 3. Create and Delete Multiple users using Variables & Loops in single playbook 

```
---
- name: Create and  Delete Users with Variables
  hosts: all
  become: true

  vars:
    users:
      - user1
      - user2
      - user3

  tasks:
    - name: Create Users
      user:
        name: "{{ item }}"
        state: present
      loop: "{{ users }}"

    - name: Delete Users
      user:
        name: "{{ item }}"
        state: absent
      loop: "{{ users }}"
```

### 4. Create and Delete Multiple users using Loops in single playbook 

```
---
- name: Create and  Delete Users with loops
  hosts: all
  become: true

  tasks:
    - name: create user
      user: name='{{item}}' state=present
      with_items:
        - user1
        - user2
        - user3

    - name: delete user
      user: name='{{item}}' state=absent
      with_items:
        - user1
        - user2
        - user3

```

