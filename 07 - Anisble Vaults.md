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
