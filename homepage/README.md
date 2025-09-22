# Homepage Stack

This directory contains the docker compose and configuration for the GetHomepage dashboard (https://gethomepage.dev).

## Files
- docker-compose.yaml — Compose definition for the homepage service, Traefik labels, and environment hooks.
- config/ — Homepage configuration (services, widgets, icons).
- .env — Private environment variables (NOT committed).
- .env.example — Template with placeholders to help set up your own .env safely.

## Quick start
1. Copy the example env and edit values:
   - cp .env.example .env (Windows: Copy-Item .env.example .env)
   - Fill URLs and credentials in .env (keep it private).
2. Create config folders (if not present):
   - mkdir config icons (Windows: New-Item -Type Directory config,icons)
3. Start the stack:
   - docker compose up -d from this folder.

## Security notes
- Do not commit .env. The repo .gitignore ignores all *.env files.
- Credentials and API tokens live only in .env and are referenced by config using {{HOMEPAGE_VAR_*}} placeholders.
- The compose file exposes port 3000. If you use Traefik on the proxy network, consider removing the host port mapping and accessing via Traefik only.
- Avoid enabling the commented Docker socket mount unless you fully understand the risks.

## Customization
- Services and widgets are defined in config/services.yaml. Prefer env vars for URLs and descriptions to avoid leaking internal details.
- Place custom icons in icons/ and reference them from config.

## Maintenance
- Pin image versions instead of :latest to avoid surprise upgrades.
- Optionally enable Watchtower only for selected services (label present) and review updates before rollout.

## Troubleshooting
- Check container logs: docker compose logs -f homepage
- Validate config: compare with docs at https://gethomepage.dev/latest/
