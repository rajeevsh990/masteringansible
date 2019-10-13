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

**Examples**
Let's navigate to Revision: 04\03 - Using Roles
- Revision 01:
> ansible-galaxy init nginx

- Revision 02: Move the playbook code or variables into respective files or folders. 
> ansible-playbook nginx_playbook.yml

- Revision 03: Now the website related thing is moved to a separate role - awesomeweb
> ansible-playbook nginx_playbook.yml
