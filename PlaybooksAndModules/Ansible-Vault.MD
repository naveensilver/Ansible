## Ansible-Vault :

Ansible Vault is a feature that allows you to encrypt sensitive data within your playbooks or inventory files. 

This data can include :
  - GitHub Tokens 
  - User related info  (jenkins usernames and pwds)
  - Passwords 
  - Terraform codes
  - API keys

(OR) any other sensitive information that you don't want to store in plain text.

Why We use Vault : 

*  Used to secure our data.

* when we configure uname and pwd in variable file everybody can see them. it is not recommended. So, we use ansible valut to protect our data.

### Ansible-Valut Commands:

* To Create Encrypt File

```
ansible-vault create filename.yml
```
* To Encrypt Existed File
```
ansible-vault encrypt filename.yml
```
* To Edit Encrypted file
```
ansible-vault edit filename.yml
```
* To Decrypt an Encrypted file 
```
ansible-vault decrypt filename.yml
```
* To view content of Encrypt file
```
ansible-vault view filename.yml
```
* To Rekeying an encrypted file/ Change Encrypt file Passwd
```
ansible-vault rekey filename.yml
```

When you run any of these commands, you will be prompted to enter a password. This password is used to encrypt and decrypt the file. You can also use a password file or key file for automation.

* To Run playbook with the "--ask-vault-pass" option
```
ansible-playbook playbook.yml --ask-vault-pass
```
* You can store vault passwd in a file and you can give that file as input to execute playbook
```
vi vaultpass

ansible-playbook filename.tml --vault-password-file=~/vaultpass
```

* We can see encrypt file in human readable format 
```
ansible-vault view /etc/ansible/group_vars/all.yml
```

### Exercise-1 : 

Using Ansible Vault to encrypt a file

1. Create a new file and add some sensitive information:

```
vi secrets.yml

username: myusername
password: mypassword
```

2. Encrypt the file:

```
ansible-vault encrypt secrets.yml
```
3. Enter a password to encrypt the file

4. Now the file is encrypted, and you can include it in your playbook like any other file:

```
- name: Copy secrets file
  copy:
    src: secrets.yml
    dest: /etc/myapp/
    owner: myuser
    group: mygroup
    mode: 0600
  vars_files:
    - secrets.yml
```
Note : Note that the `vars_files` directive is used to include the encrypted file in the playbook. When the playbook is executed, Ansible will prompt you for the password to decrypt the file

### Exercise-2 :

To include an encrypted file in an Ansible playbook, you can use the `include_vars` module with the `vault_password_file` option. 

1. Create an encrypted file using the ansible-vault command:
```
ansible-vault create secret_vars.yml
```
This will open a new file in the default editor specified in your environment variables. Add your variables to this file, then save and close it

2. Update your playbook to include the encrypted file using the `include_vars` module:

```
- hosts: all
  vars_files:
    - vars/main.yml
    - vars/other_vars.yml
  tasks:
    - name: Include encrypted variables
      include_vars:
        file: secret_vars.yml
      vars:
        ansible_vault_password_file: /path/to/vault_password_file
    - name: Do something with encrypted variable
      debug:
        var: encrypted_variable
```
Note : you need to specify the path to the vault_password_file in the vars section. This file contains the password to decrypt the encrypted file.

3. Run the playbook with the `--ask-vault-pass` option:
```
ansible-playbook playbook.yml --ask-vault-pass
```
This will prompt you for the password to decrypt the encrypted file.

Alternatively, you can specify the path to the `vault_password_file` using the `--vault-password-file` option

```
ansible-playbook playbook.yml --vault-password-file=/path/to/vault_password_file
```




## `* Configuring Encrypt file in Playbooks`

Here, some examples of playbooks that use encrypted files:

* Example 1: Using Encrypted Variables in Playbook

```
- hosts: all
  vars_files:
    - vars/secrets.yml
  tasks:
    - name: Copy file with secret
      copy:
        src: secret_file
        dest: /path/to/secret_file
```
In this example, we include an encrypted file called `secrets.yml` in the `vars_files` section of the playbook. The encrypted variables can then be used in tasks, such as copying a secret file to a remote host.

* Example 2: Using Encrypted Files with Ansible Vault

```
- hosts: all
  tasks:
    - name: Copy encrypted file to remote host
      ansible.builtin.copy:
        src: secret_file.vault
        dest: /path/to/secret_file
        decrypt: yes
```
In this example, we use Ansible Vault to encrypt the `secret_file`. The encrypted file is then copied to a remote host using the `copy` module. The `decrypt` option is set to `yes` to decrypt the file during the copy operation.

* Example 3: Using Encrypted Files with Include

```
- hosts: all
  tasks:
    - name: Include encrypted file
      include_vars: encrypted_vars.yml
      vars:
        ansible_vault_password_file: /path/to/vault_password_file
```
In this example, we use the `include_vars` module to include an encrypted file called `encrypted_vars.yml`. The `ansible_vault_password_file` variable is set to specify the location of the password file used to decrypt the file.


