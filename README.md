# Ansible Playbook for installing 2x backend servers (httpd) and 1x nginx load balancer

This playbook installs and configures two backend servers running httpd and one nginx load balancer. The nginx load balancer receives requests from clients and forwards them to one of the backend servers. The nginx load balancer runs on SSL, while the communication between nginx and httpd is plaintext.

## Requirements
- Ansible 2.10 or higher
- Three hosts: one for nginx and two for httpd
- A valid SSL certificate and key for nginx

## How to use
- Edit the `inventory` file and specify the IP addresses of the hosts under the `[nginx]` and `[httpd]` groups.
- Edit the `vars.yml` file and specify the path to the SSL certificate and key files for nginx under the `ssl_cert` and `ssl_key` variables.
- Run the playbook with the command: `ansible-playbook -i inventory main.yml`

## How it works
- The playbook consists of two roles: `nginx` and `httpd`.
- The `nginx` role installs and configures nginx on the host in the `[nginx]` group. It copies the SSL certificate and key files to the host, and creates a configuration file for nginx that enables SSL and sets up a proxy to the backend servers. It also enables and starts the nginx service.
- The `httpd` role installs and configures httpd on the hosts in the `[httpd]` group. It creates a simple index.html file that displays the hostname of the server, and enables and starts the httpd service.
- The playbook uses the `upstream` directive in nginx to define a group of backend servers, and the `proxy_pass` directive to forward requests to them. It also uses the `proxy_set_header` directive to pass the original host and protocol to the backend servers.
