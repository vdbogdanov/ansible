# ansible-roles
- [ansible-roles](#ansible-roles)
	- [Description](#description)
	- [List of roles](#list-of-roles)
	- [Usage](#usage)
			- [Initialization](#initialization)
			- [Playbook](#playbook)

## Description

A collection of ansible roles for configuring servers and deploying some services in conjunction with Docker Compose. Every role page has a more detailed description.

## List of roles

- Services
    - [openvpn](/roles/openvpn/)
    - [nginx](/roles/nginx/)
    - [gitlab](/roles/gitlab/)
    - [pgadmin](/roles/pgadmin/)
    - [cmdbuild](/roles/cmdbuild)
    - [vcsim](/roles/cmdbuild)
- Tools
    - [apt](/roles/apt/)
    - [ssh](/roles/ssh/)
    - [ufw](/roles/ufw)
    - [docker](/roles/docker/)

## Usage

#### Initialization 

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

#### Playbook

For the convenience of using roles and tasks, a playbook ```site.yaml```. All roles and most tasks are tagged and can be invoked with the ```-t``` flag.

For example, install docker:
```bash
ansible-playbook site.yaml -t 'docker, docker_install'
```