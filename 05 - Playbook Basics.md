# Ansible Playbooks

In this section, we will see multiple examples of how to create playbooks which you might need to run regularly. Let’s try to install nginx server, configure them, patch them on CentOS server.  
**Go to practice session: 02/08 - Ansible Playbooks, Creating and Executing**

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

--Revision 09 and 10: Not needed.--
