### **STAR Format for Implementing Error Handling with `block`, `rescue`, and `always` in Ansible**

---

### **S**ituation:

You are tasked with deploying **NGINX** (a critical web server application) across multiple servers in a production environment. The deployment involves several key steps, including:
- Installing the NGINX package.
- Starting the NGINX service.
- Configuring NGINX for specific use cases (e.g., setting up virtual hosts).

During this process, there is a chance that certain tasks may fail (e.g., package installation fails, or the NGINX service doesn’t start properly due to a conflict or misconfiguration). To ensure the deployment is robust and resilient to failure, you need to handle errors gracefully and perform necessary recovery steps.

In addition, you need to guarantee that certain tasks, such as sending notifications or cleanup actions, always run regardless of the success or failure of the main tasks.

---

### **T**ask:

You need to:
1. Implement error handling in Ansible using **`block`, `rescue`, and `always`**.
2. Ensure that if any task in the **`block`** fails (e.g., package installation or service startup), the **`rescue`** block is triggered to handle failure (e.g., sending notifications, rolling back the changes).
3. Ensure that cleanup or notification tasks in the **`always`** block run regardless of success or failure of the tasks in the **`block`**.

---

### **A**ction:

1. **Created a Playbook to Group Tasks in a `block`**:
   - I grouped the tasks for installing and starting the NGINX service under the `block` directive. This makes it easier to define error recovery if something goes wrong during either the installation or the service start.

2. **Used `rescue` for Failure Recovery**:
   - In the `rescue` block, I added steps to handle potential errors:
     - A **debug message** to indicate that the NGINX installation or service start failed.
     - A task to **uninstall NGINX** if the installation or service start failed, as part of the rollback.

3. **Ensured Cleanup with `always`**:
   - Regardless of whether the tasks in the `block` succeeded or failed, the `always` block contains a task to **send notifications** or **log** the result, ensuring that the team is informed of the status.
   - I used the **debug** module to simulate sending a notification (this could be an email, Slack message, or logging action in a real scenario).

Here’s the complete playbook I implemented:

```yaml
---
- name: Install and Configure NGINX Web Server
  hosts: web_servers
  become: true
  tasks:
    - block:
        - name: Install NGINX package
          apt:
            name: nginx
            state: present

        - name: Start NGINX service
          service:
            name: nginx
            state: started

      rescue:
        - name: Handle failure (e.g., notify or roll back)
          debug:
            msg: "NGINX installation or service start failed. Performing rollback."
        
        - name: Rollback - Uninstall NGINX package
          apt:
            name: nginx
            state: absent

      always:
        - name: Always run cleanup or notification tasks
          debug:
            msg: "This task always runs regardless of success or failure. Sending notification."
```

---

### **R**esult:

- **NGINX Installation Success**: If the installation and service start tasks in the `block` succeeded, the playbook would complete without triggering the `rescue` block. The `always` block would still run to send the success notification.
  
- **NGINX Installation or Service Start Failure**: If any task in the `block` failed (e.g., NGINX package installation or service start), the tasks in the `rescue` block would execute:
  - A failure message would be displayed.
  - The NGINX package would be uninstalled as part of the rollback process.
  
- **Always Run Cleanup or Notification**: Regardless of the outcome, the `always` block ensured that cleanup or notification tasks ran, providing visibility and accountability. This would send an alert (or log) indicating that the playbook ran to completion, whether it was successful or not.

#### **Outcome**:
- The implementation of **error handling** ensured that transient issues, like service startup delays or network issues, didn’t cause the entire deployment to fail.
- The **rescue** block provided an automatic recovery and rollback mechanism, ensuring that the server was restored to a known state if something went wrong.
- The **always** block guaranteed that essential post-processing actions (like notifications) occurred, allowing the operations team to stay informed.

### **Summary**:
By using **`block`, `rescue`, and `always`** in Ansible, I was able to create a **resilient deployment process** that could handle failures gracefully, automatically recover from issues, and always notify the team of the status, ensuring the deployment was safe and reliable even in the face of transient or unforeseen issues.
