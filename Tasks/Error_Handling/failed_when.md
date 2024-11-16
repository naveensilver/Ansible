### Scenario: Using `failed_when` Condition in Ansible

In this real-time scenario, we'll walk through a situation where we need to check if a critical file exists on a server (e.g., a configuration file for an application). If the file does not exist, we want to **explicitly fail the task** even if the command itself doesn't return an error (e.g., `stat` returns a 0 exit code but the file is missing).

### **STAR Format Breakdown for `failed_when` Implementation**

---

### **S - Situation:**

You are automating the configuration of a server using Ansible. As part of the automation process, the system must have a specific configuration file (e.g., `/etc/myapp/config.conf`) for the application to work properly.

- The **application configuration file** must exist, and if it doesn't, the playbook should **fail explicitly** and alert the team that the file is missing.
- The task must fail explicitly even if the command doesn't fail (i.e., the `stat` module might return a 0 exit code if it simply checks the file but doesn't see any error).
  
### **T - Task:**

Your task is to create an Ansible playbook that:
1. Checks if the file `/etc/myapp/config.conf` exists.
2. Fails explicitly if the file is missing, even if the `stat` module doesn't return a non-zero exit code (which would normally indicate a failure).

### **A - Action:**

Here’s how to implement this solution from scratch, following the **Ansible workflow**.

#### Step 1: Set up your environment

1. Install Ansible on your local machine or control node:
   ```bash
   sudo apt install ansible  # Ubuntu/Debian
   # or
   sudo yum install ansible  # CentOS/RHEL
   ```

2. Prepare an **inventory file** where you'll define the target host(s). For this example, we’ll use one server (`webserver1`).

#### Step 2: Create an Inventory File

Create an inventory file, `inventory/hosts.ini`, to define the target server.

```ini
[web_servers]
webserver1 ansible_host=192.168.1.10 ansible_user=ubuntu
```

- `webserver1` is your target machine (replace `192.168.1.10` with your actual server's IP).
- Ensure that you have SSH access to this machine and the required privileges.

#### Step 3: Write the Playbook

Create a playbook file `check_file.yml` that checks the existence of `/etc/myapp/config.conf` and fails explicitly if the file is missing.

```yaml
---
- name: Check for the existence of critical configuration file
  hosts: web_servers
  become: yes  # Run as sudo to access system directories
  tasks:
    - name: Check if the configuration file exists
      stat:
        path: /etc/myapp/config.conf
      register: file_check
      failed_when: file_check.stat.exists == false  # Explicitly fail if the file doesn't exist
      msg: "Critical configuration file /etc/myapp/config.conf is missing!"
```

### Explanation:

1. **`stat` module**: This module is used to gather information about a file, such as whether it exists, its permissions, etc.
2. **`register: file_check`**: The result of the `stat` module is saved in the variable `file_check`.
3. **`failed_when: file_check.stat.exists == false`**: This tells Ansible to **explicitly fail** the task if `file_check.stat.exists` is `false` (meaning the file does not exist).
4. **`msg`**: A custom error message to indicate why the task failed if the file is missing.

#### Step 4: Run the Playbook

Now, execute the playbook to check if the file exists on the target server:

```bash
ansible-playbook -i inventory/hosts.ini check_file.yml
```

This will:
1. Attempt to check if `/etc/myapp/config.conf` exists on `webserver1`.
2. If the file doesn't exist, the task will **fail explicitly**, and you will see the custom error message in the output.

---

### **R - Result:**

#### Scenario 1: **File Exists**

If the file `/etc/myapp/config.conf` exists on the target machine, the playbook will run successfully without any issues:

```
TASK [Check if the configuration file exists] ***********************************************************************************************
ok: [webserver1] => (item=None) => {
    "changed": false,
    "stat": {
        "exists": true,
        ...
    }
}
```

The task will be marked as `ok`, and no failure will occur.

#### Scenario 2: **File Does Not Exist**

If the file `/etc/myapp/config.conf` does **not exist**, the task will **explicitly fail** as defined by the `failed_when` condition. You’ll see output like this:

```
TASK [Check if the configuration file exists] ***********************************************************************************************
fatal: [webserver1]: FAILED! => {
    "msg": "Critical configuration file /etc/myapp/config.conf is missing!"
}
```

The `msg` field contains the custom error message that explains the failure. The playbook will stop at this point, and the error message will be displayed in the output.

---

### **Real-Time Use Case for `failed_when`**

In **production environments**, the `failed_when` condition is particularly useful when:

1. **Critical files are required**: If the playbook includes tasks that require specific configuration files, such as application settings, certificates, or key files, you want to ensure the file exists before continuing.
   
2. **Conditional checks**: You want to perform a task (like file existence check) and have more **fine-grained control** over what constitutes failure. For example, a missing file could indicate a serious misconfiguration, even if no actual error is thrown by the system or command itself.

3. **Prevent further tasks from running**: If a critical resource (file, directory, service) is missing, failing early can help prevent subsequent tasks from running in a broken state (e.g., trying to configure an app when the configuration file is missing).

---

### **Additional Considerations for `failed_when`**

- **Combining with `when`**: You can use `failed_when` along with `when` for even more complex conditions. For instance, you may want to fail the task if a file doesn't exist **only** if a certain variable is defined or a condition is true.
  
  ```yaml
  - name: Check if the configuration file exists only if the system is critical
    stat:
      path: /etc/myapp/config.conf
    register: file_check
    failed_when: file_check.stat.exists == false and some_condition == true
  ```

- **Custom Error Messages**: It’s always a good practice to provide custom error messages using `msg` with `failed_when` to ensure that anyone reading the output understands why the task failed.

---

### **Conclusion:**

The `failed_when` directive is a powerful tool for controlling the flow of your Ansible playbooks based on specific conditions that go beyond the task's return code. In real-time scenarios, such as ensuring critical configuration files exist, it helps you catch misconfigurations early and fail gracefully with a custom message. This way, your automation tasks can behave in a more predictable and controlled manner.
