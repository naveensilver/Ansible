## PIP Module :

Pip Module is used to manage the python libraries/dependencies on remote servers.

Pip can be used to install, upgrade, and uninstall Python packages on the target servers.

* Note: Python and Python-pip packages should be installed on remote servers before using Pip

Sample Pip Module Playbook :
```
---
- name: Install requests package
  hosts: all
  become: yes

  tasks:
    - name: install python and python-pip in host 
      command: yum install python python-pip -y 

    - name: Install NumPy package in host 
      pip:
        name: NumPy
        state: present

    - name: Install packages from requirements file 
      pip:
        requirements: </path/to/requirements.txt> 

```
