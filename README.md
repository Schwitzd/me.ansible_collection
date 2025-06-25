# Ansible Roles

Welcome to my personal Ansible roles repository, it contains a collection of Ansible roles that I have created and maintained for various automation tasks.

## Installation

To install this collection via `requirements.yaml`, create a file like this:

```yaml
---
collections:
  - name: https://github.com/Schwitzd/me.ansible_collection.git
    type: git
    version: main
```

Then install it using:

```sh
ansible-galaxy collection install -r requirements.yaml
```

To keep the collection update add the parameter `-force` in the above command.

## Roles

- **ssh_keygen**: Generates SSH key pairs locally and deploys the public key to remote systems for passwordless login.
- **sshd_hardening**: Applies a hardened and secure configuration to the OpenSSH daemon based on best practices and linting rules.
- **btrfs**: Partitions a full disk, formats it as Btrfs, optionally mounts it, and persists the configuration in `/etc/fstab`.
- **debian_upgrade**: Perform a full release upgrade on Debian or Raspberry Pi OS.
