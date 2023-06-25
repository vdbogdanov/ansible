# ansible-roles

This repository contains ansible roles for automating server configuring and service deployment by Docker Compose.

## Table of Contents

- [Getting Started](#getting-started)
- [Usage](#usage)
- [Roles](#roles)
	- [server](#server)
	- [secure](#secure)
	- [compose\_config](#compose_config)

## Getting Started

Clone the repository:

```
git clone https://github.com/dallings/ansible-roles.git
cd ansible-roles
```

Initiate [compose-collection repository](https://github.com/vdbogdanov/compose-collection) as submodule, and pull the latest versions:

```
git submodule init
git submodule update --recursive --remote
```

Rename inventory_example.ini -> inventory.ini:

```
mv inventory_example.ini inventory.ini
```

Change ip addresses to yours in inventory:

```ini
server1 ansible_ssh_host=<your_ip>
```

## Usage

For the convenience of using roles and tasks there is playbook `site.yaml`. All roles and most tasks are tagged and can be invoked with the `-t` flag.

Example:

```
ansible-playbook site.yaml -t "server, upgrade_packages, ..."
```

## Roles

### [server](roles/server/)

| Task      	   | Description      		             |
| ---------------- | ----------------------------------- |
| upgrade_packages | Update & upgrade packages.          |
| ssh    	       | Disable ssh PasswordAuthentication. |
| ufw  	           | Allow only 22 port.                 |
| docker           | Install docker.                     |


### [secure](roles/secure/)

| Task      	   | Description      		                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| docker_iptables  | Сlosing docker access to iptables via configuration file, because ufw can't close access to server ports for docker. |
| ip_restriction   | Configuring the ufw firewall to connect only from a specific ip.                                                     |

### [compose_config](roles/compose_config/)

**Before using this role, I highly recommend to read README.md in [compose-collection repository](https://github.com/vdbogdanov/compose-collection)!**

| Task     | Description      		                                   |
| -------- | --------------------------------------------------------- |
| openvpn  | Deploy OpenVPN and generate .ovpn config on your machine. |
| nginx    | Deploy Nginx as reverse proxy with HTTPS protocol.        |
| gitlab   | Deploy Gitlab service.                                    |
| cmdbuild | Deploy CMDBuild base                                      |
| pgadmin  | Deploy pgAdmin service.                                   |
| vcenter  | Deploy vcsim - vcenter simulator.                         |

For using this role, you should copy compose-collection on your server:

```
ansible-playbook site.yaml -t "compose_config, copy_compose_collection"
```

To change the standard configuration of services, you can use [roles/compose_config/defaults/main.yaml](roles/compose_config/defaults/main.yaml).