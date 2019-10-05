# Ansible Inventories
**Example 1:**
```
[centos]
centos1
centos2

[ubuntu]
ubuntu1
ubuntu2
```

**Example 2:** Connect your target node as root user.
```
[centos]
centos1 ansible_user=root
```

**Example 3:** Connect your target as non-root user and do sudo to become root.
```
[centos]
ubuntu ansible_become=true ansible_become_pass=password
```

**Example 4:** Connect on a different SSH port.
```
[centos]
centos1 ansible_user=root ansible_port=2222
```

**Example 5:** Different way to define SSH port
```
[centos]
centos1:2222 ansible_user=root
```

**Example 6:** Define ranges for common variables
```
[control]
ubuntu-c ansible_connection=local

[centos]
centos1 ansible_user=root ansible_port=2222
centos[2:3] ansible_user=root

[ubuntu]
ubuntu[1:3] ansible_become=true ansible_become_pass=password
```

**Example 7:** Define group variable separately
```
[control]
ubuntu-c ansible_connection=local

[centos]
centos1 ansible_port=2222
centos[2:3]

[centos:vars]
ansible_user=root

[ubuntu]
ubuntu[1:3]

[ubuntu:vars]
ansible_become=true
ansible_become_pass=password
```

**Example 8:** Set group vars for all 
```
[control]
ubuntu-c ansible_connection=local

[centos]
centos1 ansible_port=2222
centos[2:3]
[all:vars]
ansible_port=1234
```

Will look further into different types of inventory files we can define. Let's go to our practice session 02 - Ansible Architecture and Design/01 - Ansible Inventories/13

