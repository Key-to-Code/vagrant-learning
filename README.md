# Terramino Demo App — Vagrant Setup

> **Attribution:** This code is based on the [HashiCorp Education](https://github.com/hashicorp-education) curriculum. The Terramino application source can be found at [hashicorp-education/terramino-go](https://github.com/hashicorp-education/terramino-go).

This repository contains a Vagrant-based local development environment for the [Terramino](https://github.com/hashicorp-education/terramino-go) demo app — a containerized Go application managed with Docker Compose.

---

## Prerequisites

Before getting started, make sure you have the following installed on your host machine:

- [Vagrant](https://www.vagrantup.com/downloads)
- A supported hypervisor (e.g. [VirtualBox](https://www.virtualbox.org/))

---

## Getting Started

### 1. Start the VM

```bash
vagrant up
```

This will:

- Provision an Ubuntu 24.04 VM using the `hashicorp-education/ubuntu-24-04` box
- Install Docker and all required dependencies
- Clone the `terramino-go` repository (on the `containerized` branch)
- Start the Terramino app via Docker Compose

### 2. SSH into the VM

```bash
vagrant ssh
```

---

## Available Commands (inside the VM)

Once SSH'd in, the following shell aliases are available:

| Alias | Description |
|-------|-------------|
| `play` | Attach to the Terramino CLI via the running backend container |
| `reload` | Rebuild and restart all Docker containers (for backend changes) |

---

## Vagrant Provisioners

The `Vagrantfile` defines several named provisioners that can be run on demand:

| Provisioner | Description | Run on `vagrant up`? |
|-------------|-------------|----------------------|
| `install-dependencies` | Installs Docker and clones the repo | Yes |
| `start-terramino` | Starts the app with `docker compose up -d` | Yes |
| `restart-terramino` | Restarts containers without rebuilding (good for frontend changes) | No |
| `reload-terramino` | Rebuilds and restarts containers (good for backend changes) | No |

To run a specific provisioner manually:

```bash
# Restart containers (no rebuild)
vagrant provision --provision-with restart-terramino

# Full rebuild and restart
vagrant provision --provision-with reload-terramino
```

---

## Project Structure

```
.
├── Vagrantfile                  # VM configuration and provisioning steps
└── install-dependencies.sh     # Dependency installation script
```

---

## Notes

- The Terramino app source is cloned into `/home/vagrant/terramino-go` inside the VM on the `containerized` branch.
- Docker is configured to run as the `vagrant` user (no `sudo` needed for Docker commands).
- Use `reload` (full rebuild) when making backend/Go changes, and `vagrant provision --provision-with restart-terramino` for lightweight frontend-only changes.
