# Watchtower — Docker Compose

Automatic Docker container updates with optional scheduling and label-based control.

## Prerequisites
- Docker and Docker Compose v2
- Access to the Docker socket (mounted read-only is fine for Watchtower)

## How It Works
- Checks for new images on a schedule (`WATCHTOWER_SCHEDULE` in cron format)
- Updates containers and restarts them with minimal downtime
- Respects `WATCHTOWER_LABEL_ENABLE=true` to only update labeled containers

## Configuration
- Schedule: `WATCHTOWER_SCHEDULE=0 0 4 * * *` (daily at 04:00)
- Cleanup: `WATCHTOWER_CLEANUP=true` removes old images after updates
- Rolling restart: `WATCHTOWER_ROLLING_RESTART=true`
- Scope: with `WATCHTOWER_LABEL_ENABLE=true`, add this label to services you want auto-updated:
  ```yaml
  labels:
    com.centurylinklabs.watchtower.enable: "true"
  ```

## Usage
Start Watchtower:
```bash
docker compose up -d
```
Run once immediately (ignoring schedule):
```bash
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower --run-once
```

## Notifications (optional)
Add environment variables for your preferred notifier, e.g.:
- Slack: `WATCHTOWER_NOTIFICATION_URL=slack://tokenA/tokenB/tokenC`
- Gotify: `WATCHTOWER_NOTIFICATION_URL=gotify://token@host/title`
- Email and others are supported; see Watchtower docs.

## Security
- Watchtower requires Docker socket access; restrict who can access the host.
- Consider pinning image tags (e.g., `:x.y.z`) for controlled upgrades.

## Troubleshooting
- Check logs: `docker compose logs -f watchtower`
- Validate cron expression and timezone
- Ensure target services include the watchtower label when label mode is enabled