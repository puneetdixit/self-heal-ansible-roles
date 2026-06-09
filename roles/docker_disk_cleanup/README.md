# docker_disk_cleanup

Ansible role to resolve high Docker disk utilization by pruning unused Docker objects.

## Description

This role inspects the disk usage of the Docker data directory and, when usage meets or exceeds a configurable threshold (or when forced), prunes unused containers, images, networks, volumes, and build cache to reclaim disk space.

## Requirements

- Docker installed and running on the target host.
- The Ansible user must have permission to run `docker` commands (root or member of the `docker` group).

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `docker_cleanup_threshold_pct` | `80` | Disk usage percentage at or above which cleanup runs. |
| `docker_force_cleanup` | `false` | Run cleanup regardless of current disk usage. |
| `docker_prune_containers` | `true` | Prune stopped containers. |
| `docker_prune_images` | `true` | Prune dangling images. |
| `docker_prune_networks` | `true` | Prune unused networks. |
| `docker_prune_volumes` | `false` | Prune unused volumes (use with caution). |
| `docker_prune_build_cache` | `true` | Prune build cache. |
| `docker_prune_all_images` | `false` | Prune all unused images, not only dangling. |
| `docker_prune_until` | `""` | Only remove objects older than this duration (e.g. `24h`). |

## Example Playbook

```yaml
- hosts: docker_hosts
  become: true
  roles:
    - role: docker_disk_cleanup
      vars:
        docker_cleanup_threshold_pct: 75
        docker_prune_all_images: true
        docker_prune_until: "168h"
```

## Safety Notes

- Volume pruning is disabled by default because it can delete persistent data.
- Use `docker_prune_until` to retain recently created objects.

## License

MIT
