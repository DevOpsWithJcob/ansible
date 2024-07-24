# Ansible 

### Source

https://www.youtube.com/watch?v=ww4yY5ipYgo&list=PLRMCwJJwWR1AKYcUkdcorTFR-bhXUN6oO&index=1

-----

### Management Tools

Also name as 'Configuration Tools' or 'Automation Tools'. 

such as 

1. Ansbile
2. Puppet
3. Chef

--------

Ansible will be install on a control machine, like your laptop, PC, VM and etc.

then it will connect to servers with SSH protocol.

**SSH:** to connect to servers you need user_name and an authentication method (SSH KEY recommended in ansible).

SSH user: mostly 'root'

**Python:** you need to install python on your servers. 

**NOTE:** most linux systems include python by default.

Ansible will transmit python script on servers ,and then scripts will run on your servers.

**Ansible Roles**

We describe roles in Ansible.

- Role 
  -    -- Task1
  -    -- Task2
  -    -- Task3

And We have some hosts: h1, h2, h3, h4, h5, h6

WEB: h1, h2; DB: h3, h4; Storage: h5, h6

**Explain:**

We define some 'Roles' that each roles contains one or multiple 'Tasks', then classify 'Hosts' then execute roles on host groups.

## Ansible Structure

**Search:** 

> ansible structure best practice

**Structure**

- A main Directory for your Ansible project

  1- **Inventory:** contains hosts (it can have any name) 

  2- **Playbook:** Roles that must be execute (it can have any name)

  3- **Roles Directory:** it must name as 'Roles':  

    - **Tasks Directory:** contain tasks
      		- ​		**main.yml file:** exists in 'tasks' directory. file name is absolute. 

   -  **Vars Directory:** 
      -  contains **main.yml file**
   -  **files Directory**
      -  contains files and directories that ansible will copy to hosts.
   -  **templates Directory**
   -  **defaults Directory**

---

**ansible.cfg:** this file contains configurations of ansible project.

----

### Inventory files

contain hosts that we will config.

 two main formats of inventory file. 

- inventory.yml or inventory.yaml
- inventory.ini

**Example**

> inventory.ini 

```ini
[webservers]
webserver01 ansible_host=192.168.1.100
webserver02 ansible_host=192.168.1.101

[dbservers]
dbserver01 ansible_host=192.168.1.102
dbserver02 ansible_host=192.168.1.103

```

> inventory.yml

```yml
all:
  vars:
    ansible_user: ubuntu
    ansible_ssh_private_key_file: /path/to/key

  children:
    webservers:
      hosts:
        webserver01:
          ansible_host: 192.168.1.100
        webserver02:
          ansible_host: 192.168.1.101

```

--------

## Playbooks

A sequence of files where we can list roles and tasks.

-----

## Host_vars and Group_vars

