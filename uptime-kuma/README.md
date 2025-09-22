# Uptime Kuma — Docker Compose

Self-hosted monitoring and status page with Traefik routing and Docker socket integration.

## Prerequisites
- Docker and Docker Compose v2
- External Docker network `proxy` (shared with Traefik)
- DNS record for your Kuma host (if exposing via Traefik)

## Configuration
- Ports: `3001` exposed for direct access
- Data persistence: `/home/serveradmin/uptime-kuma/data` -> `/app/data`
- Docker socket mount enables Docker host monitoring (optional)
- Traefik labels route `https://uptime-kuma.<domain>` to internal port `3001`

## Usage
Start Uptime Kuma:
```bash
docker compose up -d
```
Access:
- Direct: `http://<host>:3001`
- Via Traefik: `https://uptime-kuma.<your-domain>`

First run:
- Create the admin account in the web UI.
- Add monitors (HTTP/TCP/UDP/DNS/Ping, Docker containers, etc.).

## Backup & Restore
- Back up the data directory: `/home/serveradmin/uptime-kuma/data`
- To restore, stop the container, restore the folder, then start again.

## Maintenance
- Update:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Logs:
  ```bash
  docker compose logs -f uptime-kuma
  ```

## Security
- Consider removing the Docker socket mount if not needed.
- Protect the Traefik route with auth middleware if exposing publicly.

## Troubleshooting
- Ensure `proxy` network exists and matches `traefik.docker.network` label.
- Verify DNS and certificate resolver settings in Traefik.
- Check file permissions on the data directory if persistence fails.