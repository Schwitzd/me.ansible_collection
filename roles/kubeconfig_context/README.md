# Role: kubeconfig_context

This role retrieves a kubeconfig from a target cluster, rewrites the Kubernetes API endpoint, and merges a renamed context, cluster, and user into the controller's local kubeconfig.
It replaces any existing entries with the same name, preserves unrelated contexts, clusters, and users, and can optionally set the current context and verify connectivity.

## Role variables

### Required

| Variable | Description |
|----------|-------------|
| `kube_context_name` | Final context name in the local kubeconfig. |
| `kube_api_fqdn` | FQDN used for the Kubernetes API endpoint. |

### Optional

| Variable | Description | Default |
|----------|-------------|---------|
| `kubeconfig_context_remote_kubeconfig_path` | Remote kubeconfig path on the target cluster. | `/etc/rancher/k3s/k3s.yaml` |
| `kubeconfig_context_local_kubeconfig_path` | Local kubeconfig path on the controller. | `~/.kube/config` |
| `kubeconfig_context_kube_api_port` | Kubernetes API port. | `6443` |
| `kubeconfig_context_set_current_context` | Set current context to `kube_context_name`. | `false` |
| `kubeconfig_context_backup_enabled` | Create a timestamped backup of the local kubeconfig. | `true` |
| `kubeconfig_context_verify_connectivity` | Verify the context with `kubectl get --raw=/readyz`. | `true` |
| `kubeconfig_context_kubectl_path` | Path to the kubectl binary. | `kubectl` |

## Example usage

```yaml
- hosts: k3s_controller
  roles:
    - role: schwitzd.collection.kubeconfig_context
      vars:
        kube_context_name: "lab-k3s"
        kube_api_fqdn: "k3s.lab.example.com"
```

### Set current context and disable connectivity check

```yaml
- hosts: k3s_controller
  roles:
    - role: schwitzd.collection.kubeconfig_context
      vars:
        kube_context_name: "prod-k3s"
        kube_api_fqdn: "k3s.prod.example.com"
        kubeconfig_context_set_current_context: true
        kubeconfig_context_verify_connectivity: false
```
