## COPY Module :

The Copy module in Ansible is used to copy files from the local machine to a remote machine
```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh

#1 Copy a file from the local machine to a remote machine:

  tasks:
    - name: Copy file
      copy:
        src: </path/to/local/file>
        dest: </path/to/remote/file>

#2 Copy a file from a remote machine to the local machine: 

    - name: Copy file
      copy:
        src: </path/to/remote/file>
        dest: </path/to/local/file>
        remote_src: yes

#3 Copy a file from local to remote machine with specific permissions:

    - name: Copy file
      copy:
        src: </path/to/local/file>
        dest: </path/to/remote/file>
        mode: "0644"

#4 Copy content to a file local to remote machine

    - name: Copy content to a file
      copy: content="Hello World \n DevOps" dest=/home/ansible/hello.txt

    - name: copy/insert content to a file 
      copy: 
        dest: /home/ansible/hello.txt
        content: |
          "adding some extra data into a file"        

```

* Execute Playbook Using Below Command :
    ```
    ansible-playbook <playbookName>.yml
    ```
