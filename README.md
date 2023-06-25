# ansible-roles

This repository contains ansible roles for automating server configuring using some tools and service deployment in docker containers by docker compose.

## Table of Contents

- [Getting Started](#getting-started)
- [Usage](#usage)
- [Roles](#roles)
	- [server](#server)
		- [upgrade\_packages](#upgrade_packages)
		- [ssh](#ssh)
		- [ufw](#ufw)
		- [docker](#docker)
	- [secure](#secure)
		- [docker\_iptables](#docker_iptables)
		- [ip\_restriction](#ip_restriction)
	- [compose\_config](#compose_config)
		- [openvpn](#openvpn)
		- [nginx](#nginx)
		- [gitlab](#gitlab)
		- [cmdbuild](#cmdbuild)
		- [pgadmin](#pgadmin)
		- [vcenter](#vcenter)

## Getting Started

Clone the repository:

```bash
git clone https://github.com/dallings/ansible-roles.git
cd ansible-roles
```

Initiate available [compose files folder](roles/compose_config/files/compose-collection/) as submodule, and pull the latest versions:

```bash
git submodule init
git submodule update --recursive --remote
```

Rename inventory_example.ini -> inventory.ini:

```bash
mv inventory_example.ini inventory.ini
```

Change ip addresses to yours in inventory:

```ini
server1 ansible_ssh_host=<your_ip>
```

## Usage

For the convenience of using roles and tasks there is playbook `site.yaml`. All roles and most tasks are tagged and can be invoked with the `-t` flag.

Example:

```bash
ansible-playbook site.yaml -t "compose_config, pgadmin, openvpn, ..."
```

## Roles

### [server](roles/server)

#### upgrade_packages

Update & Upgrade packages.

#### ssh

Disable ssh PasswordAuthentication.

#### ufw

Allow only 22 port.

#### docker

Install Docker.

---

### [secure](roles/secure)

#### docker_iptables

Сlosing docker access to iptables via configuration file, because ufw can't close access to server ports for docker.

#### ip_restriction

Configuring the ufw firewall to connect only from a specific ip.

---

### [compose_config](roles/compose_config)

With this role you can configure and start some services by Docker Compose. If you want use my compose-collection, you should copy it on server:

```bash
ansible-playbook site.yaml -t "compose_config, copy_compose_collection"
```

To change the standard configuration of services, you can use [role defaults](roles/compose_config/defaults/main.yaml).

#### openvpn

Deploy OpenVPN and generate .ovpn config on your machine.

#### nginx

Deploy Nginx as reverse proxy with HTTPS protocol. In my case for gitlab, pgadmin and vcenter docker containers, but you can change `/roles/nginx/files/templates/nginx.conf.template` according to your needs. Unlike previous nginx services, reverse proxy requires a domain name and certificates to establish an HTTPS connection (you can modify the nginx configuration to use the HTTP protocol).

#### gitlab

Deploy Gitlab service where `username = root`, for get `password` you should run this command in terminal:

```bash
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

#### cmdbuild

Deploy CMDBuild base on `http://<ip>:<port>/cmdbuild` where `username = cmdbuild` and `password = cmdbuild`.

#### pgadmin

Deploy pgAdmin service.

#### vcenter

Deploy vcsim - vcenter simulator in docker container `nimmis/vcsim`. If you want use it via HTTP, you should use official container. You can test connection on `https://<ip>:<port>/about`.