# Ansible Vault


Most of the times when sensitive or confidential data need to be protected in the playbook, then it can be encrypted rather than just keeping it in a text file which is readable by all.  
Ansible Vault allows you to encrypt the playbook to protect the confidential data. For Example, consider the following task where a confidential job agreement is being copied.  

In such cases, you would need an Ansible Vault.

```
---
- hosts: webservers
  become: true
  tasks:
  - name: Copying Confidential Job Agreement
    copy: content="This is a Confidential Job Agreement" dest=/home/ansible/jobagreement.txt
 ``` 
Following are the steps that you need follow to encrypt the above playbook files.

1) Creating new encrypted files

To create new encrypted files with vault use the ansible-vault create command.

> ansible-vault create jobagreement.yml  

After confirming password an editing window will open to add contents to the file.  
Ansible will encrypt the contents when you close the file. Instead of seeing the actual contents you will see encrypted blocks.

2) To encrypt an existing yml file use the following

> ansible-vault encrypt existingfile.yml
Password will again be asked for encryption.

3) Viewing encrypted file
Use the command ansible-vault view to look at the actual contents of the file.

> ansible-vault view jobagreement.yml
You will be asked for the password again to look at the contents of the file.

4) Editing encrypted files
If you need to edit the file use the command ansible-vault edit

> ansible-vault edit users.yml
Enter the password to edit the file.
 
5) Changing password of the encrypted files
Use the command ansible-vault rekey to change the password of the file.

> ansible-vault rekey jobagreement.yml

6) Run an encrypted Ansible playbook file
Use the option –ask-vault-pass with the ansible-playbook command.

> ansible-playbook users.yml --ask-vault-pass

7) Manually decrypting the encrypted files
Use the command ansible-vault decrypt command.
> ansible-vault decrypt jobagreement.yml


**Summary:**
Well in this tutorial, we saw the two most important aspects of configuration management which are Ansible Playbooks and protecting sensitive data using Ansible Vaults.


# Ansible Roles
Reference: https://www.learnitguide.net/2018/02/ansible-roles-explained-with-examples.html  

With Ansible roles you can group your variables, tasks, handlers etc., which increase reusability and most certainly reduce syntax errors. Ansible roles are similar to modules in Puppet and cookbooks in Chef.
In order to create roles, you use the ansible-galaxy command which has all the templates to create it.  

**Example Scenario**  
Example in Continuous Delivery where I am deploying a new build of my J2EE application (WAR file) to tomcat my steps would be as follows:
- Stop the application
- Uninstall the application
- Deploy the new build of an application
- Start the application

So I would be creating a role with at least 4 tasks and one main file calling it. This way I am making my code more modular and reusable. So let’s call this role as tomcat and create it.

> cd /etc/ansible/roles  
> sudo ansible-galaxy init apache –offline

Once the role is created you can see the directory structure which it has created.
The main components we will use in this section include:
- tasks/main.yml – This is the starting point for tasks created for the role. You can use the main.yml file to point to the other task files.
- vars – This is to define any variables used.
- handlers - contains handlers, which may be used by this role or even anywhere outside this role.
- defaults - default variables for the role.
- files - contains files required to transfer or deployed to the target machines via this role.
- templates - contains templates which can be deployed via this role.
- meta - defines some data / information about this role (author, dependency, versions, examples, etc,.)

