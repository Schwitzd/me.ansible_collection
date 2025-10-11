# Role: ssh_keygen

This Ansible role creates an Ed25519 SSH key pair on the control node (the machine running Ansible), ensures secure permissions, optionally encrypts the private key with a random passphrase, and copies the public key to a remote host’s `authorized_keys`.

> [!NOTE]
> The local user that runs the play (`ansible_user_id`) is the account whose `~/.ssh` directory is used.

## Role Variables

### Required

| Variable            | Description                                                        |
|---------------------|--------------------------------------------------------------------|
| `ssh_keygen_user`   | Remote account that will receive the public key in `authorized_keys`. |
| `ssh_keygen_server` | Inventory host (or reachable hostname) where the public key is installed. |

### Optional

| Variable                          | Description                                                                    | Default        |
|-----------------------------------|--------------------------------------------------------------------------------|----------------|
| `ssh_keygen_name`                 | Base name for the key files (`<name>` and `<name>.pub`).                       | `id_ed25519`   |
| `ssh_keygen_encrypt_privatekey`   | When `true`, encrypt the private key with a generated passphrase.             | `true`         |
| `ssh_keygen_passphrase_length`    | Length of the generated passphrase (used when encryption is enabled).         | `32`           |
| `ssh_keygen_copy_key`             | When `false`, skip copying the public key to the remote host.                 | `true`         |

## Example Usage

### Generate and Deploy an Encrypted Key

```yaml
- hosts: localhost
  gather_facts: false
  roles:
    - role: schwitzd.collection.ssh_keygen
      vars:
        ssh_keygen_user: "deploy"
        ssh_keygen_server: "bastion"
        ssh_keygen_name: "deploy_prod_ed25519"
        ssh_keygen_encrypt_privatekey: true
```

### Generate a Local Key Only

```yaml
- hosts: localhost
  gather_facts: false
  roles:
    - role: schwitzd.collection.ssh_keygen
      vars:
        ssh_keygen_user: "deploy"
        ssh_keygen_server: "bastion"
        ssh_keygen_copy_key: false
```

When encryption is enabled, the generated passphrase is saved in `/tmp/<key_name>_passphrase.txt` on the control node for later retrieval. Remove it or store it securely once the key is deployed. When `ssh_keygen_copy_key` is `true`, the task delegates to `ssh_keygen_server` to install the public key into the target user’s `authorized_keys`. Use paramiko or another connection type if the remote host does not yet accept key-based authentication. 
