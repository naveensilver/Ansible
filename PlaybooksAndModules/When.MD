## WHEN Module :

When Module is used to define the condition. It will execute the tasks based on specific conditions.

Tasks :

### 1. We can use 'when' to "skip" tasks based on conditions :

* Scenario1 ,
I have 2 VM's - Red Hat VM (yum) and Ubuntu VM (apt)
So, Can you write a playbook to install git in both VM's ?

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: installing git if os is RedHat (package is yum) 
      yum: name=git state=present
      when: ansible_os_family == "RedHat"

    - name: install git if os is Ubuntu (package is apt)
      command: apt install git -y
      when: ansible_os_family == "Ubuntu"

```
In this example, we are installing git on RedHat hosts and Maven on Ubuntu hosts. If a host does not meet the condition for a particular task, the task will be skipped.

* Scenario-2 ,

If client machine >> OS >> RedHat >> install httpd ,

If client machine >> OS >> Ubuntu >> install apache2,

If client machine >> OS >> Debian >> install git .

```
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: instal httpd if os is RedHat
      action: yum name=httpd state=present
      when: ansible_os_family == "RedHat"

    - name: install apache2 if os is Debian
      command: apt install apache2 -y
      when: ansible_os_family == "Debian"

    - name: install git if os is Ubuntu
      command: apt install git -y
      when: ansible_os_family == "Ubuntu"
```
In this Example, Task will perform only on suitable 'when' condition and remaining tasks will be skipped.

### 2. We can use 'when' to run task on particular condition :

```
---
- name: Example playbook with when
  hosts: all
  become: true
  tasks:
    - name: Install Apache web server on CentOS
      yum:
        name: httpd
        state: present
      when: ansible_distribution == 'CentOS' and ansible_distribution_major_version == '7'

```
In this example, we are using the 'yum' module to install the Apache web server, but we only want to install it on CentOS 7 hosts. We have added a 'when' condition that checks if the distribution is CentOS and the major version is 7. If the condition is true, the task will be executed.
