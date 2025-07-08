# Ansible Role: debian_upgrade

This role performs a Debian or Raspberry Pi OS (Raspbian) release upgrade.

## Role variables

### Required

| Variable                        | Description                | Default    |
|---------------------------------|----------------------------|------------|
| `debian_upgrade_target_release` | Target codename to upgrade | `bookworm` |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.debian_upgrade
      vars:
        debian_upgrade_target_release: bookworm
```
