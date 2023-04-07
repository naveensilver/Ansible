# Ansible
it has all my ansible notes 

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
To Varify Ansible Version, Run the following command
``` 
ansible --version 
```

2. Install Ansible Dependencies
```
sudo yum install python python-pip python-level openssl -y
```
To varify Python 
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

5. Give Permission to Inventory file and sudo_user
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
* To get the list of hosts connected to control node
```
    ansible all --list-hosts
``` 
* To get the particular group hosts connected to control node
```
    ansible <groupName> --list-hosts
```
