## Ansible Set up 

### Ansible server 
```
sudo -i

sudo amazon-linux-extras install ansible2 -y

yum install git python python-pip python-level openssl -y

vi /etc/ansible/hosts   # paste slave ip 

vi /etc/ansible/ansible.cfg   # un-tag (inventory and sudo_user)

useradd ansible 

passwd ansible

visudo  # :100 line    >> ansible ALL=(ALL)  NOPASSWD: ALL

vim /etc/ssh/sshd    # :63 line  >> PasswordAuthentication Yes
```

### Node :

```
sudo -i 
useradd ansible 
passwd ansible 
visudo 
vim /etc/ssh/sshd 

```

### All Servers :
```
sudo systemctl restart sshd
sudo systemctl status sshd
```

### Login as Ansible in all servers by Using 
```
su - ansible
```




