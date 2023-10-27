# ansible-roles

This repository contains ansible roles for automating server configuring and service deployment by Docker Compose.

## Table of Contents

- [Roles](#roles)
- [Getting Started](#getting-started)
- [Usage](#usage)

## Roles

1. [ansible_compose](https://github.com/vdbogdanov/ansible_compose)
2. [ansible_server](https://github.com/vdbogdanov/ansible_server)

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

```
server1 ansible_ssh_host=<server1_ip>
```

## Usage

For the convenience of using roles and tasks there is playbook `site.yaml`. All roles and most tasks are tagged and can be invoked with the `-t` flag.

Example:

```
ansible-playbook -i inventories/example site.yaml -t "server, upgrade_packages, ..."
```