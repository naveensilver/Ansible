Variables :

Variables in Ansible are used to store data that can be used in playbooks and templates. 

Variables are used to make the playbooks more reusable, dynamic and easier to maintain. 

Variables can be defined at various levels like inventory level, playbook level, role level or even at the task level.

In Ansible, variables are defined using YAML syntax and can be of various types such as strings, lists, dictionaries, and boolean values. They can also be defined using Jinja2 templates.

Variable are used to declare the values. In the playbook we can declare variables.

In Ansible, there are several ways to configure and use variables, including:

1) Command-Line Variable

we can declare value to the variable in Run time while executing the playbook.
```
---
- hosts: all
  become: true
  tasks:
    - name: install httpd package
      yum:
        name: "{{package_name}}"       #variable
        state: latest

   - name: start httpd service
      service:
        name: "{{package_name}}" 
        state: started
```

Here, we are declaring value to the variable in run time
```
ansible-playbook <filename> --extra-vars package_name=httpd
```

2) Playbook Variable :

we can declare the variable value within the playbook itself.

```
---
- hosts: all
  become: true
  
  vars:						#Variable value 
      package_name: httpd

  tasks:
    - name: install httpd package
      yum:
        name: "{{package_name}}"       #variable
        state: latest

   - name: start httpd service
      service:
        name: "{{package_name}}" 
        state: started
```

3) We can declare Group Variable
```
		    /etc/ansible/group_vars
				  >> vi <groupname>.yml  >> package_name: <httpd/nginx>
```
4) We can declare Host Variable  
```
		    /etc/ansible/hosts_vars
			    >> vi <172.168.10.3>.yml  >> package_name: <httpd/nginx>
```
-------------------------------
3. Group Variable : 

Group variables allow you to define variables that apply to multiple hosts or to all hosts within a group in your inventory. 

This can be useful if you have a set of hosts that require similar configuration or if you want to apply a specific setting to multiple hosts at once

To define group variables, you can create a file named "group_vars/group_name.yml" in your Ansible project directory. Here, "group_name" is the name of the group as defined in your inventory file. For example, if you have a group named "web_servers" in your inventory file, you can define group variables for this group in a file named "group_vars/web_servers.yml".

In the "group_vars/group_name.yml" file, you can define variables as key-value pairs in YAML format. For example, to set the "http_port" variable to "8080" for all hosts in the "web_servers" group, you can define the following variable in "group_vars/web_servers.yml":

```
http_port: 8080
```
You can then use this variable in your playbooks or roles by referencing it as {{ http_port }}.

Note : you define the same variable in both host and group variables, the host variable takes precedence over the group variable for that specific host.

Example :
Scenario,
For webserver group i want to install "nginx" Package
For dbservers i want to install "tomcat" Package 
For all i want to install "httpd" package 

We can achieve this using "Group Variable" 

Group vars directory should be created at host inventory location 

	>> Go to inventory location $ cd /etc/ansible 
	>> Create a group directory $ sudo mkdir group_vars 
	>> Go inside group_vars $ cd group_vars and create group files 
		>> sudo vi <groupName>.yml 
		  >> sudo vi webserver.yml  
        >> package_name: nginx >> :wq
		  >> sudo vi dbserver.yml 
        >> package_name: tomcat >> :wq
		  >> sudo vi all.yml 
        >> package_name: httpd >> :wq

In the Working DIrectory Create a main playbook, 

vi package_name.yml 
```
- hosts: all / webserver / dbserver
  become: true
  tasks:
    - name: install package
      yum:
        name: "{{package_name}}"       #variable
        state: latest

   - name: start service
      service:
        name: "{{package_name}}" 
        state: started

   - name: print package 
     debug:               #Debug module is used to print the output message.
        msg: "{{package_name}}"
```

4) Host variable :

Host variables in Ansible allow you to define variables for specific hosts or groups of hosts in inventory files. This is useful when you want to define host-specific configurations or parameters.

Here's an example of how to define host variables:

* Open your inventory file, which is typically located at "/etc/ansible/hosts".

* Add the host or group of hosts for which you want to define variables. For example:

```
[web]
webserver1
webserver2
```
* Add variables for the host or group of hosts using YAML syntax. For example:
```
[web:vars]
http_port=80
https_port=443
```
In this example, we've defined two variables "http_port" and "https_port" for the web group of hosts.

* Save the inventory file.

* You can now use these variables in your playbooks or tasks like this:
```
- name: Example playbook
  hosts: web
  tasks:
    - name: Example task
      debug:
        msg: "HTTP port is {{ http_port }}, HTTPS port is {{ https_port }}"
```

This will output the values of the "http_port" and "https_port" variables for each host in the web group.


Note : you can also define host variables in separate YAML files, using the "host_vars" directory structure. For example, you could create a file called webserver1.yml in the host_vars directory with the following content:
```
http_port: 8080
```
This would override the "http_port" variable defined in the inventory file for the 'webserver1' host.


Example : Write a playbook to set a variable for a particular host machine ?

If you want to set a variable for particular host machine then we use Host variables. 

* Host Varibale are Server specific variables.
* In one group we will have multiple servers 
* For every host if we can separate variables using "host_vars".




	create a files with hostname or IP
	

To define host variables, you need to create a host-specific file in the "host_vars" directory.

$ mkdir /etc/ansible/host_vars   (Follow steps same as Group variable)

The file should be named with the hostname or IP address of the host, and it should be a YAML file with variable definitions. 

For example, if you have a host with the name "webserver1", you would create a file called "webserver1.yml" in the "host_vars" directory
i.e, vi etc/ansible/host_vars/webserver1.yml (OR) vi etc/ansible/host_vars/182.164.54.yml with the following content

```
---
http_port: 80
```
In this example, we have defined variables specific to the "webserver1" host - "http_port". These variable can then be used in your playbook or tasks by referencing them with the "{{ }}" syntax. For example, to use the http_port variable in a task, you might have:

```
- name: Configure web server
  template:
    src: templates/httpd.conf.j2
    dest: /etc/httpd/conf/httpd.conf
  vars:
    http_port: "{{ http_port }}"
```

In this task, we are using the http_port variable in the "vars" section to set the value of the "http_port" variable used in the "template" module.

Using host variables in this way allows you to define configuration details specific to each individual host in your inventory, and to manage those details in a centralized and organized way.


Q) Difference Variable and Group Variable 

if you want to use the same value for all host nodes then we will go for variable.

If you want to change the variable value depends upon the group name then we need to go for Group Variable 




