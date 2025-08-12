# Ansible Role: MinIO in Docker (evertonagilar.minio-docker)

![Ansible Galaxy](https://img.shields.io/badge/Ansible--Galaxy-minio-blue?style=flat-square)

## Description

This Ansible role installs and configures a **MinIO server** inside a Docker container.  
It can automatically create a list of buckets on first startup, and works well alongside the [`evertonagilar.docker-tls`](https://galaxy.ansible.com/evertonagilar/docker-tls) role to enable secure Docker setups.

---

## Features

- Install MinIO server in a Docker container
- Automatically create a list of buckets
- Customizable ports, directories, image, and credentials

---

## Requirements

- Docker must be installed on the target host.
- You may use [`evertonagilar.docker-tls`](https://galaxy.ansible.com/evertonagilar/docker-tls) to install and configure Docker with TLS support.

---

## Supported Platforms

- Ubuntu 20.04+
- Debian 11+

---

## Role Variables

| Variable               | Description                        | Default                 |
|------------------------|------------------------------------|-------------------------|
| `minio_user`           | MinIO root user                    | `minioadmin`            |
| `minio_password`       | MinIO root password                | `minioadmin`            |
| `minio_app_dir`        | MinIO configuration directory      | `/opt/minio`            |
| `minio_data_dir`       | MinIO data directory               | `/data/minio`           |
| `minio_container_name` | Name of the MinIO container        | `minio`                 |
| `minio_image`          | Docker image for MinIO             | `minio/minio:latest`    |
| `minion_restart`       | Docker restart policy              | `unless-stopped`        |
| `minio_api_port`       | MinIO API port                     | `9000`                  |
| `minio_console_port`   | MinIO Console port                 | `9001`                  |
| `bucket_list`          | List of buckets to create          | `[]`                    |

---

## Example Usage

```yaml
- name: Deploy MinIO with Docker
  hosts: all
  become: true
  roles:
    - role: evertonagilar.docker-tls  # Optional: install Docker with TLS
    - role: evertonagilar.minio-docker
      vars:
        minio_user: minioadmin
        minio_password: minioadmin
        minio_image: minio/minio:latest
        bucket_list:
          - mybucket
          - mybucket2
```
