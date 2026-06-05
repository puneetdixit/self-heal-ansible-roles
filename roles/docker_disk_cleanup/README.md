# docker_disk_cleanup

Ansible role to resolve high Docker disk utilization by pruning unused Docker resources and managing oversized container logs.

## Description

This role checks the disk usage of the Docker data root directory and performs cleanup operations when usage exceeds a configurable threshold. It can prune stopped containers, dangling or all unused images, unused networks, unused volumes, build cache, and truncate large container log files.

## Requirements

- Docker installed and running on the target host.
- The Ansible controller user must have permission to run `docker` commands (typically root or a member of the `docker` group).

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `docker_data_root` | `/var/lib/docker` | Path used for disk usage checks. |
| `docker_disk_threshold` | `80` | Disk usage percent threshold to trigger cleanup. |
| `docker_force_cleanup` | `false` | Run cleanup regardless of threshold. |
| `docker_prune_containers` | `true` | Prune stopped containers. |
| `docker_prune_images` | `true` | Prune images. |
| `docker_prune_networks` | `true` | Prune unused networks. |
| `docker_prune_volumes` | `false` | Prune unused volumes (data loss risk). |
| `docker_prune_build_cache` | `true` | Prune build cache. |
| `docker_prune_all_unused_images` | `false` | Remove all unused images, not just dangling. |
| `docker_image_prune_until` | `168h` | Only prune images older than this. |
| `docker_buildcache_prune_until` | `168h` | Only prune build cache older than this. |
| `docker_truncate_logs` | `true` | Truncate large container log files. |
| `docker_max_log_size_mb` | `100` | Max log size in MB before truncation. |

## Example Playbook

```yaml
- hosts: docker_hosts
  become: true
  roles:
    - role: docker_disk_cleanup
      vars:
        docker_disk_threshold: 75
        docker_prune_volumes: true
        docker_force_cleanup: false
```

## Warnings

- Enabling `docker_prune_volumes` can permanently delete data in unused volumes.
- Enabling `docker_prune_all_unused_images` removes all images not used by a container.

## License

MIT
