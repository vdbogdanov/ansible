# ansible-roles

## Description

This repository contains ansible roles for automating server configuration using some utilities and service deployment.

The algorithm of using roles for utilities is very simple, they automate the process as if you enter commands in the console.  But the algorithm of using roles for the deployment of services requires some explanation:
```
1. Configuring the server to use the service (for example, installing the necessary packages)
2. Uploading the docker compose file to the server and running it
3. The ports on which containers are launched are indicated next to the name of the service
```

## List of roles

- Services
    - [openvpn (port 1194)](/roles/openvpn/)
    - [nginx (ports 80, 443)](/roles/nginx/)
    - [cmdbuild (port 8081)](/roles/cmdbuild)
    - [gitlab (ports 8082, 2224)](/roles/gitlab/)
    - [pgadmin (port 8083)](/roles/pgadmin/)
    - [vcenter (port 8084)](/roles/cmdbuild)

- Tools
    - [apt](/roles/apt/)
    - [ssh](/roles/ssh/)
    - [ufw](/roles/ufw)
    - [docker](/roles/docker/)

## Usage

### Initialization 

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
0.0.0.0 ansible_ssh_user=root ansible_ssh_pass=<password>
```

or if you use keys:

```ini
0.0.0.0 ansible_ssh_user=root ansible_ssh_private_key_file=~/.ssh/<private key>
```

### Playbook

For the convenience of using roles and tasks, a playbook ```site.yaml```. All roles and most tasks are tagged and can be invoked with the ```-t``` flag.

For example, install docker:
```bash
ansible-playbook site.yaml -t 'docker, docker_install'
```