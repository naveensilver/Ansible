## Ansible Modules Modules :

Module is a small program that performs actions on a local machine, application programming interface (API), or remote host

Ansible has a large number of built-in modules that can be used in playbooks and ad-hoc commands.

* Module is a reusable, standalone script that ansible runs on your behalf, either locally or remotely.

* Modules are designed to be simple, flexible, and easy to use.

These modules cover a wide range of tasks, including system administration, application deployment, network automation, and more. 

### Some examples of Ansible modules :

* file: This module is used to manage files and directories on a remote system.

* copy: This module is used to copy files from the control node to remote systems.

* yum/apt: These modules are used to install and manage packages on remote systems.

* service: This module is used to manage services on remote systems.

* user/group: These modules are used to manage users and groups on remote systems.

* shell/command: These modules are used to execute shell commands on remote systems.

* template: This module is used to render templates and copy them to remote systems.

* git: This module is used to manage git repositories on remote systems.

* debug: This module is used to print debug information during playbook execution.

* wait_for: This module is used to wait for a certain condition to be true before continuing with playbook execution


### Module Commands :

* To display Ansible Module list
```
ansible-doc -l 
```
* To get Full information about module
```
ansible-doc <ModuleName>
```
* To Display details of Copy module
```
ansible-doc -l | grep "copy"
```
