# Architecture
![Ansible Architecture](https://github.com/rajeevsh990/masteringansible/blob/master/images/ansible-arch.png)

**Control Machine:**
The machine where Ansible is installed and where you are writing your playbooks. Windows can’t be used as CM, use Linux flavour. Can be called Master. 

**Inventory:** A file which keeps the information of your infrastructure. Node/host details (/etc/ansible/hsots). Concept of Inventory to manage and track the target machines. By default, this file is located in /etc/ansible/hosts and can be changed as well.

**Modules:** Modules are the main building blocks of Ansible and are basically reusable scripts that are used by Ansible playbooks. Ansible comes with a number of reusable modules. These include functionality for controlling services, software package installation, working with files and directories etc.

**Plugins:** Ansible uses a plugin architecture to enable a rich, flexible and expandable feature set.

**Playbooks:** The playbook consists of steps that the control mechanism will perform on the servers defined in the inventory file.

**Hosts/Nodes** The target/remote machine managed by Ansible. Where the palybooks runs. 
