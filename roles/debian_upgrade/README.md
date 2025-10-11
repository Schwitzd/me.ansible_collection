# Ansible Role: debian_upgrade

This role performs a Debian or Raspberry Pi OS (Raspbian) release upgrade.

## Role variables

### Required

| Variable                        | Description                | Default    |
|---------------------------------|----------------------------|------------|
| `debian_upgrade_target_release` | Target codename to upgrade | `bookworm` |

### Optionals

| Variable                      | Description                                 | Default |
|-------------------------------|---------------------------------------------|---------|
| `debian_upgrade_allow_reboot` | Whether to automatically reboot if required | `true`  |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.debian_upgrade
      vars:
        debian_upgrade_target_release: bookworm
        debian_upgrade_allow_reboot: false
```

> > **Note:** It’s strongly recommended to define `ansible_python_interpreter: /usr/bin/python3` in your `group_vars/all.yaml`. This ensures Ansible continues to work after an OS upgrade, since the default interpreter path is version-specific (e.g. `/usr/bin/python3.11`) and may disappear when Debian upgrades Python to a new major version.
