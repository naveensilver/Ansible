## Template Module :

The template module in Ansible is used to copy a file to a remote host after processing a Jinja2 template on the control machine. This is useful when you need to generate a configuration file for a service or application on the remote host based on a template and variables.

The template module takes two required parameters: `src` and `dest`. The `src` parameter specifies the path to the template file on the control machine, while the `dest` parameter specifies the path where the file will be created on the remote host. When the `template` module runs, it uses the template and any variables you specify to generate a file, which it then copies to the remote host.

Here is an example playbook that uses the `template` module to generate a configuration file for Apache on a remote host:

```
- name: Generate Apache config file
  hosts: webserver
  vars:
    server_name: example.com
  tasks:
    - name: Copy Apache config template
      template:
        src: /path/to/apache.conf.j2
        dest: /etc/httpd/conf.d/apache.conf
      notify:
        - Restart Apache
    - name: Ensure Apache is running
      service:
        name: httpd
        state: started
  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted
```
In this example, the playbook runs on the `webserver` host and generates an Apache configuration file based on the template at `/path/to/apache.conf.j2`. The `server_name` variable is set to `example.com` in the vars section of the playbook, and is used in the template file to set the `ServerName` directive. Once the file is generated, it is copied to `/etc/httpd/conf.d/apache.conf` on the remote host.

The `notify` section of the playbook specifies that the `Restart Apache` handler should be called when the `template` task completes. This handler is defined later in the playbook, and is responsible for restarting the Apache service so that it picks up the new configuration file.

Overall, the `template` module is a powerful tool for generating configuration files and other text-based files on remote hosts, and can be used in combination with variables and other Ansible features to make your infrastructure management more flexible and scalable.
