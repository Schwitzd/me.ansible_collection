# Role: sshd_hardening

This Ansible role applies a hardened configuration to the OpenSSH daemon based on security best practices.

> ⚠️ **Note**: This role assumes the OpenSSH server is already installed. It does **not** install or manage the `sshd` package itself.

## Role variables

### Optionals

| Name                                     | Description                                        | Default                            |
|------------------------------------------|----------------------------------------------------|------------------------------------|
| `sshd_hardening_config_path`             | Path to the rendered SSHd config                   | `/etc/ssh/sshd_config.d/sshd.conf` |
| `sshd_hardening_port`                    | SSH listening port                                 | `22`                               |
| `sshd_hardening_permit_root_login`       | Whether to allow root login                        | `"no"`                             |
| `sshd_hardening_permit_empty_passwords`  | Disallow accounts with empty passwords             | `"no"`                             |
| `sshd_hardening_password_authentication` | Disable password-based login                       | `"no"`                             |
| `sshd_hardening_protocol`                | SSH protocol version                               | `2`                                |
| `sshd_hardening_login_grace_time`        | Time before login session times out (seconds)      | `60`                               |
| `sshd_hardening_client_alive_interval`   | Interval for client keepalive (seconds)            | `120`                              |
| `sshd_hardening_max_auth_tries`          | Maximum authentication attempts                    | `3`                                |
| `sshd_hardening_x11_forwarding`          | Disable X11 forwarding                             | `"no"`                             |
| `sshd_hardening_allowed_users`           | List of explicitly allowed SSH users (empty = all) | `[]`                               |
| `sshd_hardening_sftp_enabled`            | Whether to enable the sFTP subsystem               | `false`                            |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.sshd_hardening
      vars:
        sshd_hardening_allowed_users:
          - "admin"
        sshd_hardening_port: 2222
```
