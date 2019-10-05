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
