## Usage

For the convenience of using roles and tasks there is playbook `site.yaml`. All roles and most tasks are tagged and can be invoked with the `-t` flag.

Example:

```
ansible-playbook -i inventories/example site.yaml -t "server, upgrade_packages, ..."
```