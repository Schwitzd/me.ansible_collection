# Ansible Roles

Welcome to my personal Ansible roles repository, it contains a collection of Ansible roles that I have created and maintained for various automation tasks.

## How to Use

### SSH keygen

```yaml
---
- name: Duck SSH key provision
  hosts: localhost
  gather_facts: true
  collections:
    - schwitzd.collection

  roles:
    - role: ssh_keygen
      vars:
        ssh_user: k3s
        ssh_server: <hostname>
        ssh_key_name: <hostname>_ed25519
```

Run the following command, the ssh key pair will be created on the localhost, only only the public key will be added on the `authorized_keys` of the remote system.

```sh
ansible-playbook duck-ssh.yaml -u k3s -c paramiko --ask-pass
```
