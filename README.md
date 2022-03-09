[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)
[![lint](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-update/actions/workflows/lint.yml/badge.svg)](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-update/actions?query=workflow%3Aansible-lint)

# ansible-opnsense-update
perform a firmware update for opnsense via ansible

## Role Variables

[defaults/main.yml](defaults/main.yml)

## Example

### Sample Playbook

```yaml
- name: opnsense
  hosts: opnsense
  vars:
    ansible_become: false
  roles:
    - role: opnsense-facts
      tags:
        - opnsense
        - facts
    - role: opnsense-update
      tags:
        - opnsense
        - update
```

### Run

```
ansible-playbook -v -e opn_update_desired_version=22.1 -e opn_update_force=true -l opnsense -D firewalls.yml
ansible-playbook -v -e opn_update_desired_version=22.1.2 -l opnsense -D firewalls.yml
```
