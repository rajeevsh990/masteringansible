# Custom Facts:

Custom facts are used to set custom defined facts on the target server. e.g. location = Toronto 
 
Go to: 03 - Ansible Playbooks, Advanced Topics/01 - Ansible Playbook Modules/01/set_fact_playbook.yml

# Pause tasks: 
Go to: 03 - Ansible Playbooks, Advanced Topics/01 - Ansible Playbook Modules/05/pause_playbook.yml

# Looping based on when:
Go to: 03 - Ansible Playbooks, Advanced Topics/04 - Looping/01/motd_playbook.yml

# Parallelism: 
**Reference:** https://docs.ansible.com/ansible/latest/user_guide/playbooks_async.html

- By default ansible behaviour is to execute tasks is linear. 
- The execution of next task won't happen if the currect executing task is not finished for all it's targets. 
- If any task is dedicated for a specific host it will hold up the execution of rest of the tasks. 


Execution of tasks in parallel. Asynchronous modules can be used in case there are background jobs running and we need to wait for them to be finished. 

By default the jobs execute in liner fashion. 

Go to: 03 - Ansible Playbooks, Advanced Topics/05 - Asynchronous and Parallel Execution/ 

> time ansible-playbook slow_playbook.yaml 

Execute above command in 01, 02, 03, 04 and 05

Use async_status module to execute jobs in parallel. 
Reference: https://docs.ansible.com/ansible/latest/modules/async_status_module.html

Go inside 07 

> ansible-playbook slow_playbook.yaml

	
# Delegation: 

If you want to perform a task on one host with reference to other hosts, use the ‘delegate_to’ keyword on a task. This is ideal for placing nodes in a load balanced pool, or removing them. 

Explanation: 03 - Ansible Playbooks, Advanced Topics/06 - Task Delegation/01/dynamic_dns_playbook.yml

Example:
```
---
- hosts: ServerB
  tasks:
    - name: Transfer file from ServerA to ServerB
      synchronize:
        src: /path/on/server_a
        dest: /path/on/server_b
      delegate_to: ServerA
```
```
---
- hosts: cent
  tasks:
    - name: Install zenity
      action: yum name=zenity state=installed
    - name: configure zenity
      action: template src=hosts dest=/tmp/zenity.conf
    - name: Tell Master
      action: shell echo "{{ansible_fqdn}} done" >> /tmp/list
      delegate_to: 172.16.202.97
...
```
