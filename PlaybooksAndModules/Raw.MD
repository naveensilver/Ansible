## Raw Module :

Raw Module used to execute a command directly on the remote server without having to use a specific module or write a script. 

It can be useful for running commands that don't have a corresponding module, or for running commands that require a certain level of control that may not be available with other modules

Sample playbook :
```
---
- name: Execute command with raw module
  hosts: all
  become: yes
  tasks:
    - name: install VIM Using Raw Module
      raw: yum install vim-common -y 

    - name: Execute command
      raw: ls -l /var/log

    - name: Execute command with specific shell
      raw: ps -ef | grep nginx
      args:
        executable: /bin/bash
      
    - name: Execute command and capture output
      raw: echo "Hello, world!"
      register: output

    - name: Display captured output
      debug:
        var: output.stdout_lines

```
