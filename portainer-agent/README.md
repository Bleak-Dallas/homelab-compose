# Portainer Agent — Docker Compose

Lightweight agent that allows a remote Portainer server to manage this Docker host securely.

## Prerequisites
- Docker and Docker Compose v2
- A reachable Portainer server (CE or Business) to connect from

## What It Does
- Exposes TCP `9001` for the Portainer server to connect
- Mounts the Docker socket and volumes for management and stack deployments

## Usage
Start the agent:
```bash
docker compose up -d
```
On your Portainer server UI:
- Add Environment → Agent
- Set the agent endpoint address to `tcp://<host-ip>:9001`
- Finish to connect and start managing this node

## Configuration
- Ports: `9001:9001`
- Volumes:
  - `/var/run/docker.sock:/var/run/docker.sock`
  - `/var/lib/docker/volumes:/var/lib/docker/volumes`
- Auto-update: Watchtower label included

## Security
- Restrict access to port 9001 to only your Portainer server (firewall)
- Keep image tags pinned if you need controlled upgrades
- Socket access grants full Docker control; secure the host

## Maintenance
- Update:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Logs:
  ```bash
  docker compose logs -f agent
  ```