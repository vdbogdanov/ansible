# ansible-roles

This repository contains ansible roles for automating server configuration using some utilities and service deployment.

```
The algorithm of using roles for tools is very simple. But the algorithm of using roles for the deployment of services requires some explanation:
1. Configuring the server to use the service (for example, installing the necessary packages)
2. Uploading the docker compose file to the server and running it
3. Docker containers with services work on ports according to the [table](#services-ports) (you can change it in docker compose file of service)
4. If you to want more secure passwords in services, you can change defaults for role or create env vars with identical names as in defaults.
```

## Table of Contents

- [Initialization](#initialization)
- [Services ports](#services-ports)
- [Usage](#usage)
    - Services
        - [openvpn](#openvpn)
        - [nginx](#nginx)
        - [cmdbuild](#cmdbuild)
        - [gitlab](#gitlab)
        - [pgadmin](#pgadmin)
        - [vcenter](#vcenter)
    - Tools
        - [apt](#apt)
        - [ssh](#ssh)
        - [ufw](#ufw)
        - [docker](#docker)

## Initialization

Clone the repository:

```bash
git clone https://github.com/dallings/ansible-roles.git
```

Create inventory.ini:

```bash
cd ansible-roles
touch inventory.ini
```

Add your hosts in inventory:

```ini
0.0.0.0 ansible_ssh_user=root ansible_ssh_private_key_file=~/.ssh/<private key>
```

## Services ports

| Service   | Ports      |
| --------- | ---------- |
| openvpn   | 1194       |
| nginx     | 80, 443    |
| cmdbuild  | 8081       |
| gitlab    | 8082, 2224 |
| pgadmin   | 8083       |
| vcenter   | 8084       |

## Usage

For the convenience of using roles and tasks, a playbook `site.yaml`. All roles and most tasks are tagged and can be invoked with the `-t` flag.

### [openvpn](roles/openvpn/)

Deploy OpenVPN and generate .ovpn config on desktop.

`client_pass = admin` – pasword for generated config

Start deployment:

```bash
ansible-playbook site.yaml -t 'openvpn, ovpn_setup, ovpn_gencerts, ovpn_genprofile'
```

### [nginx](roles/nginx/)

Deploy Nginx as reverse proxy with HTTPS protocol. In my case for gitlab, pgadmin and vcenter docker containers, but you can change `/roles/nginx/files/templates/nginx.conf.template` according to your needs. Unlike previous nginx services, reverse proxy requires a domain name and certificates to establish an HTTPS connection (you can modify the nginx configuration to use the HTTP protocol).

Example of the defaults file `/roles/nginx/main.yaml`:

```yaml
---
domain:                     example.com
domain_certificate_path:    /root/letsencrypt/fullchain.pem
domain_privatekey_path:     /root/letsencrypt/privkey.pem
nginx_config_template_path: files/templates/nginx.conf.template
```

Start deployment:

```bash
ansible-playbook site.yaml -t 'nginx, nginx_reverse_proxy'
```

### [cmdbuild](roles/cmdbuild/)

Deploy CMDBuild base on `http://<ip>:8081` where `username = cmdbuild` and `password = cmdbuild`.

Start deployment:

```bash
ansible-playbook site.yaml -t 'cmdbuild, cmdbuild_setup'
```

### [gitlab](roles/gitlab/)

Deploy Gitlab base on `http://<ip>:8082` where `username = root`, for get `password` you should run this command in terminal:

```bash
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

Start deployment:

```bash
ansible-playbook site.yaml -t 'gitlab, gitlab_setup'
```

### [pgadmin](roles/pgadmin/)

Deploy pgAdmin on `http://<ip>:8083` where `username = admin@github.com` and `password = admin`.

Start deployment:

```bash
ansible-playbook site.yaml -t 'pgadmin, pgadmin_setup'
```

### [vcenter](roles/vcenter/)

Deploy vcsim - vcenter simulator in docker container `nimmis/vcsim`. If you want use it via HTTP, you should use official container. You can test connection on `https://<ip>:8084/about`. 

Start deployment:

```bash
ansible-playbook site.yaml -t 'vcenter, vcenter_setup'
```

---

### [apt](roles/apt/)

`apt_upgrade` – Update & Upgrade packages

### [ssh](roles/ssh/)

`ssh_setup` – Disable ssh PasswordAuthentication

### [ufw](roles/ufw/)

`ufw_common_setup` – Allow only 22 port

`ufw_secure_setup (not tagged)` – Allow only from specific ip address

### [docker](roles/docker/)

`docker_install` – Install docker

`docker_iptables_off (not tagged)` – Closes docker access to iptables (if you configure a firewall via ufw and do not close docker access to iptables, docker containers will ignore the specified rules)
