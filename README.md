# Ansible

* Ansible is a Configuration management tool.

* Ansible is managed by System administrator.

## System Administrator :

In Every Project, we use multiple machines to setup infrastructure for our application.

All these Machines are managed by System Administrator.

The roles and responsibilities of sys administrator is to manage configuration management for machines manually.

## Configuration Management :

* It is method through which we automate the admin tasks like creating user, installing s/w , adding, updating, deleting, data-backup.. anything that is required for our project.

* Configuration management tools turns your code into Infrastructure.

* So, your code would be testable, repeatable and versionable.

* Infrastructure refers to the composite of -
	- Software
	- Network 
	- People 
	- Process

If we have 100's of machines then Manual Configration Management will be difficult.

### Problem with this approach 

* Time consuming

* chance of getting error

* Repeatitive task

* To avoid this Manual Approach, Automation Configuration Management Tools came into Market, those tools are 

    - Ansible (Trending Tool)
    - Chef 
    - Puppet 

Ansible work based on 'Push' Mechanism

Chef & Puppet Tools work based on 'Pull' Mechanism.

Ansible :
========

• Ansible is one of the DevOps Configuration Management tool which is famous for its simplicity.

• Ansible is open-source software developed by Michael DeHaan and Its Owned by RedHat.

• Ansible is an Open-source IT configuration Management, Deployment & Orchestration Tool.

• Ansible is easy to deploy because it doesn’t use any agents or custom security infrastructure. Which means ansible works by connecting nodes through SSH.

• Ansible is automates the Orchestration service – means it follows master and slave concept 

• Ansible uses Playbooks to describes automate the jobs and Playbook uses YAML scripting language which is simple and easy to understand. Which works on Key-Value pair format. (YML/YAML – Yet Another Markup Language)

• Ansible Provides a way to define Infrastructure as code (IAC). Simply means that managing infrastructure by writing code rather than using manual process.

• Ansible is mainly designed for multi-tier deployments.

• Ansible uses the hosts file where one can group the hosts & can control the actions on a specific group in the playbook.

• Ansible GUI [Graphical user interface] is called Ansible-Tower. It was just drag and drop.

• Ansible was written in Python. Dependency of ansible is python.

• Ansible doesn’t require any software to be installed on other machines.

• Ansible’s ability to manage multiple system in parallel and makes well suited for large scale deployment.

• Ansible is widely used in IT industry for managing infrastructure.
 
• Ansible is used for diff tasks such as software installation and file management, service management etc.

• The Main Components of Ansible are Playbooks, Configuration Management and Deployment.

• Ansible uses a playbooks to automate deploy, manage, build, test and configure anything.

## HISTORY:

•	Ansible was first developed in Feb 2012 by Michael Dehaan. 

•	In 2013, Ansible taken over by RedHat.

•	Ansible is available for other OS like Oracle Linux, Debian, CentOS, RHEL.

•	Over the years, Ansible has added many features like security features, support for cloud providers and improved support for windows systems.

•	Now, Ansible is considered one of the leading automation tool in IT industry.

•	Ansible tool is used whether the servers are in On-prem or in the cloud.

•	Ansible turns your code into infrastructure.

## Ansible Features :

* Ansible manages machines in an Agent-less manner SSH.

* Ansible Written in Python and Hence provides a lot of python's functionality. 

* YAML based playbooks. (YML is Human and Machine readable)

* User SSH for secure connections.

* Follows Push based Architecture for sending configurations.

## WHY ANSIBLE :

•	Ansible Automate and Simplify - the repetitive, complex operations and long operations.

•	Ansible is open source, saves time as well as human efforts & is easy to implement.

•	Ansible architecture is easy and effective, it works by connecting to your nodes & pushing small programs to them.

•	Ansible is push based architecture & doesn’t need agents running on the client nodes.

•	Ansible works over SSH. So, nothing needs to install on client machines and it need only a text editor and command line tools are usually enough to get your work done and other tools like chef/puppet need to install agent on the client machines. When we need to perform a task.

•	Ansible is light weight, easy to use and speed deployment compared to other tools.

•	Ansible used when you have multiple server which needs to be configure the same setup in all servers.

•	While doing one to one server their might be a chance to miss some configuration steps in some servers.

•	That’s why we use automation tools 

•	It follows Describe The Desired State Of The System 

## ANSIBLE USES :

1. Agentless Architecture: Ansible doesn’t require any extra software on your remote nodes. Which makes it easy set up and use and It helps to keep the installation clean.

2. Open-source: Ansible one of powerful DevOps tool which is open-source.

3. Simple: Ansible uses the simple syntax written in YML called playbooks. YAML is simple and human readable and doesn’t require any coding skills.

4. Ease of use: One can configure and manages the complex infrastructure solutions very easily. 

5. Powerful & Flexible: Ansible has powerful features that can enable even the most complex IT workflows. 

6. Efficient: No extra software on your server means more resources for your applications.

7. Secure: Ansible uses SSH connection which is secure and encrypted.

8. Configuration Management: used to automate configuration managemet tasks such as provisionig, application deployment and infastructure management.

9. Scalability: Ansible can manage a large number of systems simultaneously, making it ideal for large-scale deployments.

10. Open-source: Free to use and has large community contributors who regularly contribute to its development.

11. Integrate with other tools: Ansible can be integrated with other tools such as Docker, Kubernetes and AWS, which makes it varsatile and easy to use in a variety of environments.

### Push vs Pull Based Architecture

* Puppet and Chef are Pull Based Architecture

	- Agents on the other server periodically checks for the configuration information from central server (Master)

* Ansible is Push Based Architecture

	- Central server pusher configuration information on target servers.

### What Ansible Can do ?

	1) Configuration Management 
	2) App Deployment 
	3) Contineous Delivery 

## Ansible Work-flow / Ansible Architecture 

![image](https://user-images.githubusercontent.com/120022254/231712373-be2dae08-5403-49b8-a015-828a6d24e224.png)

Ansible uses Master-Slave Architecture.

In Which Machine, Ansible is installed that machine is called as Master Node / Control Node or Ansible Server. 

The Machines, which are managed by Master node they are called as Host Node / Slave node.

Inside Master Node, we are going to write 'Playbooks' to Automate our configuration Management.

Playbooks are written in YAML [Yet Another Markup Language] Language.

Ansible works by connecting to your nodes and pushing out a small program called 'Ansible Module'

Master Node is connected to Host-Node through SSH and execute the small modules on host's machine and install the software.

After Execution, The Modules will be removed on host machines.

The Master node control the entire execution of the playbook. 

The Inventory file provides the list of hosts where ansible modules need to be run.

Ansible requires following three Components to automate the Network Infrastructure, 

	1) Control Node / Master Node 
	2) Managed Nodes / Host Node
	3) Ansible Playbook

1. Control/Master Node :

- The Machine which contains ansible server is called as Control node. It will controll other Host nodes.

2. Managed/Host Node :

- The Nodes which are managed or controlled by ansible is called Host Node /Managed Node

3. Ansible PlayBook :

- Ansible will use Playbook to automate configurations in Host Node.

- Ansible playbooks are expressed in YML format 

- Playbooks are a collection of tasks that will be runs on one or more hosts nodes.

## Inventory File

Ansible Inventory hosts file is used to store the IP_Address of Target servers. we can list and group your host server.

The Default location is "/etc/ansible/hosts"

Note: In Inventory file, we can mention Private_IP Address or Hostname also.


### Few Important Point about Inventory File 

1. Comments begins with '#' character.

2. Blanklines are ignore

3. Group of hosts are delimited by '[header]' elements.

4. You can enter hostname or Ip addresses.

5. A hostname/ip can be a member of multiple groups.

6. Ungrouped hosts are specifying before any group header like below

### Sample Inventory File :

```
#Blank lines are ignore

#Ungrouped hosts are specifying before any group header like below 

192.168.122.1    #Private_IP
192.168.122.2
192.168.122.3

[webservers]

192.168.122.1
#192.168.122.2
192.168.122.3

[dbserver]

192.168.122.1
192.168.122.2 	
silver-db1  	#hostname
silver-db2  	
```

## Chef Work-Flow / Chef Architecture :

![image](https://user-images.githubusercontent.com/120022254/231712744-c05a3070-3016-49ae-bb1a-5da6560973c3.png)

* Work stations - we write cookbooks[ruby] after shared to chef master.

* Chef server - we install chef and connected to chef client servers.

* Chef client - we have different Nodes

* If we Compare Ansible and chef - Ansible is easy to setup and run in any environment [any device]

## Difference B/w Ansible, Cheff and puppet :

![image](https://user-images.githubusercontent.com/120022254/231713522-47f95554-0b01-4cc0-b1b5-4059c93fe099.png)


## ``STEPS TO SETUP ANSIBLE`` 

* Create 2 RedHat Systems In AWS with .pem  (Free-ties Eligible)
    - Master Node 
    - Slave Node
* Connect to the Nodes and create Ansible_User and Passwd

* Create User 
```
sudo useradd ansible
```
* Create Password 
```
sudo passwd ansible 
```
* Configure Ansible_User in Sudo file (All Permissions like Root)
```
sudo visudo 
```
Add the following configuration in 100th line
    >> ```ansible ALL=(ALL) NOPASSWD: ALL```   

* Configure SSH : Ansible uses SSH to connect to remote machines.
```
sudo vi /etc/ssh/sshd_config
```
Change the following configurations in below 60 line

    >> Comment  ``` #PasswordAuthentication no ```
    
    >> Un-Comment  ``` PasswordAuthentication yes ```

* Now, Restart SSH Service
```
sudo systemctl restart sshd 
```
Or We can use alternate command: ``sudo service sshd restart``

Note: Do all above steps in Master and Slave Nodes

## `` Install Ansible In Master/Control Node ``

1. Install Ansible in Master Node
```
sudo amazon-linux-extras install ansible2 -y
```
* To Varify Ansible Version, Run the following command
``` 
ansible --version 
```
2. Install Ansible Dependencies
```
sudo yum install python python-pip python-level openssl -y
```
* To varify Python 
```
python --version
```
That's it! You have successfully set up an Ansible server on Linux with the necessary dependencies. You can now use Ansible to automate the configuration and management of your IT infrastructure.

* Ansible Default location : ``cd /etc/ansible``
    * Ansible folder staructure 
        * ansible.cfg 
        * hosts
        * roles

## ``Establish The Connection B/w Control & Host Node Through SSH ``

1. Switch to Ansible user in Control Node
```
    sudo su - ansible
```
2. Generate SSH key in Ansible Server
```
    ssh-keygen
```
Click on 'Enter' 3 Times and You will get the SSH key. The Generated SSH key stored in '/home/ansible/.ssh/id_rsa'

3. Add the Control node's SSH key (Copy Keygen to Host Server as Ansible user):
```
    ssh-copy-id <user>@<slave_node_ip_address>
```
Replace "user" with the username you use to log in to the slave node and "slave_node_private_ip" with the IP address of the slave node.
i.e, ``` ssh-copy-id ansible@172.31.41.168 ``` and Slave Node User Passwd: ``123``

Note : We can configure no. of slave nodes using above command.

* We can login into slave node from control node using following command 
```
    ssh ansible@172.31.41.168
```
If you want to logout from slave node, run `` exit `` command 

4. Configure Host Server Details in Host Inventory file  
```
    vi /etc/ansible/hosts
```
In the inventory file, The Host details as Grouped and Ungrouped Hosts

    #Ungrouped Host can configured as 
    172.31.41.168  or Host_Name

    #Grouped Host Can be configured as 
    [webserver]
    172.31.41.168  or Host_Name
    172.31.41.169  or Host_Name

These Hosts are not execute directly. if u want to execute these groups by giving permissions.

5. Give Permission to Inventory file and sudo_user [FROM ROOT USER]
```
    vi /etc/ansible/ansible.cfg
```
Giving permissions to Inventory and Root by removing '#'

    inventory      = vi /etc/ansible/hosts   
    sudo_user      = root

6. Using Ping module, we can varify the connection b/w Control and Host Node
```
    ansible all -m ping
```
* To get the list of all hosts connected to control node
```
    ansible all --list-hosts
``` 
* To get the particular group hosts connected to control node
```
    ansible <groupName> --list-hosts

    Example : ansible webserver --list-hosts
```
* To get the particular group index value connected to control node 
```
    ansible <groupName>[IndexValue] --list-hosts

    Example : ansible all[0] --list-hosts 
```

## YAML (Yet Another Markup Language) :

* YAML is human friendly data serialization language for all programming languages.

* YAML files are both human readable and machine readable

* YML files will have .yml as an extension

* In YML, the data will represent in Key-Value Pair.
	
* Indent space very important in YML language. 

* Official website : https://yaml.org/


### Key-Value Pair Format :
```
---
name: silver 
email: silver@gmail.com
gender: male

...
```

### Arrays / list Format :
```
---
name: 				     #element 
    firstname: naveen    #sub-element 
    lastname: silver
email: silver@gmail.com
gender: male
skills:
    - python       #list of items
    - linux
    - devops 
    - aws 
```
Here, - dash indicate the element of any array.

we can use VS code IDE to write and validate YML files.


## PlayBooks :

* Playbooks in ansible written in YAML language.

* Playbooks is a single yaml files, containing one or more 'plays' in a list.

* Plays are ordered sets of tasks to execute against host servers from inventory file.

* Playbook code consists of vars, tasks, handlers, file, template and roles.

* Each playbook is composed of one or more modules in a list.

* Module is a collection of configuration files

* Play defines a set of tasks to be run on hosts.

* Task is an action to be performed on host.
 
* Example: 
	- Execute a command
	- Run a shell script
	- Install Package 
	- Shutdown/Restart hosts.

* Playbooks starts with '---' and ends with '...'

### Playbook contains 3 sections :

    1) Host Section : Defines the target machines on which the playbook should run. This is based on the Ansible Inventory file.

    2) Variable Section: This is optional and can declare all the variables needed in the playbook.

    3) Task Section: This section list out all tasks that should be executed on the target machine. it specifies the use of modules. Every tasks has a name which is small description of what the task will do and will be listed while the playbook is run.

### Types of state In Playbook :

    1. present - add/install
    2. absent - remove/uninstall
    3. latest - latest  
    4. restarted - restart

## Playbook Examples :

Q) Write a playbook to ping all host nodes ?
```
---                  #indicates yaml file 
- hosts: all           # - indicates list
  user: ansible         # user name 
  become: true         #it allows you to execute playboks as root user
  connection: ssh 

  tasks:		# here provide the list of tasks 
    - name: Ping All Host Nodes
      ping:
      remote_user: ansible
...
```

#Hosts - The Tasks will be executed in specified group of server.

#Name - Which is the task name that will appear in your terminal when you run the playbook.

#Remote_user - This parameter was formerly called just user.


## Ansible Playbook Commands :

* To Varify the Syntax of Playbook
```
ansible-playbook <playbookName> --syntax-check
```
* Syntax to Execute a Playbook
```
ansible-playbook <PlaybookName>
```
* Syntax to Execute a Playbook in verbose mode
```
ansible-playbook <PlaybookName> -v	#Details of execution 
ansible-playbook <PlaybookName> -vv	#To get more details of execution
ansible-playbook <PlaybookName> -vvv	#To get complete execution details  # -v verbosity means debug
```
* Playbook Dry-Run Command 
```
ansible-playbook <PlaybookName> --check
```
* List of Hosts in Playbook
```
ansible-playbook <PlaybookName> --list-hosts
```
* It will Execute one-step-at-a-time, confirm each task before running (N)o/(y)es/(c)ontinue
```
ansible-playbook <playbookName> --step
```
* It will Help on Ansible-playbook Command
```
ansible-playbook help
```


### Why Ansible Roles :


Ansible roles is efficient way to writing ansible playbook that will only improve your efficiency to write complex playbooks.

Best ex, Lets say i want to configure a kubernetes using ansible. so, it will have close to some 50 to 60 tasks and we have lot of variable and lots of parameters you have certificate you have secrets that you have to configure while creating the kubernetes cluster. So, for that very own reason if you try to do it with Roles like we can segregate each and everything and you can properly structure your ansible playbooks. So, thats why the concept of roles is introduced.

$ mkdir folder

$ cd folder

$ ansible-galaxy role init kubernetes

-  Role kubernetes was created successfully.

if you give $ ls 

there is a folder called `kubernetes`

$ ls kubernetes 

we can see bunch of files that created. This is the concept roles.
Using this files and folders, we can structure ansible playbooks.

Files and folders inside roles :

```
meta : used to write some meta data info like liscence, author, role description.. etc

defaults :  default files we can store 
 
vars : store the variable 

tests : used add some unit tests

handlers : 

tasks : 

readme.md :

files : 

templates :
```

Whenever you want to write some complicated playbooks. start using ansible-galaxy command to create roles. 

If you create roles, we can write structured and efficient ansible playbooks.





