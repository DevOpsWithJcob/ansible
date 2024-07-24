# Inventory 

manage hosts in 'inventory.ini' or 'inventory.yml'.

## How to build inventory

The simplest inventory is a single file with a list of hosts and groups. The default location for this file is `/etc/ansible/hosts`. You can specify a different inventory file at the command line using the `-i <path>` option or in configuration using `inventory`.

You can create a directory with multiple inventory files. See [Organizing inventory in a directory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#inventory-directory). These can use different formats (YAML, ini, and so on).

