# Role: sshd_hardening

This Ansible role applies a hardened configuration to the OpenSSH daemon.

> ⚠️ **Note**: This role assumes the OpenSSH server is already installed. It does **not** install or manage the `sshd` package itself.

## Role variables

### Optionals

| Name                                     | Description                                        | Default                            |
|------------------------------------------|----------------------------------------------------|------------------------------------|
| `sshd_hardening_config_path`             | Path to the rendered SSHd config                   | `/etc/ssh/sshd_config.d/00-hardening.conf` |
| `sshd_hardening_port`                    | SSH listening port                                 | `22`                               |
| `sshd_hardening_permit_root_login`       | Whether to allow root login                        | `"no"`                             |
| `sshd_hardening_permit_empty_passwords`  | Disallow accounts with empty passwords             | `"no"`                             |
| `sshd_hardening_password_authentication` | Disable password-based login                       | `"no"`                             |
| `sshd_hardening_pubkey_authentication`   | Allow public key authentication                    | `"yes"`                            |
| `sshd_hardening_kbd_interactive_authentication` | Allow keyboard-interactive authentication   | `"no"`                             |
| `sshd_hardening_hostbased_authentication` | Allow host-based authentication                   | `"no"`                             |
| `sshd_hardening_login_grace_time`        | Time before login session times out (seconds)      | `60`                               |
| `sshd_hardening_client_alive_interval`   | Interval for client keepalive (seconds)            | `120`                              |
| `sshd_hardening_max_auth_tries`          | Maximum authentication attempts                    | `3`                                |
| `sshd_hardening_x11_forwarding`          | Disable X11 forwarding                             | `"no"`                             |
| `sshd_hardening_allow_tcp_forwarding`    | Allow TCP forwarding                               | `"no"`                             |
| `sshd_hardening_allow_agent_forwarding`  | Allow ssh-agent forwarding                         | `"no"`                             |
| `sshd_hardening_permit_tunnel`           | Allow tun(4) device forwarding                     | `"no"`                             |
| `sshd_hardening_permit_user_environment` | Allow users to set environment options             | `"no"`                             |
| `sshd_hardening_allowed_users`           | List of explicitly allowed SSH users (empty = all) | `[]`                               |
| `sshd_hardening_sftp_enabled`            | Whether to enable the sFTP subsystem               | `false`                            |
| `sshd_hardening_lockout_check`           | Abort if password logins are disabled and no `authorized_keys` file exists under `/root` or `/home` | `true` |
| `sshd_hardening_service_name`            | Service to restart                                 | `ssh` on Debian family, else `sshd` |

## Safety checks

The role aborts, before changing anything, when:

- `/etc/ssh/sshd_config` does not `Include` the directory of `sshd_hardening_config_path` (the drop-in would be ignored).
- `PasswordAuthentication` is `"no"` and no non-empty `authorized_keys` was found (see `sshd_hardening_lockout_check`; custom `AuthorizedKeysFile` locations are not detected).

The rendered file is validated with `sshd -t -f` before it replaces the current one, and a leftover `sshd.conf` from earlier versions of the role is removed.

The drop-in is named `00-hardening.conf` because sshd uses the first value it finds, so it has to be read before files such as `50-cloud-init.conf`.

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
