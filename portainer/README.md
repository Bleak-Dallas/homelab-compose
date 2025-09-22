# Portainer CE — Docker Compose

Self-hosted Portainer Community Edition with HTTPS and Traefik support.

## Prerequisites
- Docker and Docker Compose v2
- Traefik stack with an external network named `proxy` (if using labels)
- DNS record for your Portainer host (e.g., `portainer.example.com`)

## Configuration
- Ports: `9000` (HTTP), `9443` (HTTPS)
- Data persistence: bind mount to `/home/serveradmin/portainer/data`
- Traefik labels route `https://portainer.<domain>` to internal `9443` over HTTPS
- Timezone via `TZ` env var

## Usage
Start Portainer:
```bash
docker compose up -d
```
Access:
- Direct: `https://<host>:9443` (or `http://<host>:9000`)
- Via Traefik: `https://portainer.<your-domain>`

First run:
- Create the admin account when prompted.
- Connect to the local Docker environment (via the mounted socket).

## Maintenance
- Update:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Logs:
  ```bash
  docker compose logs -f portainer
  ```
- Backup: back up `/home/serveradmin/portainer/data` regularly.

## Security
- Prefer HTTPS (port 9443) or put Portainer behind Traefik with TLS.
- Keep the Docker socket mount read-only (already set).
- Limit access to the Portainer URL in your firewall/reverse proxy.

## Troubleshooting
- Ensure the `proxy` network exists and Traefik is attached to it.
- Confirm the Traefik certresolver name matches your Traefik config.
- If UI is unreachable via domain, check DNS and Traefik router rule.