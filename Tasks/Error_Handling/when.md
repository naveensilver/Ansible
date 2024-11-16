### **STAR Format for Using `when` Conditional Statements in Ansible**

---

### **S**ituation:

You are managing a fleet of servers that serve different roles in a multi-tier application. These servers include:

- **Web Servers**: Running web applications (e.g., Apache, NGINX).
- **Database Servers**: Running database services (e.g., MySQL, PostgreSQL).
- **Application Servers**: Running business logic and APIs.

You need to ensure that tasks like installing **Apache** should only be performed on the **web servers** in your infrastructure. Running Apache installation on database or application servers could cause unnecessary bloat, service conflicts, or performance issues.

Thus, you need a way to execute specific tasks only on the appropriate servers (e.g., only web servers should install Apache).

---

### **T**ask:

The goal is to conditionally install the **Apache** package only on the **web servers** in the inventory, ensuring that web server tasks are isolated from other types of servers (e.g., database or application servers).

The challenge is to ensure that this task does not run on servers that are not part of the **`web_servers`** group, thereby preventing the installation of Apache on non-web servers.

---

### **A**ction:

1. **Identify Target Servers**:
   - The first step was to identify which servers should run the task. In this case, **web servers** are defined in the Ansible **inventory file** or **group_vars** under a specific group (`web_servers`).

2. **Use the `when` Conditional**:
   - I used the **`when`** conditional to ensure that the **Apache installation** task only runs on hosts that are part of the **`web_servers`** group. This was achieved by checking if the current host (`inventory_hostname`) belongs to the `web_servers` group.

3. **Create a Task to Install Apache**:
   - I used the `apt` module to install the **Apache** package, but added the `when` conditional to ensure the task is only executed on the servers in the `web_servers` group.

Here is the implementation:

```yaml
---
- name: Install Apache on web servers only
  hosts: all  # This will apply to all hosts in the inventory
  become: true  # To run as root or with sudo
  tasks:
    - name: Install Apache2
      apt:
        name: apache2
        state: present
      when: inventory_hostname in groups['web_servers']
```

In this playbook:
- The **`apt`** module installs the **Apache2** package on the target machine.
- The **`when`** conditional ensures that the installation happens **only on hosts in the `web_servers` group**. If the server is not part of the `web_servers` group, the task is skipped.

---

### **R**esult:

1. **Conditionally Executed Task**:
   - On **web servers**, the Apache installation task executed successfully.
   - On **non-web servers** (e.g., database or application servers), the task was **skipped**.

2. **Efficient Playbook Execution**:
   - This approach ensured that **unnecessary tasks** were not executed on servers where they were not needed, reducing the risk of errors or conflicts.
   - It also **improved playbook efficiency** by skipping tasks that didn’t apply to certain server types.

3. **Consistent Server Configuration**:
   - The playbook made sure that only the relevant servers received the Apache installation, keeping configurations consistent across the environment and avoiding potential misconfigurations on the wrong server types.

---

### **Example Output:**

When running the playbook on multiple servers, here’s what you would see:

```bash
PLAY [Install Apache on web servers only] ************************************

TASK [Install Apache2] ********************************************************
ok: [web-server-1]
ok: [web-server-2]
skipping: [db-server-1] => (item=None) 
skipping: [app-server-1] => (item=None)

PLAY RECAP *******************************************************************
web-server-1               : ok=1    changed=1    unreachable=0    failed=0
web-server-2               : ok=1    changed=1    unreachable=0    failed=0
db-server-1                : ok=0    changed=0    unreachable=0    failed=0    skipped=1
app-server-1               : ok=0    changed=0    unreachable=0    failed=0    skipped=1
```

As you can see:
- **Apache** was installed on **`web-server-1`** and **`web-server-2`**.
- The task was **skipped** for **`db-server-1`** and **`app-server-1`**, as expected.

---

### **Why It’s Used:**

1. **Targeted Task Execution**:
   - Using the `when` condition allows you to execute tasks **only on relevant hosts**, ensuring that you don’t perform unnecessary or potentially harmful actions on servers that don’t require them.

2. **Efficient and Scalable Automation**:
   - This approach works in heterogeneous environments (where servers have different roles), allowing you to write **generic playbooks** that work across multiple servers while tailoring specific tasks for different groups (e.g., web servers, database servers, etc.).

3. **Improves Maintenance**:
   - With the `when` condition, you can easily add or remove tasks for specific groups without modifying the entire playbook. For instance, if you decide to install Apache on new servers that are part of the `web_servers` group, the playbook will automatically apply without any further changes.

4. **Reduces Errors and Conflicts**:
   - The conditional execution prevents accidental installations or configurations on the wrong servers, which could lead to service conflicts, unnecessary resource consumption, or performance degradation.

---

### **Conclusion:**

By using the `when` conditional, you can easily tailor your playbooks to work with **specific server roles** or environments, ensuring that tasks are executed **only when necessary**. In this case, it allowed for the **conditional installation of Apache** only on the **web servers**, preventing issues on other types of servers (e.g., database or application servers). 

This is a **best practice** when working in environments with mixed server roles, as it allows for efficient, flexible, and error-free automation.
