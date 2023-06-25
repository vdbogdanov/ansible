# ansible-roles

This repository contains ansible roles for automating server configuring using some tools and service deployment in docker containers by docker compose.

## Table of Contents

- [Getting Started](#getting-started)
- [Usage](#usage)
- [Roles](#roles)
	- [server](#server)
	- [secure](#secure)
	- [compose\_config](#compose_config)

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

| Task      	   | Description      		             |
| ---------------- | ----------------------------------- |
| upgrade_packages | Update & Upgrade packages.          |
| ssh    	       | Disable ssh PasswordAuthentication. |
| ufw  	           | Allow only 22 port.                 |
| docker           | Install Docker.                     |


### [secure](roles/secure)

| Task      	   | Description      		                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| docker_iptables  | Сlosing docker access to iptables via configuration file, because ufw can't close access to server ports for docker. |
| ip_restriction   | Configuring the ufw firewall to connect only from a specific ip.                                                     |

### [compose_config](roles/compose_config)

| Task     | Description      		                                   |
| -------- | --------------------------------------------------------- |
| openvpn  | Deploy OpenVPN and generate .ovpn config on your machine. |
| nginx    | Deploy Nginx as reverse proxy with HTTPS protocol.        |
| gitlab   | Deploy Gitlab service.                                    |
| cmdbuild | Deploy CMDBuild base                                      |
| pgadmin  | Deploy pgAdmin service.                                   |
| vcenter  | Deploy vcsim - vcenter simulator.                         |

Firstly, you should copy compose-collection on server:

```bash
ansible-playbook site.yaml -t "compose_config, copy_compose_collection"
```

To change the standard configuration of services, you can use [roles/compose_config/defaults/main.yaml](roles/compose_config/defaults/main.yaml).