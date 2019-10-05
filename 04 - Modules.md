# Playbook Introduction & Commands:

![Ansible Architecture](https://github.com/rajeevsh990/masteringansible/blob/master/images/playbook-structure.png)

Playbooks define variables, configurations, deployment steps, assign roles, perform multiple tasks. Playbook is written in YAML format with a .yml file extension. One needs to be very careful with the format and alignment which makes it very sensitive.
It contains the following sections:
1.	Every playbook starts with 3 hyphens '---'
2.	Host section – Defines the target machines on which the playbook should run. This is based on the Ansible inventory file.
3.	Variable section – This is optional and can declare all the variables needed in the playbook. 
4.	Tasks section – This section lists out all the tasks that should be executed on the target machine. It specifies the use of Modules. Every task has a name which is a small description of what the task will do and will be listed while the playbook is run.

## Playbook commands:
To execute ansible playbook:  
> ansible-playbook <playbook.yml>

To check the playbook for syntax errors:  
> ansible-playbook <playbook.yml> --syntax-check

To view hosts list:  
> ansible-playbook <playbook.yml> --list-hosts

Before we go ahead and execute some playbooks let's have a look at Ansible Modules. 

## Ansible Modules

**Setup  module:**
To get information about the network or hardware or OS version or memory related information the setup module will help to gather the same about the target machines.

> ansible webservers –m setup

**Note:** webservers is the name of group defined in the hosts (inventory) file. You can change this as per your configuration.  


**Command module:**
The command module simply executes a specific command on the target machine and gives the output.
> ansible webservers –m command - a 'uptime'

> ansible webservers –m command –a 'hostname'

**Shell Module:**
To execute any command in the shell of your choice you can use the Shell module. The shell module commands are run in /bin/sh shell and you can make use of the operators like ‘>’ or ‘|’ (pipe symbol or even environment variables.
> ansible webservers -m shell -a 'ls -l > temp.txt'

> ansible webservers –m command -a 'cat temp.txt'

**User Module:** 
Using this module one can create or delete users.

> ansible webservers -m user -a 'name=user1 password=user1' --become

> ansible webservers -m user -a 'name=user1 state=absent' –-become

**File Module:**
This module is used to create files, directories, set, or change file permissions and ownership etc. 

-	Create file:

> ansible webservers -m file -a 'dest=/home/ansible/rajeev.txt state=touch mode=600 owner=ansible group=ansible'

-	Create directory

> ansible webservers -m file -a "dest=/home/ansible/vndir state=directory mode=755"

-	Delete a file

> ansible webservers -m file -a "dest=/home/ansible/rajeev.txt state=absent"

-	Delete a directory

> ansible webservers -m file -a "dest=/home/ansible/vndir state=absent"

**Explain: Idempotency**
 
**Copy Module:**
It is used for copying files to multiple target machines.

> ansible webservers -m copy -a "src=sample.txt dest=/home/ansible/sample.txt"

Managing Software packages Managing : If you need to install software packages through ‘yum’ or ‘apt’ you can use the below commands.

> ansible webservers –m yum -a "name=git state=present" --become


> ansible webservers -m yum -a "name=git state=latest" --become

> ansible webservers -m yum -a "name=httpd state=present" –-become

> ansible webservers -m yum -a "name=maven state=absent" –-become

**Managing Services Module:**

To manage services with ansible, we use a module ‘service’.

> ansible webservers -m service -a "name=httpd state=started" --become

> ansible webservers -m service -a "name=httpd state=stopped" --become

> ansible webservers -m service -a "name=httpd state=restarted" --become

**Fetch Module:**

To fetch/copy the file from remote server to local machine. E.g. create a file on remote server and fetch it.

> ansible docker –m file –a 'path=/tmp/test_modules.txt mode=600 state=touch'

> ansible docker –m fetch –a 'src=/tmp/test_modules.txt dest=/tmp/test_modules.txt'
