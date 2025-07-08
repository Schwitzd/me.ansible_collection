# Role: btrfs

This Ansible role provisions a Btrfs filesystem on a given block device, optionally mounts it, and can persist the mount in `/etc/fstab`.

> ⚠️ **Warning**: This role will create a single primary partition that spans the **entire disk** specified by `btrfs_device`. Any existing data or partitions on the disk will be destroyed.

## Role variables

### Required

| Name           | Description                                                                              |
|----------------|------------------------------------------------------------------------------------------|
| `btrfs_device` | The target block device (e.g. `/dev/sda`). This will be fully partitioned and formatted. |

### Optional

| Name                        | Description                                        | Default                                                                        |
|-----------------------------|----------------------------------------------------|--------------------------------------------------------------------------------|
| `btrfs_mount`               | Mount point                                        | `/mnt/storage`                                                                 |
| `btrfs_opts`                | Mount options                                      | `defaults,noatime,compress=zstd:3,ssd,discard=async,space_cache=v2,autodefrag` |
| `btrfs_do_mount`            | Whether to mount the partition                     | `true`                                                                         |
| `btrfs_persist_fstab`       | Whether to write to `/etc/fstab`                   | `true`                                                                         |
| `btrfs_partition_overwrite` | Whether to wipe the partition if it already exists | `false`                                                                        |

## Example usage

```yaml
- hosts: all
  roles:
    - role: schwitzd.collection.btrfs
      vars:
        btrfs_device: "/dev/sda"
        btrfs_persist_fstab: false
```
