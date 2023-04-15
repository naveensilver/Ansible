## Debug Module :

Debug Module is used to print out the values of variables and other information during playbook execution.
 
It is often used for troubleshooting or verifying the values of variables.

The `debug` module accepts several parameters, including the `msg` parameter which is used to specify the message that should be printed.

### Example :
```
- hosts: all
  vars:
    my_var: "Hello, World!"
  tasks:
    - name: Print variable value
      debug:
        msg: "The value of my_var is {{ my_var }}"
```

In this example, the `debug` module is used to print out the value of the `my_var` variable. The `msg` parameter is used to specify the message that should be printed along with the variable value. 

When this playbook is Executed, it will print out the following message:
```
TASK [Print variable value] ***************************************************************************************************************************************************************************************
ok: [all] => {
    "msg": "The value of my_var is Hello, World!"
}
```

Note: the variable value is included in the message, and is enclosed in double curly braces `{{ }}` to indicate that it is a variable.

### Some Other Examples Using Debug Module :

1. Write a playbook using debug module and using pre-defined variables.

```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh

  tasks: 
    - name: To Print the Slave OS 
      debug:   
        msg: "OS family for slave server is {{ansible_os_family}}"

    - name: To Print the Slave server total, free, used memory in MB Format  
      debug:   
        msg: "Slave server memory is {{ansible_memory_mb.real}}"

    - name: To Print the Slave server used memory in MB Format  
      debug:   
        msg: "Slave server memory is {{ansible_memory_mb.real.used}}"
```
## `Pre-defined Variable (Setup Modules)`

* To Represents OS of the machine
```
ansible_os_family
```
* To Represents Kernal version of machine
```
ansible_kernal 
```
* To Represents IP address of slave
```
ansible_default_ipv4
```
* To Represents No. of CPU's in Slave Server
```
ansible_processor_cores
```
* To Represents Connected devices
```
ansible_devices
```
* To Represents bit (32 or 64)
``` 
ansible_architecture
```


2. Write a playbook to print list of users while execution and define our own variable.
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh

  tasks: 
    - name: To Register list of users in "abc" variable
      command: cat /etc/passwd
      register: abc

    - name: To print users list while execution 
      debug: 
        msg: "the slave server users are {{abc.stdout}}"

```
In this example, First task `register` is used to store the task output values in `abc` variable. we are defining variable as `abc`.
In the 2nd task, we are using `debug` to print the variable information while executing the playbook.

3. Write a Playbook to install git on slave server. if git is installed in slave server, print msg as ' git is already installed' and if git is not installed "install git "

```
- name: Install Git on slave node
  hosts: slave
  become: true
  tasks:
    - name: Check if Git is installed
      command: git --version
      register: git_check
      ignore_errors: true

    - name: Install Git
      yum:
        name: git
        state: present
      when: git_check.rc != 0

    - name: Print message if Git is already installed
      debug:
        msg: "Git is already installed"
      when: git_check.rc == 0
```
In this playbook, we first check if Git is already installed by running the `git --version` command using the `command` module and registering the output in the `git_check` variable. We also use `ignore_errors: true` to prevent the playbook from failing if Git is not installed.

Next, we use the `yum` module to install Git if `git_check.rc` is not equal to `0`, which means Git is not installed.

Finally, we use the `debug` module to print a message saying that Git is already installed if `git_check.rc` is equal to `0`

4. Install Apache 
```
- hosts: all
  become: yes
  tasks:
    - name: Install Apache Web Server
      yum:
        name: httpd
        state: present
      register: install_apache
    - name: Restart Apache Web Server
      service:
        name: httpd
        state: restarted
      when: install_apache.changed
```

Explanation: This playbook installs Apache web server on the `web_servers` group of hosts. The `become: yes` statement is used to run the tasks with sudo privileges. The `yum` module is used to install the `httpd` package. The `register` statement is used to save the output of the task to the `install_apache` variable. The `service` module is used to restart the Apache web server only if it was installed or updated
