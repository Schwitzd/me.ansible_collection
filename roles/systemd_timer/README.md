# Ansible Role: systemd_timer

This role manages a systemd timer/service pair with full support for all major scheduling options, user/group execution, pre/post commands, and timer persistence.

The role supports both **regular services** (`name.service`) and **templated services** (`name@.service`).  
A templated service allows you to pass an *instance name* (e.g. `backup@daily.service`) that expands inside the service unit as `%i`.

> [!WARNING]
> The timer unit is **always non-templated** (`name.timer`). If you use a templated service (`name@.service`), the timer can only target **one instance** (e.g. `name@daily.service`), not multiple.

## Role Variables

### Required

| Variable                | Description                       |
|-------------------------|-----------------------------------|
| `systemd_timer_name`    | Name for the service/timer units  |
| `systemd_timer_command` | Command to execute in the service |

### Optional

| Variable                             | Description                                                                           | Default |
|--------------------------------------|---------------------------------------------------------------------------------------|---------|
| `systemd_timer_instance`             | If set, create a templated service and bind the timer to the given instance. Leave unset for a regular service. | unset   |
| `systemd_timer_on_calendar`          | Schedule using systemd's calendar syntax (e.g., `"Mon *-*-* 02:00:00"` or `"daily"`). | unset   |
| `systemd_timer_on_active_sec`        | Time after last activation to run (e.g., `"10min"`).                                  | unset   |
| `systemd_timer_on_boot_sec`          | Time after system boot to run (e.g., `"5min"`).                                       | unset   |
| `systemd_timer_on_startup_sec`       | Time after systemd manager startup to run (useful in containers or user units).       | unset   |
| `systemd_timer_on_unit_active_sec`   | Time after associated service last ran successfully.                                  | unset   |
| `systemd_timer_on_unit_inactive_sec` | Time after associated service last finished (any state).                              | unset   |
| `systemd_timer_persistent`           | If `true`, missed `OnCalendar` events while the system was off will be run immediately at next boot; if `false`, missed runs are skipped. Only applies with `OnCalendar`. | false   |
| `systemd_timer_user`                 | Run the service as this user.                                                         | root    |
| `systemd_timer_group`                | Run the service as this group.                                                        | root    |
| `systemd_timer_exec_start_pre`       | Command (string) to run as `ExecStartPre` before the main command.                    | unset   |
| `systemd_timer_exec_start_post`      | Command (string) to run as `ExecStartPost` after the main command.                    | unset   |

## Example usage

### Non-templated service

```yaml
- hosts: all
  become: true
  roles:
    - role: schwitzd.collection.systemd_timer
      vars:
        systemd_timer_name: "backup-job"
        systemd_timer_command: "/usr/local/bin/backup.sh --full"
        systemd_timer_on_calendar: "daily"
        systemd_timer_user: "backupuser"
        systemd_timer_group: "backupuser"
        systemd_timer_persistent: true
```

### Templated service

```yaml
- hosts: all
  become: true
  roles:
    - role: schwitzd.collection.systemd_timer
      vars:
        systemd_timer_name: "backup"
        systemd_timer_instance: "daily"
        systemd_timer_command: "/usr/local/bin/backup.sh --target=%i"
        systemd_timer_on_calendar: "Mon *-*-* 02:00:00"
```

This creates:

- `/etc/systemd/system/backup@.service`
- `/etc/systemd/system/backup.timer` (bound to `backup@daily.service`)
