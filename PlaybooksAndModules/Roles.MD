# ROLES :

In Ansible, roles are a way of organizing and grouping related tasks, variables, files, and templates into a reusable unit. Roles allow you to structure your playbooks in a modular way, making them more organized, easier to maintain, and more reusable.

In simply, Roles are used to make our playbooks easily readble, maintainable and manageble and also reusable.

Ansible roles used to allow better management of playbooks and tasks. 

* Ansible roles are consist of many playbooks, which is similar to modules in puppet and cookbooks in chef.

* Roles are a way to group tasks together into one container to do automation in very effective manner with clean directory structures.

* Roles are set of tasks and additioal files for a certain role which allow you to break up the configurations.

* Roles can easily share them with others.

* it can be easily modify and will reduce syntax errors.

## Sample PLaybook with Roles :
```
---
- hosts: all 
   become: true 
   roles: 
	- <roleName1>
	- <roleName2>

```

### HOW TO CREATE ANSIBLE ROLES:

To create a role in Ansible, you can use the "ansible-galaxy" command-line tool. This tool can create a new role structure for you, including the necessary directories and files.

## ANSIBLE-GALAXY :

Ansible-galaxy is a command-line tool that is used to manage Ansible roles. It allows you to create new roles, install roles from Ansible Galaxy, and manage dependencies for your roles.

Ansible-Galaxy Basic Commands:

* init : To create a new role structure 
```
ansible-galaxy init <myrole>
```
Role is Stored in `/root/.ansible/roles/<myrole>`

* Install : To install a role from Ansible-Galaxy
```
ansible-galaxy install geerlingguy.mysql
```

* Remove : To remove a role 
```
ansible-galaxy remove geerlingguy.mysql
```

* list : To specify roles list 
```
ansible-galaxy list -p /path/to/roles
```
You can use the '-p' option to specify the directory where your roles are installed

* Import : To Import an existing role to your own role
```
ansible-galaxy import geerlingguy.mysql </path/to/myrole>
```

* Info : To display information about the role
```
ansible-galaxy info geerlingguy.mysql
```

Note: Roles created in "/home/ansible"

### Role Directory Structure:

Whenever we create a role using "ansible-galaxy init `<role-Name>`"
This will create a new directory with the same name as your role, and inside it will create the following directories and files.

To Dislay Folder Structure :

```
sudo tree <role-Name>
```

```
<role-name>/
├── defaults/
│   └── main.yml
├── files/
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── README.md
├── tasks/
│   └── main.yml
├── templates/
├── tests/
│   ├── inventory
│   └── test.yml
└── vars/
    └── main.yml

```

Roles in Ansible are a way to organize related tasks, files, templates, and variables into a single, reusable unit of work. A role is a collection of files and directories that are structured in a particular way, which allows Ansible to identify and use the role.

Roles have a specific directory structure that contains the following directories:

* defaults: This directory contains "default variables" for the role

* files: This directory contains files that need to be copied to the target hosts.

* handlers: This directory contains handlers, which are tasks that are triggered by other tasks when certain conditions are met.

* meta: This directory contains metadata about the role,such as its name, description, author, and dependencies, Versions..

* README.md: This file contains information about the role, such as its purpose and how to use it.

* tasks: This directory contains the main tasks for the role.

* templates: This directory contains Jinja2 templates that are used to generate configuration files or other files that need to be customised for each host.

* tests: This directory contains test files that can be used to test the role.

* vars: This directory contains other variables that are used in the role. This 'vars' has high priority than 'default'


To use a role in a playbook, you simply need to include the role name in the playbook's YAML file, along with any necessary variables. Ansible will then automatically execute the tasks defined in the role. like this,

```
---
- hosts: web
  become: yes
  roles:
    - role: <role-name>

```

## ``Lets Work with Ansible Roles in Linux:``

Lets Take a example to create a role for Apache webserver :

* Below is sample playbook code to deploy Apche webserver. lets convert this playbook code into ansible roles.

```
---
- hosts: all
  become: true
  tasks:
    - name: install httpd package
      yum:
        name: httpd
        state: latest

    - name: copy index.html file
      copy:                                     #use 'template' instead copy.
        src: index.html
        dest: /var/www/html/index.html
      notify: restart apache

    - name: start apache
      service:
        name: httpd
        state: started

  handlers:
    - name: restart apache        #same as notify
      service:
        name: httpd
        state: restarted

```

* Create A Separate Directory for Roles
```
sudo mkdir roles
```

* To create a new role, run following command,

```
ansible-galaxy init roles/apache
```
Note : This apache Role created in "/home/ansible"

* Check the Role folder structure using 'tree' package
```
tree roles/apache
```
Note : tree package should be installed and To install tree package using below command
 ```
 sudo yum install tree -y 
 ```

First, Go to ansible roles directory and start editing the yml files.
```
cd roles/apache
```
1. Tasks : Edit 'main.yml' available in the tasks folder to define the tasks to be executed.

```
vi tasks/main.yml 
```

```
---
#tasks file for roles/apache

- name: install httpd package
  yum:
    name: httpd
    state: latest

- name: copy index.html file
  copy:                          #use 'template' instead copy.
    src: index.html
    dest: /var/www/html/index.html
  notify: restart apache 

- name: restart apache
  service:
    name: httpd
    state: restarted

```
Note: Here we are configuring only tasks section.

2. Files: Copy required files into files directory are create index.html file with content.

```
  cd files/
  vi index.html 
```
* Add some content/code into a file, like this 
```
<h1> Hello Babes..Welcome to My DevOps Worlds </h1>
```
Note: Here, we add the project files.

3. Handlers : Edit handlers main.yml to restart the server when there is a chance. because we have already defined it in the tasks with notify option. use the same name "restart apache" with in the main.yml file as below.

```
  cd handlers/
  vi main.yml 
```
* Add Handlers Task, 
```
---
#handlers file for /etc/ansible/roles/apache 
- name: restart apache        #same as notify
  service:
    name: httpd
    state: restarted
```
Note : Here, We are configuring only Handlers. Hanlders will perform after 'notify' task execution.

4. Meta : Edit meta main.yml to add the information about the roles like author, descriptions, license, platforms supported

```
  cd meta/ 
  vi main.yml 
```
```
---
galaxy_info:
   author: silver
   description: apache webserver role
   company: tcs
```
Note: Here, we are adding Metadata, ignore if you want.

## Now check the apache role structure :

```
  sudo tree roles/apache
```
we have got all the required files for apache roles. 

* lets apply this role into playbook "<playbook_role>.yml" as below to deploy it on the client nodes.

```
  cd /home/ansible/
  vi playbook_role.yml
```
```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh
  roles:
    - apache

```
Note: Here, we are configuring only Host section and Roles.

* Execute the playbook which contains roles
```
  ansible-playbook playbook_role.yml
```
Note : Access static webserver using VM_IP 

So, Based on the requirements, we can create roles and we can execute the roles from single playbook.


