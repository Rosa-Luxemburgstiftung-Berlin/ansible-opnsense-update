[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)
[![lint](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-update/actions/workflows/lint.yml/badge.svg)](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-update/actions?query=workflow%3Aansible-lint)
![Ansible 12 read](https://img.shields.io/badge/ansible_12-ready-green?logo=ansible&labelColor=black)

# ansible-opnsense-update
Perform a firmware update for opnsense via ansible.

This role handles community and enterprise releases!

## Role Variables

[defaults/main.yml](defaults/main.yml)

## Example

### Sample Playbook

`opnsenseupdate.yml`
```yaml
- name: opnsenseupdate
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

#### determine the next possible update/upgrade step

Just run
```
ansible-playbook opnsenseupdate.yml
```
to get a report of the next possible update target.

Example output for a update:
```
TASK [opnsense-update : announce available update] ****************************************************************************************************************************************************************
ok: [OPNsense] => {
    "msg": [
        "update available for opnsense from 23.7 to 23.7.12_5",
        "reboot_required: True"
    ]
}
```

Example output for a upgrade:
```
ok: [OPNsense2] => {
    "msg": [
        "upgrade available from 24.1.10_8 to 24.7",
        "reboot_required: True",
        "upgrade_major_message:",
        "<p>OPNsense 24.1 \"<em>Savvy Shark</em>\" has reached its end of life. As such it will not receive any more updates, but the upgrade to the new 24.7 series is seamless and can be performed right here from the web GUI.</p> <p> Another method is to import and reinstall using a new installation image, which will retain your settings using \"Import Configuration\", then reformat the disk and apply a clean system using either \"Install (ZFS)\" or \"Install (UFS)\".</p> <p>You can also upgrade via console / SSH by using option 12 from the menu by typing \"24.7\" when prompted.</p> <p>Make sure to read the migration notes and account for possible breaking changes.</p> <p>Please backup your configuration, preview the new version via live image or in a virtual machine. Create snapshots. If all else fails, report back <a href=\"https://forum.opnsense.org/\" target=\"_blank\">in the forums</a> for assistance.</p> "
    ]
}
```

#### update/upgrade

Update to the the next upgradable version.
(this might **not** be the latest version, as sometimes several iterations are required to reach this target)
(this should correspond to the way a update would have been performed using the WebUI).
In some cases this step must be repeated until the latest release is reached.
```
ansible-playbook -v -e opn_update=true -D opnsenseupdate.yml
```

### pkg upgrade

In order to run a `pkg upgrade` please use

```
ansible-playbook -e opn_pkg_upgrade=true ...
```

This can be run as a extra step or direct after a update/upgrade (combining `-e opn_pkg_upgrade=true` and `-e opn_update=true`)


### zfs snapshots

The role can create zfs snapshots before running a update/upgrade.
Use var `opn_zfs_snapshot: true` (default).

Default naming convention for snapshots is `opn_update__$CURRENT_OPNSENSE_VERSION$`.

Existing snapshots from previous runs can be removed.
Use var `opn_zfs_snapshot_delete_existing: true` (default).

## Notes

The playbook requires:
  * ansible version >= 2.11 [(due to the split filter)](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#manipulating-strings)
  * the lates [ansible-opnsense-facts](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-facts) to be run before this
  * user with shell access to the opnsense box

## Related

Related repositories:
  * [ansible-opnsense - Ansible role to configure OPNsense firewalls](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense)
  * [ansible-opnsense-plugpack - ansible playbook for opnsens handling plugins and packages](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-plugpack)
  * [ansible role installing check_mk agent on opnsense](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-checkmk)
  * [ansible-opnsense-playbook - draft on how we use the ansible opnsense roles @ RLS](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-opnsense-playbook)
