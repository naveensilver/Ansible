## Shell module : 

It is used to execute particular command.

* To print date of host nodes 
```
ansible all -m shell -a 'date'   
```
* To Print update from all host machine
```
ansible all -m shell -a 'uptime' 
```
* To print the OS Version 
```
ansible all -m shell -a 'cat /etc/*release' 
```
* Kind of mount on all the machines
```
ansible all -m shell -a 'mount' 
```
* To check the sshd service status of all host node
```
ansible all -b -m shell -a 'service sshd status' 
```
* To Print disk space of all Host Server
```
ansible all -m shell -a 'df -h' 
```
* To display disk space use for all the nodes which are from webserver group
```
ansible webserver -a 'free -m' 
```
