## Git Module :

Git module used to manage Git repositories on remote servers. You can use it to clone repositories, pull updates, and manage branches and tags.

* Note : Git should be installed in Remote servers. 

Sample :
```
---
- name: Manage Git repository
  hosts: all
  become: yes

  tasks:
    - name: install git in remote servers
      yum: name=git state=present

    - name: Clone repository
      git:
        repo: https://github.com/example/repo.git
        dest: /opt/repo                           # it will create `repo` folder
        version: master

    - name: Checkout branch
      git:
        repo: /opt/repo
        version: <branch_Name>
        force: yes

    - name: Checkout tag
      git:
        repo: /opt/repo
        version: v1.0
        force: yes

    - name: Pull updates
      git:
        repo: /opt/repo
        update: yes
```
### 1. Write a playbook to clone the public Repo from Github Using Git Module 
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh

  tasks:
    - name: install git in remote servers
      yum: name=git state=present

    - name: clone "public repo" from Github
      git:
        repo: "https://github.com/naveensilver/Devops.git"
        dest: "/home/my_git_repo"

    - name: clone public repo "particular branch" code from Github
      git:
        repo: https://github.com/naveensilver/maven-web-app.git 
        dest: "/home/my_git_repo"
        version: master  # To get particular branch code 
```
      

### 2. Write a playbook to clone the Private repo from Github Using Git Module 

* Create a Github Token for Private Repository and Paste it inside Playbook
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh

  tasks:
    - name: install git in remote servers
      yum: name=git state=present

    - name: clone "Private repo" from Github
      git:
        repo: "https://<Paste-GIT-HUB-TOKEN>@github.com/naveensilver/Devops.git"
        dest: "/home/my_git_repo"
        version: master

```

