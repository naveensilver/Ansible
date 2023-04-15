## Ansible Ad-Hoc Commands:

Ansible Ad-hoc commands are one-liners used to execute an action module on one or many hosts managed by Ansible.

Ad-hoc commands are used to executing tasks for immediate response.

Syntax:

Ansible Adhoc Command :
```
ansible <all/groupName/Host-Ip> -a "command"
```
Ansible Adhoc Module Command:
```
ansible <all/groupName/Host-Ip> -b -m <moduleName> -a <Argument>
```
Where, `<all/groupName/HostIp>` Represents Host Server, `-b` represents 'become=yes'`-m` represents 'Module' and `-a` represents 'Arguments'

### Here are some examples of Ansible Ad-hoc commands:

* Ping all hosts:
```
ansible all -m ping
```
* To check webserver files list 
```
ansible webserver -a "ls -al"
```
* Install Git in slave 
```
ansible dev -a " yum install git -y"
```
* Get the hostname of all hosts:
```
ansible all -m command -a 'hostname'
```
* Check the disk space of all hosts:
```
ansible all -m shell -a 'df -h'
```
* Install the Apache web server on all hosts:
```
ansible all -m yum -a 'name=httpd state=present'
```
* Restart the Apache web server on all hosts:
```
ansible all -m service -a 'name=httpd state=restarted'
```
* Copy a file to all hosts:
```
ansible all -m copy -a 'src=/path/to/local/file dest=/path/to/remote/file'
```
* Check the uptime of all hosts:
```
ansible all -m command -a 'uptime'
```
* Check the memory usage of all hosts:
```
ansible all -m shell -a 'free -m'
```
