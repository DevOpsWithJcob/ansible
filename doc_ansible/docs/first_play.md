# Ansible First Playbook

Ansible documnt

source: https://docs.ansible.com

## Inventory

add hosts with fully qualified domain name (FQDN) or host IP address in a file named inventory.ini (name is optional).

```ini
[webservers]
192.168.35.25
192.168.35.26
```

**Verify inventory**

```bash
ansible-inventory -i inventory.ini --list
```

**Ping hosts**

```bash
ansible webservers -m ping -i inventory.ini
```

**Tips for building inventory**

>Ensure that group names are meaningful and unique. Group names are also case sensitive.
>
>Avoid spaces, hyphens, and preceding numbers (use `floor_19`, not `19th_floor`) in group names.
>
>Group hosts in your inventory logically according to their **What**, **Where**, and **When**. 
>
>- What
>
>  Group hosts according to the topology, for example: db, web, leaf, spine.
>
>- Where
>
>  Group hosts by geographic location, for example: datacenter, region, floor, building.
>
>- When
>
>  Group hosts by stage, for example: development, test, staging, production.

### 

## Playbook

Playbooks are automation blueprints, in `YAML` format, that Ansible uses to deploy and configure managed nodes.

- Playbook

  A list of plays that define the order in which Ansible performs operations, from top to bottom, to achieve an overall goal.

- Play

  An ordered list of tasks that maps to managed nodes in an inventory.

- Task

  A reference to a single module that defines the operations that Ansible performs.

- Module

  A unit of code or binary that Ansible runs on managed nodes. Ansible modules are grouped in collections with a [Fully Qualified Collection Name (FQCN)](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html#term-Fully-Qualified-Collection-Name-FQCN) for each module.

  

**playbook.yml**

```yml
- name: my first play
  hosts: webservers
  tasks:
  	- name: ping host1
  	  ansible.builtin.ping:
  	  
  	- name: print message
  	  ansible.builtin.debug:
  	    msg: Hello world
```



**Run playbook**

```bash
ansible-playbook -i inventory.ini playbook.ylm
```





