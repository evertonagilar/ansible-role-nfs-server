# Ansible Role: NFS Server (evertonagilar.nfs-server)

![Ansible Galaxy](https://img.shields.io/badge/Ansible--Galaxy-nfs--server-blue?style=flat-square)

## Description

This Ansible role installs and configures a **NFS server** on Debian/Ubuntu hosts.  
It sets up the required packages, creates export directories with customizable permissions and ACLs, and manages the `/etc/exports` configuration.  
Ideal for sharing directories over the network with fine-grained access control.

---

## Features

- Installs NFS server packages (`nfs-kernel-server`, `nfs-common`)
- Creates export directories with configurable owner, group, permissions, and ACLs
- Manages `/etc/exports` entries dynamically from a variable list
- Supports ACL configuration for fine-grained user/group permissions
- Creates a default group `nfsusers` for shared access (configurable)
- Idempotent and easy to customize for production or development environments

---

## Requirements

- Target hosts running Debian/Ubuntu-based Linux distributions
- Filesystem mounted with ACL support enabled
- `acl` package installed (the role can install it for you)
- Proper network setup for NFS clients and server communication

---

## Supported Platforms

- Ubuntu 18.04+
- Debian 10+

---

## Role Variables

| Variable            | Description                                   | Default                       |
|---------------------|-----------------------------------------------|-------------------------------|
| `nfs_exports`       | List of exports with path, clients, options   | `[]` (empty list)             |
| `nfs_export_dir_mode` | Default permissions mode for export directories | `'2775'`                      |
| `nfs_export_dir_owner` | Default owner for export directories           | `'root'`                     |
| `nfs_export_dir_group` | Default group for export directories           | `'nfsusers'` (created by role) |
| Each export item in `nfs_exports` can include:  
`path`, `user`, `group`, `mode`, `options`, `clients` (list or string) |

---

## Example `nfs_exports` variable

```yaml
nfs_exports:
  - path: /srv/public
    user: root
    group: nfsusers
    mode: '2775'
    options: rw,sync,no_subtree_check,no_root_squash
    clients:
      - cidr: "192.168.1.*"
        options: rw,sync,no_subtree_check
      - cidr: "192.168.25.*"
        options: rw,no_subtree_check
  - path: /srv/private
    clients: "*"
    options: rw,sync,no_subtree_check
