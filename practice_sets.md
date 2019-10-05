# Architecture
![Ansible Architecture](https://github.com/rajeevsh990/masteringansible/blob/master/images/ansible-arch.png)

**Control Machine:**
The machine where Ansible is installed and where you are writing your playbooks. Windows can’t be used as CM, use Linux flavour. Can be called Master. 

**Inventory:** A file which keeps the information of your infrastructure. Node/host details (/etc/ansible/hsots). Concept of Inventory to manage and track the target machines. By default, this file is located in /etc/ansible/hosts and can be changed as well.

**Modules:** Modules are the main building blocks of Ansible and are basically reusable scripts that are used by Ansible playbooks. Ansible comes with a number of reusable modules. These include functionality for controlling services, software package installation, working with files and directories etc.

**APIs:**

**Plugins:** Ansible uses a plugin architecture to enable a rich, flexible and expandable feature set.

**Playbooks:** The playbook consists of steps that the control mechanism will perform on the servers defined in the inventory file.

**Hosts/Nodes** The target/remote machine managed by Ansible. Where the palybooks runs. 

# Installation of Ansible on CentOS
a) Create a common id on both the machines (master and target) , for Example, ansible with SUDO privileges. This id will be used for communicating across all the machines involved for automation of tasks.

> useradd ansible
> passwd ansible

b) Edit the /etc/ssh/sshd_config file on the control machine and uncomment out the lines for PasswordAuthentication and PermitRootLogin
Once completed, restart the sshd service on both the machines.

> systemctl restart sshd

c) To enable passwordless authentication to perform the steps shown below. Firstly add the user ansible to the /etc/sudoers file on both the machines which will enable the user ansible to run any command which requires root privileges.
> vi /etc/sudoers
ansible	ALL=(ALL)	NOPASSWD: ALL

e) Going forward we will use the user ansible to perform all the steps. So switch to the user ansible and generate ssh keys. 
Control Machine su – ansible AND Target Machine su – ansible

> ssh-keygen
> ssh-copy-id <IP-Address-Host-Machine>
> ssh-copy-id <IP-Address-Control-Machine>

*Note:* if you are getting error then change the settings in file /etc/ssh/sshd_config 
PasswordAuthentication yes

Restart daemon: 
> sudo systemctl restart sshd

f) Install wget if not installed on both the machines.

> sudo yum install wget -y

g) We can now install ansible on the Control machine only by enabling the EPEL repo from fedora which provides add-on software packages. Perform the following steps to install ANSIBLE.

> wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

> sudo rpm -ivh epel-release-latest-7.noarch.rpm

> sudo yum install ansible -y 

> ansible --version 

The ansible version used is 2.5.3

h) Edit the ansible.cfg file and enable the inventory file parameter on the Control machine.

> sudo vi /etc/ansible/ansible.cfg

i) Every group is denoted by a square bracket and a group name within. A server can actually exist in multiple groups. Edit the inventory file /etc/ansible/hosts and add all the servers which need to be managed.

> vim /etc/ansible/hosts  
[webserver]  
192.168.0.2

j) To test the connectivity of the servers under the webservers group run the ansible ping command as shown
> ansible webservers –m ping

To list the hosts in the inventory file, you can run the below command
> ansible webservers --list-hosts

 
** YAML: **
Ansible dynamically converts YAML code into python code and uses the same module piYaml what python uses. 
Online parser: http://yaml-online-parser.appspot.com

# Playbook Introduction & Commands:

![Ansible Architecture](https://github.com/rajeevsh990/masteringansible/blob/master/images/playbook-structure.png)

Playbooks define variables, configurations, deployment steps, assign roles, perform multiple tasks. Playbook is written in YAML format with a .yml file extension. One needs to be very careful with the format and alignment which makes it very sensitive.
It contains the following sections:
1.	Every playbook starts with 3 hyphens '---'
2.	Host section – Defines the target machines on which the playbook should run. This is based on the Ansible inventory file.
3.	Variable section – This is optional and can declare all the variables needed in the playbook. 
4.	Tasks section – This section lists out all the tasks that should be executed on the target machine. It specifies the use of Modules. Every task has a name which is a small description of what the task will do and will be listed while the playbook is run.

## Playbook commands:
> ansible-playbook <playbook.yml>

To check the playbook for syntax errors
> ansible-playbook <playbook.yml> --syntax-check

To view hosts list
> ansible-playbook <playbook.yml> --list-hosts

**Setup  module:**
To get information about the network or hardware or OS version or memory related information the setup module will help to gather the same about the target machines.

> ansible webservers –m setup

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

# Ansible Inventory

Ansible’s inventory file, which defaults to being saved in the location /etc/ansible/hosts. The format for /etc/ansible/hosts is an INI-like format. You can specify a different inventory file using the i  <path> option on the command line.
mail.example.com

[webservers]  
foo.example.com  
bar.example.com

[dbservers]  
one.example.com  
two.example.com  
three.example.com

Go through the below reference:
**refrence:** https://docs.ansible.com/ansible/2.3/intro_inventory.html

**Practice this module now: 02 - Ansible Architecture and Design**

# Ansible Playbooks

In this section, we will see multiple examples of how to create playbooks which you might need to run regularly. Let’s try to install nginx server, configure them, patch them on CentOS server.  
Go to: Ansible Playbooks, Creating and Executing

**Revision 01:** Will be applying our code to only CentOS server using facts

> ansible all -i hosts -m setup -a 'filter=ansible_distribution'  
> ansible-playbook nginx_playbook.yml

**Revision 02:** Combined apt and yum module using package module.
> ansible-playbook nginx_playbook.yml

**Revision 03:** Install patch utility on CentOS server and execute the playbook
> ansible-playbook nginx_playbook.yml

**Revision 04:** Create required directories and link
> ansible-playbook nginx_playbook.yml

**Revision 05:** Templating the file index.html.j2 and follow other tasks
> ansible-playbook nginx_playbook.yml

**Revision 06:** Added firewall module to enable http traffic
> ansible-playbook nginx_playbook.yml

**Revision 07:** added {{ ansible_managed }} – (a configuration file attribute) line in template file. When added this line the file is not supposed to edit manually. 
> ansible-playbook nginx_playbook.yml

**Revision 08:** added config in ansible.cfg regarding {{ansible_managed}} 
> ansible-playbook nginx_playbook.yml

**Revision 09 and 10: Not needed.**

# Ansible Facts

**Basic facts**  
- To view network related facts collected from docker host  
> ansible docker -m setup -a 'gather_subset=network'
- To narrow down the output and get only important data  
> ansible docker -m setup -a 'gather_subset=network,!all,!min'
- Use filter to get specific fact about host  
> ansible docker -m setup -a 'filter=ansible_memfree_mb'
- Use wildcasd in above command  
> ansible docker -m setup -a 'filter=ansible_mem*'

**Get started with revisions : 01**


**Custom facts:** Custom facts can be written in any language. It retruns a JSON or INI structure. By default ansible expects custom facts to be placed in /etc/anisble/facts.d but can changed for non-root users.

**Revisions : 02 – custom facts on local host (control master)**  
- Copy the templates (2 files) in /etc/ansible/facts.d and ensure they are getting exected.  
> cp templates/* /etc/anisble/facts.d  
> ansible jenkins -m setup | tee /tmp/x  
> ansible jenkins -m setup -a 'filter=ansible_local'

**Revisions : 03 – Let’s add custom facts on remote host**
-	Limit only to jenkins loca host machine  
> ansible-playbook facts_playbook.yml -l jenkins

**Revisions : 04 – Let’s get the custom fact value from hostvars**

- Once the facts are available in ansible_local they are also available for hostvars section.  
> ansible-playbook facts_playbook.yml -l jenkins

**Revisions : 05 – Let’s add custom facts on remote host**
- Copy fact script to remote server and execute them  
> ansible-playbook facts_playbook.yml

**Revisions : 06 – Let’s add add custom facts using non-root user**
- Copy fact script to remote server in non-root user’s home directory  
> ansible-playbook facts_playbook.yml
