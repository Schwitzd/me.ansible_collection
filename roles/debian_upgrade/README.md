# Ansible Role: debian_upgrade

This role performs a Debian or Raspberry Pi OS (Raspbian) release upgrade.

## Role Variables

### Required

| Variable                          | Description                | Default       |
|-----------------------------------|----------------------------|---------------|
| `debian_upgrade_target_release`   | Target codename to upgrade | `bookworm`    |

## Example Playbook

```yaml
- name: Upgrade Raspberry Pi OS to Bookworm
  hosts: raspberrypi
  become: true
  roles:
    - role: debian_upgrade
      vars:
        debian_upgrade_target_release: bookworm
