# ansible-roles

This repository contains ansible roles for automating server configuring and service deployment by Docker Compose.

## Table of Contents

- [Roles](#roles)
- [Getting Started](#getting-started)
- [Usage](#usage)

## Roles

1. [Personal](personal/)
   * [ansible_compose](https://github.com/vdbogdanov/ansible_compose)
   * [ansible_server](https://github.com/vdbogdanov/ansible_server)
2. 

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