
# Laravel Production Deployment via Docker Compose

An enterprise-grade, highly dynamic Ansible role engineered to deploy containerized **Laravel Framework** applications safely and efficiently on remote production servers.

Developed and maintained by **Naif Saleh Alsebaeai** (DevOps Engineer specializing in scalable infrastructures).

## 🚀 Core Features

- **Dynamic Environment Handling**: Synchronizes production compose configurations and `.env` profiles on-the-fly.
- **Zero-Downtime Preparation**: Handles container lifecycle management, volume pruning, and orphan container sanitation before runtime.
- **Automated Pipeline Execution**: Safely orchestrates critical Laravel runtime optimizations:
  - Configuration cache clearing and rebuilding.
  - Production database migrations (`migrate --force`).
  - Initial database seeding (`db_seed --force`).
  - Production-ready view and route caching.
- **Robust Permission Enforcement**: Fixes stubborn Docker-to-Host storage and cache ownership issues using the isolated internal container identities (`devops:devops`).

## 🛠️ Role Variables

To make this role generic and reusable, all configuration parameters are externalized. You must define these variables in your main playbook or secret vault:

| Variable Name          | Description                                             | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----------------------------------- |
| `remote_deploy_dir`  | The destination directory on the target server.         | `/home/ubuntu`                     |
| `app_container_name` | The exact container name of the running Laravel app.    | `ticket-bus-app`                   |
| `local_compose_src`  | Local path on your management node to the compose file. | `/path/to/docker-compose-prod.yml` |
| `local_env_src`      | Local path to the production environment secrets file.  | `/path/to/.env.prod`               |
| `docker_hub_user`    | Your Docker Hub account username.                       | `naif775devops`                    |
| `docker_hub_token`   | Secure Docker Hub access token or personal token.       | `dckr_pat_xxxxxx`                  |

## 📦 Requirements

- Ansible version `2.10` or higher.
- Target server must have Docker Engine and the Docker Compose plugin already installed (You can use the companion `docker_setup` role to fulfill this requirement).

## 📋 Usage Example

After importing the role, implement it into your root orchestration playbook (`site.yml`) like so:

```yaml
---
- name: Production Deployment Architecture
  hosts: production_servers
  become: yes

  vars_files:
    - vars/secrets.yml # It is highly recommended to mask your tokens using Ansible Vault

  vars:
    remote_deploy_dir: "/home/ubuntu"
    app_container_name: "my-laravel-container"
    local_compose_src: "/home/devops/Desktop/projects/docker-compose.yml"
    local_env_src: "/home/devops/Desktop/projects/.env.prod"
    docker_hub_user: "naif775devops"
    docker_hub_token: "{{ vault_docker_hub_token }}"

  roles:
    - naif_alsebaeai.laravel_deploy
```

## 📄 License

This project is licensed under the MIT License.
