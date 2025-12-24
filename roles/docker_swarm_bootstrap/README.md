# Ansible Role: docker_swarm_bootstrap

This role bootstraps a single-node Docker Swarm cluster by initializing a manager node. It sets up a Python venv for the Docker SDK, validates prerequisites, and initializes the swarm with dual-stack defaults.
The swarm is configured for dual-stack networking by default (IPv4 + IPv6) by listening on the default network interface and advertising the default IPv6 address when available.

## Role Variables

### Optional

| Variable | Description | Default |
|----------|-------------|---------|
| `docker_swarm_bootstrap_base_path` | Base directory for bootstrap assets (venv, etc.) | `/opt/docker_swarm_bootstrap` |
| `docker_swarm_bootstrap_user` | Owner user for created files and venv | `root` |
| `docker_swarm_bootstrap_group` | Owner group for created files and venv | `root` |
| `docker_swarm_bootstrap_venv_path` | Python venv path for Docker SDK | `{{ docker_swarm_bootstrap_base_path }}/venv` |
| `docker_swarm_bootstrap_python_venv_package` | OS package for Python venv support | `python3-venv` |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.docker_swarm_bootstrap
      vars:
        docker_swarm_bootstrap_user: "swarm-user"
        docker_swarm_bootstrap_group: "swarm-user"
```

Avoid using `root` as the bootstrap user unless you have a specific need for it.

## Limitations

- Single-node swarm only (manager init only, no join of additional nodes).
- Docker must already be installed on the target hosts. Consider using `geerlingguy.docker` to install Docker before running this role.
- The bootstrap user is not created by this role and must already exist on the target host.
