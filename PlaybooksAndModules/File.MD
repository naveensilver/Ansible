## FILE MODULES :

### 1.Write a playbook to Create a single file with specific permissions, owner, and group using the file module:
```
- hosts: all
  user: ansible
  become: true
  connection: ssh 

  tasks:
    - name: Create a single file with specific permissions, owner, and group
      file:
        path: /root/file.txt 
        state: touch             # touch is used to create a file and directory used to create folder
        mode: '0644'
        owner: ansible   #user and group should present in slave server 
        group: root
```


### 2.Create multiple files with specific permissions, owner, and group using the file module:
```
- hosts: all
  user: ansible
  become: true
  connection: ssh 
  
  tasks:
    - name: Create a single file with specific permissions, owner, and group
      file:
        path: /root/file.txt
        state: touch
        mode: 751
        owner: ansible
        group: root

    - name: Create multiple files using file module & loop
      file:
        path: "{{ item.path }}"
        state: touch
      loop:
        - { path: './file1.txt' }
        - { path: '/root/file2.txt' }

    - name: creating mulitple files using file module with loop
      file:
        path: "{{item}}"
        state: touch
      loop:
        - /root/file3.txt
        - file4.txt
```


### 3.Creating multiple files and sets permission, user and group using VARIABLES With LOOPS

```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh
  vars:
    file_list:
      - path: "./file1.jpg"
        content: "This is the content for file1"
        mode: "421"
        owner: ansible
        group: ansible
      - path: "/root/file2.png"
        content: "This is the content for file2"
        mode: "771"
        owner: root
        group: root
  tasks:
    - name: Create multiple files using variables
      file:
        path: "{{ item.path }}"
        state: touch
        mode: "{{ item.mode }}"
        owner: "{{ item.owner }}"
        group: "{{ item.group }}"
        content: "{{ item.content }}"
      loop: "{{ file_list }}"
```
