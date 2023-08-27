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
    - role: ansible-opnsense-facts
      tags:
        - opnsense
        - facts
    - role: ansible-opnsense-update
      tags:
        - opnsense
        - update
```

### Run


update to a fixed main release version
```
ansible-playbook -v -e opn_update_desired_version=22.1.5 -l opnsense -D firewalls.yml
```

update to a fixed hotfix release
```
ansible-playbook -v -e opn_update_desired_version=23.1.5_4 -l opnsense -D firewalls.yml
```

update to the lates version available
```
ansible-playbook -v -e opn_update_force=true -l opnsense -D firewalls.yml
```

## Notes
The playbook requires:
  * ansible version >= 2.11 [(due to the split filter)](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#manipulating-strings)
  * the lates [ansible-opnsense-facts](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-facts) to be run before this
  * user with shell access to the opnsense box
