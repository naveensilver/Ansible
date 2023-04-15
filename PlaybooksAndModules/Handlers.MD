## Handlers : 

In Ansible, a handler is a task that is triggered by another task. Handlers are typically used to perform actions that are related to changes made during the execution of a playbook, such as restarting a service or update a configuration file.

Handlers are only executed when triggered by a task that notifies them, which helps to minimize unnecessary actions during playbook execution.

Notify will directly communicate with the handlers.

Simply, Handler will execute task based on other task execution.

`1.Restarting a service after modifying its configuration file:`

```
---
- hosts: all
  user: ansible
  become: true 
  connection: ssh

  tasks:
    - name: Copy Apache configuration file
      copy:
        src: </path/to/apache.conf>
        dest: </etc/httpd/conf/httpd.conf>
      notify: Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted

```
`2. Installing a package and restart the package using hanlders`

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: install httpd apache webserver
      command: yum install httpd -y
      notify: restart httpd

  handlers:
    - name: restart httpd             #same as notify
      service: name=httpd state=restarted

```

Task : `Configure multiple Handlers in Single playbook ?`

```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: Install Apache HTTP server
      yum:
        name: httpd
        state: present
      notify: Restart Apache

    - name: Copy Apache configuration file
      copy:
        src: </path/to/apache.conf>
        dest: </etc/httpd/conf/httpd.conf>
      notify: Reload Apache

  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted

    - name: Reload Apache
      service:
        name: httpd
        state: reloaded

```
