# homelab-compose-git

A collection of Docker Compose stacks for my homelab. Each service lives in its own folder with a `docker-compose.yml` (or `.yaml`) and a short README covering service-specific notes.

## Prerequisites
- Docker Engine and Docker Compose Plugin installed
- Suitable storage paths created on the host (as referenced by each compose file)
- Optional: Traefik reverse proxy (see `traefik/`) for routing and TLS

## Structure
- `homepage/` – Dashboard (compose + sample configs)
- `infisical/` – Secret management
- `iperf3/` – Network throughput testing
- `paperless/` – Paperless-ngx document management
- `plex/` – Plex Media Server
- `portainer/` – Portainer UI for Docker
- `portainer-agent/` – Portainer agent for remote management
- `storyteller/` – Storyteller service
- `traefik/` – Traefik reverse proxy (with example config)
- `uptime-kuma/` – Status and uptime monitoring
- `watchtower/` – Automatic container updates

## Quick Start
From a service directory, run:

```bash
cd <service>
docker compose up -d
```

- Review the folder README first for ports, volumes, and env vars.
- Some stacks require editing example files (e.g. `.env`, `*.example`).

## Networking and Routing
- If using Traefik, bring it up first: `cd traefik && docker compose up -d`.
- Service labels in their compose files can route through Traefik with TLS.
- See `traefik/data/traefik.yml.example` and `traefik/data/config.yml.example`.

## Environment Files
- Some services include example env files (e.g., `paperless/docker-compose.env.example`).
- Copy and adjust before starting: `cp example.env .env` (PowerShell: `Copy-Item`).

## Common Commands
- Start: `docker compose up -d`
- Stop: `docker compose down`
- Logs: `docker compose logs -f`
- Update images: `docker compose pull && docker compose up -d`

## Backups and Data
- Volumes and bind mounts are defined in each compose file.
- Ensure host paths exist and are backed up according to your needs.

## Notes
- This repository targets Windows hosts running Docker Desktop or Linux hosts; verify path syntax in binds for your platform.
- See each service folder’s README for specifics (ports, admin creds, storage paths).

## Services

- .git/
- homepage/ – [README](homepage\README.md) | [Compose](homepage\docker-compose.yaml)
- infisical/ – [README](infisical\README.md) | [Compose](infisical\docker-compose.yml)
- iperf3/ – [README](iperf3\README.md) | [Compose](iperf3\docker-compose.yml)
- paperless/ – [README](paperless\README.md) | [Compose](paperless\docker-compose.yml)
- plex/ – [README](plex\README.md) | [Compose](plex\docker-compose.yml)
- portainer/ – [README](portainer\README.md) | [Compose](portainer\docker-compose.yml)
- portainer-agent/ – [README](portainer-agent\README.md) | [Compose](portainer-agent\docker-compose.yml)
- storyteller/ – [README](storyteller\README.md) | [Compose](storyteller\docker-compose.yaml)
- traefik/ – [README](traefik\README.md) | [Compose](traefik\docker-compose.yml)
- uptime-kuma/ – [README](uptime-kuma\README.md) | [Compose](uptime-kuma\docker-compose.yml)
- watchtower/ – [README](watchtower\README.md) | [Compose](watchtower\docker-compose.yml)

