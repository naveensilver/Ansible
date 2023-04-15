## TAGS Module :

Tags Module is used to execute a particular task in the playbook. 

* We can ignore/skip particular tasks.
* We can a run particular tasks.

Sample Tags Playbook :

```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh

  tasks:
    - name: install git
      action: yum name=git state=present
      tags: git

    - name: install java
      yum: name=java state=present
      tags:
        - java

    - name: install maven
      command: yum install maven -y
      tags: maven

```

In this playbook, we have three tasks that install different packages. Each task has been assigned a tag, allowing us to selectively run or skip them based on their tags.

## Tags Commands :

* To Run Specific Tags, we can use "--tags"
```
ansible-playbook <fileName>.yml --tags "tagName1, tagName2"
```

* To Skip Specific Tag, we can use "--skip-tags" 
```
ansible-playbook <fileName>.yml --skip-tags "tagName"
```

* To display all tags available in playbook :
```
ansible-playbook filename.yml  --list-tags
```
