## Setfact Module :

In Ansible, set_fact module is used to set the value of a variable. It is used to set a variable dynamically based on a condition, the output of a command or the result of a registered variable.

* The syntax for using `set_fact` module is as follows:

```
- set_fact:
    variable_name: value
```
Here, `variable_name` is the name of the variable that you want to set and `value` is the value that you want to assign to that variable.

* For example, if you want to set a variable `my_var` with a value of `Hello World`, you can use the following task in your playbook:

```
- name: Set the value of a variable
  set_fact:
    my_var: "Hello World"
```
After running this task, the variable `my_var` will be set with a value of `Hello World`.

* `set_fact` can also be used with the `register` keyword to set a variable to the `output` of a command or the result of a registered variable. Here is an example:

```
- name: Run a command and set a variable
  shell: echo "Hello World"
  register: output

- name: Set a variable to the output of a command
  set_fact:
    my_var: "{{ output.stdout }}"
```

In this example, the `shell` module is used to run the `echo` command, and the output of the command is registered in the `output` variable using the `register` keyword. Then, the `set_fact` module is used to set the value of `my_var` to the output of the command stored in the `output.stdout` variable.

* `set_fact` module can also be used in conjunction with `include` module to dynamically include a task or playbook based on the value of a variable. Here is an example:

```
- name: Include a task based on the value of a variable
  include_tasks: "{{ task_to_include }}"
  vars:
    task_to_include: "task1.yml"
```
In this example, the `include_tasks` module is used to include a task defined in `task1.yml`. The name of the task to be included is determined dynamically based on the value of the `task_to_include` variable, which is set using the `set_fact` module or any other method.




