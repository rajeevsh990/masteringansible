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

**Get started with revisions : 06 - Ansible Playbooks, Facts/01**


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
