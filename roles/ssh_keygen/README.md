# SSH keygen

```yaml
---
- hosts: localhost
  roles:
    - role: schwitzd.collection.ssh_keygen
      vars:
        ssh_keygen_user: k3s
        ssh_keygen_server: <hostname>
        ssh_keygen_name: <hostname>_ed25519
```

Run the following command, the ssh key pair will be created on the localhost, only only the public key will be added on the `authorized_keys` of the remote system.

```sh
ansible-playbook duck-ssh.yaml -u k3s -c paramiko --ask-pass
```
