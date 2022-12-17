# How to set up a webserver on a machine with Ansible and Apache! :)

### 1.  Install Apache web server
___
To install Apache, you can use the `apt` module. Here is an example playbook task:

`- name: Install Apache web server`  
`apt:`  
`name: apache2`  
`state: present`

### 2.  Start Apache service
___
To start the Apache service, you can use the `service` module. Here is an example playbook task:

`- name: Start Apache`  
`service:`  
`name: apache2`  
`state: started`

### 3.  Enable Apache service to start on boot
___
To enable the Apache service to start on boot, you can use the `systemd` module. Here is an example playbook task:

`- name: Enable Apache service`  
`systemd:`  
`name: apache2`  
`state: enabled`

### 4.  Create a virtual host configuration file
___
To create a virtual host configuration file, you can use the `template` module. Here is an example playbook task:

`- name: virtual host configuration file`  
`template:`  
`src: templates/virtualhost.conf.j2`  
`dest: /etc/apache2/sites-available/example.com.conf`

##### What is a virtual host configuration file? 
-   A virtual host configuration file tells a webserver how to handle requests for a specific website.
-   It allows the webserver to host multiple websites on the same server, each with its own unique domain name.
-   A virtual host configuration file contains information about the specific domain name and web root directory for a website.
-   When a visitor sends a request to the webserver, the webserver looks at the virtual host configuration file for the domain name and serves the website files from the appropriate web root directory.

In this example, `templates/virtualhost.conf.j2` is the path to the template file on the Ansible control machine, and `/etc/apache2/sites-available/example.com.conf` is the path on the target host where the virtual host configuration file will be created.

### 5.  Enable the virtual host
___
To enable the virtual host, you can use the `apache2_module` module. Here is an example playbook task:

`- name: enable virtual host`  
`apache2_module:`  
`name: ssl`  
`state: present`  
`state: enabled`

### 6.  Restart Apache service
___
To restart the Apache service, you can use the `service` module. Here is an example playbook task:

`- name: Restart Apache`  
`service:`  
`name: apache2`  
`state: restarted`
___
#### Here is the complete playbook that puts all of these tasks together:

`---`  
`- hosts: all`
`  become: true`  
`tasks:`  
`- name: Install Apache web server`  
`apt:`  
`name: apache2`  
`state: present`  
`- name: Start Apache`  
`service:`  
`name: apache2`  
`state: started`  
`- name: enable Apache`  
`systemd:`  
`name: apache2`  
`state: enabled`  
`- name: virtual host configuration file`  
`template:`  
`src: templates/virtualhost.conf.j2`  
`dest: /etc/apache2/sites-available/example.com.conf`  
`- name: Enable virtual host`  
`apache2_module:`  
`name: ssl`  
`state: present`  
`state: enabled`  
`- name: Restart Apache`  
`service:`  
`name: apache2`  
`state: restarted`
