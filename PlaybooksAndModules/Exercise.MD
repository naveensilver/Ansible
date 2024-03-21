## Tasks 

### 1. Write a playbook to deploy a static httpd webservers in Slave Node  
```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh

  tasks:
    - name: install httpd
      action: yum name=httpd state=present
      notify: 
        - restart httpd

    - name: create a file 
      file: 
        path: "/var/www/html/index.html"
        state: touch 

    - name: adding content to the file 
      copy:
        src: "/var/www/html/index.html"
        content: |
          <h1>Hey Buddy..! we are learning DevOps </h1>


    - name: copy index.html file
      copy:  #use 'template' instead copy.
        src: index.html
        dest: /var/www/html/index.html

  handlers: 
    name: restart httpd #same as notify 
    service: name=httpd state=restarted
```
          
### 2. Setup Jenkins in slave node Using Playbook 
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh 

  tasks: 
    - name: getting links from jenkins.io 
      get_url:  # Used to getting links from internet
        url: https://pkg.jenkins.io/redhat-stable/jenkins.repo
        dest: /etc/yum.repos.d/jenkins.repo

    - name: getting key from jenkins.io 
      ansible.builtin.rpm_key: # used to import the key 
        state: present 
        key: https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

    - name: update links 
      action: yum name="*" state=latest

    - name: install java11
      command: amazon-linux-extras install java-openjdk11 -y 

    - name: install jenkins 
      command: yum install jenkins -y 

    - name: restart jenkins
      service: name=jenkins state=restarted

```



### 3. Write a playbook to Create multiple files in Slave Node
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh 

  tasks: 
    - name: create multiple files in slave node using loops 
      file: 
        path: "./{{item}}"   # . represents present working directory
        state: touch

      with_items:
        - file.txt 
        - file.jpg 
        - file.png

```
### 4. Write a playbook to Install Jenkins in Slave With Jenkins Password
```
---
- hosts: all 
  user: ansible
  become: true
  connection: ssh 

  tasks: 
    - name: getting links from jenkins.io 
      get_url:  # Used to getting links from internet
        url: https://pkg.jenkins.io/redhat-stable/jenkins.repo
        dest: /etc/yum.repos.d/jenkins.repo

    - name: getting key from jenkins.io 
      ansible.builtin.rpm_key: # used to import the key 
        state: present 
        key: https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

    - name: update links 
      action: yum name="*" state=latest

    - name: install java11
      command: amazon-linux-extras install java-openjdk11 -y 

    - name: install jenkins 
      command: yum install jenkins -y

    - name: import systemd
      ansible.builtin.systemd:
        daemon_reload: yes 

    - name: restart jenkins
      service: name=jenkins state=restarted

    - name: sleep for 30 seconds and continue with play
      wait_for:
        delay: 30
        port: 8080

    - name: init password jenkin
      shell: cat /var/lib/jenkins/secrets/initialAdminPassword
      changed_when: false
      register: result

    - name: print init password jenkins
      debug:
        var: result.stdout
```
### 5. Creating multiple files and sets permission, user and group using VARIABLES With LOOPS
```
---
- hosts: all
  user: ansible
  become: true
  connection: ssh

  vars:
    file_list:
      - path: "./file1.jpg"
        content: "This is the content for file1"
        mode: "421"
        owner: ansible
        group: ansible
      - path: "/root/file2.png"
        content: "This is the content for file2"
        mode: "771"
        owner: root
        group: root
        
  tasks:
    - name: Create multiple files using variables
      file:
        path: "{{ item.path }}"
        state: touch
        mode: "{{ item.mode }}"
        owner: "{{ item.owner }}"
        group: "{{ item.group }}"
        content: "{{ item.content }}"
      loop: "{{ file_list }}"
```
### 6. Write a playbook to install httpd webserver using "set_fact" and "include" modules.

set_fact and include is used to establish connection between two files

$ vi one.yml
```
---
 - set_fact: abc=httpd
 - name: install httpd apache one playbook
   yum: name=httpd state=present
```

$ vi two.yml =>
```
---
- hosts: dev
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - include: one.yml   #set_fact playbook-name

    - name: restart apache httpd webserver in another playbook
      service: name='{{abc}}' state=restarted
```
#Execute: ansible-playbook two.yml



### 7. Write a playbook using Handlers and Notify 
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
    - name: restart httpd                               #same as notify
      service: name=httpd state=restarted
        
# notify: 'notify' will directly communicate with 'handlers' wherever it maybe.
```
### 8. Write a playbook using Loops 
```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
    - name: installing multiple tools using loops
      yum: name='{{item}}' state=present

      with_items:  #Pre-defined module
        - tree
        - git
        - maven
          
#LOOPS: it will execute multiple tasks, one by one-1st tree,2nd git, 3rd maven 
#LOOPS used to perform multiple user,groups and files..etc 

Tasks
	1.create multiple users using loops and delete the multi users ==> check o/p
	2.create multi groups using loops and delete the multi groups ==> check o/p 
```
### 9. Write a playbook using Tags 
```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  tasks:
   - name: installing git in slave server
     action: yum name=git state=present
     tags: abc

   - name: instal maven in slave server
     yum: name=maven state=present

   - name: install tree in slave server
     command: yum install tree -y
     tags: xyz

#Execute: ansible-playbook one.yml --skip-tags "abc, xyz"
```
### 10. Create Roles in Ansible Manually.

Roles

First Create a main.yml playbook with roles user1, user2, user3 

Users will perform individual tasks 

Task ==> user1 ==> install git 
Task ==> user2 ==> install maven 
Task ==> user3 ==> install tree

Note: main.yml file and users folder should be in same directory 

$ vi main.yml  #Main playbook / Execution Playbook: in this playbook we configure the roles 
```
---
- hosts: all
  user: ansible
  become: yes
  connection: ssh

  roles:
    - user1
    - user2
    - user3
```
Now Create Separate user directory with main.yml file :

$ cd user1/tasks/main.yml 
```
- name: installing git - user1
  yum: name=git state=present

```
$ cd user2/tasks/main.yml
```
---
- name: installing maven - user2
  yum: name=maven state=present
```
$ cd user3/tasks/main.yml
```
---
- name: installing maven - user3
  yum: name=tree state=present
```
If you give #ll it show be like this 

main.yml 
user1 
user2
user3

Execute : $ansible-playbook main.yml 


### 11. 
