# Ansible Role: apt_3rdparty_repo

This role adds one or more third-party APT repositories and securely installs their GPG keys on Debian-based systems.

## Role variables

### Required

| Variable             | Description                                                                                    | Default     |
|----------------------|------------------------------------------------------------------------------------------------|-------------|
| `apt_3rdparty_repos` | **List of dicts**: each dict must define `repo`, `filename`, `key_url`, `key_file` (see below) | _required_  |

Each item in `apt_3rdparty_repos` must contain:

- `repo` – The full `deb` line to add (including `signed-by=...` if used).
- `filename` – The name for the source list file (e.g., `helm-stable-debian`).
- `key_url` – URL to download the GPG signing key.
- `key_file` – Filename for the dearmored key in `/etc/apt/keyrings/`.
- `name` – _(optional)_ A friendly label for the repo (for logs/readability).

### Optional

| Variable                         | Description                             | Default               |
|----------------------------------|-----------------------------------------|-----------------------|
| `apt_3rdparty_repo_keyring_dir`  | Directory to store dearmored GPG keys   | `/etc/apt/keyrings`   |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.apt_3rdparty_repo
      vars:
        repos:
          - name: "helm"
            repo: "deb [arch=arm64 signed-by=/etc/apt/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main"
            filename: "helm-stable-debian"
            key_url: "https://baltocdn.com/helm/signing.asc"
            key_file: "helm.gpg"
          - name: "kubernetes"
            repo: "deb [arch=arm64 signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /"
            filename: "kubernetes-stable-v1.33"
            key_url: "https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key"
            key_file: "kubernetes-apt-keyring.gpg"
```
