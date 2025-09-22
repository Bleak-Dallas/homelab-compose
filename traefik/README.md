# Traefik — Docker Compose (Cloudflare DNS challenge)

Reverse proxy with automatic TLS via Cloudflare and a protected dashboard.

## Prerequisites
- Docker and Docker Compose v2
- Cloudflare zone for your domain and an API token with DNS edit
- External Docker network `proxy` shared with routed services

## Files
- `.env` (copy from `.env.example`):
  - `CF_API_EMAIL` — Cloudflare account email (if needed for your token)
  - `CF_DNS_API_TOKEN` — API token with DNS:Edit for the zone
- `data/traefik.yml` — static config (entrypoints, providers, certresolver)
- `data/config.yml` — dynamic config (middlewares, extra routers if desired)
- `data/acme.json` — certificate store (must exist with `chmod 600` permissions on Linux)

## Ports
- `80` — HTTP (redirects to HTTPS)
- `443` — HTTPS

## Usage
1. Populate `.env` with Cloudflare credentials.
2. Ensure `data/acme.json` exists and is writable by Traefik.
3. Start Traefik:
   ```bash
   docker compose up -d
   ```
4. Access dashboard:
   - `https://traefik-dashboard.<your-domain>` (basic auth enabled via labels)

## Networking
- Traefik runs on external network `proxy`. Attach your app services to the same network and add labels, for example:
  ```yaml
  labels:
    - traefik.enable=true
    - traefik.http.routers.app.rule=Host(`app.<your-domain>`)
    - traefik.http.routers.app.entrypoints=https
    - traefik.http.routers.app.tls=true
    - traefik.http.routers.app.tls.certresolver=cloudflare
    - traefik.http.services.app.loadbalancer.server.port=8080
  networks:
    - proxy
  ```

## Dashboard Security
- Basic auth is configured via `traefik.http.middlewares.traefik-auth.basicauth.users` label.
- Generate new credentials:
  ```bash
  htpasswd -nb <user> <password>
  ```
  Escape `$` as `$$` in compose labels.

## Certificates
- Uses DNS-01 challenge with Cloudflare (`cloudflare` certresolver).
- Wildcard example in labels covers `*.your-domain` and base domain.

## Maintenance
- Update images:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Logs:
  ```bash
  docker compose logs -f traefik
  ```

## Troubleshooting
- Verify Cloudflare token scope (Zone:DNS:Edit) and correct zone.
- Ensure `proxy` network exists and apps are attached to it.
- Check that your domain DNS records point to this host.
- Review `data/traefik.yml` for entrypoints/providers misconfigurations.