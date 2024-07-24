# Project 1

Connect to server using ssh authentication key. install and configure Nginx and set SSL certificate for simple index.html

**Project structure**

```bashansible-project/
├── files/
│   └── index.html
├── hosts
├── nginx_setup.yml
└── templates/
    └── nginx_ssl.conf.j2
```

## Setup SSH Authentication key

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

```bash
ssh-copy-id your_user@192.168.35.25
```

add ssh config file (recommended)

```plaintext
Host 192.168.35.25
    User your_user
    IdentityFile ~/.ssh/id_rsa
```

## Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
```

## Add hosts

touch 'hosts' file (hosts.ini)

```bash
touch hosts
```

add this config to hosts

```ini
[webservers]
192.168.35.25 ansible_user=your_user ansible_ssh_private_key_file=~/.ssh/id_rsa
```

**Create multiple directories**

```bash
mkdir -p ~/ansible-project/{group_vars,host_vars,roles/nginx/tasks,templates}
```

## Add Playbook

```yml
---
- name: Setup NGINX with SSL
  hosts: webservers
  become: true
  tasks:
    - name: Ensure NGINX is installed
      apt:
        name: nginx
        state: present
        update_cache: true

    - name: Ensure NGINX service is running and enabled
      service:
        name: nginx
        state: started
        enabled: true

    - name: Create SSL directory
      file:
        path: /etc/nginx/ssl
        state: directory
        owner: root
        group: root
        mode: 0700

    - name: Generate self-signed SSL certificate and key
      command: openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=US/ST=Denial/L=Springfield/O=Dis/CN=www.example.com" -keyout /etc/nginx/ssl/certificate.key -out /etc/nginx/ssl/certificate.crt

    - name: Copy the index.html file
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
        owner: www-data
        group: www-data
        mode: '0644'

    - name: Configure NGINX to use SSL
      template:
        src: templates/nginx_ssl.conf.j2
        dest: /etc/nginx/sites-available/default

    - name: Test NGINX configuration
      command: nginx -t
      notify: Restart NGINX

  handlers:
    - name: Restart NGINX
      service:
        name: nginx
        state: restarted
```

## Add nginx configs

Create a template file named `templates/nginx_ssl.conf.j2`:

```j2
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    server_name _;

    ssl_certificate /etc/nginx/ssl/certificate.crt;
    ssl_certificate_key /etc/nginx/ssl/certificate.key;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

```

## Run Ansible project

```bash
ansible-playbook -i hosts nginx_setup.yml
```

**OR**

```bash
ansible-playbook -i hosts nginx_setup.yml --ask-become-pass
```

**List hosts**
```bash
ansible-inventory -i inventory --list
```


