# Paperless-ngx — Docker Compose (Postgres)

Self-hosted Paperless-ngx with Redis and PostgreSQL. Includes sane defaults, bind mounts for persistence, and healthchecks.

## Prerequisites
- Docker and Docker Compose v2
- Storage paths available under `/srv/paperless` (or adjust paths)
- A strong `POSTGRES_PASSWORD` and `PAPERLESS_ADMIN_PASSWORD`

## Configuration
- Edit `paperless/docker-compose.env` (copy from `docker-compose.env.example` if needed):
  - `POSTGRES_PASSWORD`: DB password used by Postgres and Paperless
  - `PAPERLESS_ADMIN_PASSWORD`: initial admin password (user: `admin`)
  - `PAPERLESS_SECRET_KEY`: Django secret key
  - `PAPERLESS_URL`: external URL (e.g., `https://paperless.example.com`)
  - Other optional envs per comments in `.env.example`
- Volumes (host -> container):
  - `/srv/paperless/data` -> `/usr/src/paperless/data`
  - `/srv/paperless/media` -> `/usr/src/paperless/media`
  - `/srv/paperless/consume` -> `/usr/src/paperless/consume`
  - `/srv/paperless/export` -> `/usr/src/paperless/export`

## Usage
1. Populate `docker-compose.env`.
2. Pull images:
   ```bash
   docker compose pull
   ```
3. Start services:
   ```bash
   docker compose up -d
   ```
4. First login at `http://<host>:8010` with `admin` / `PAPERLESS_ADMIN_PASSWORD`.

## Backup & Restore
- Database (Postgres):
  ```bash
  docker exec -t $(docker compose ps -q db) pg_dump -U paperless paperless > paperless_$(date +%F).sql
  ```
- App data: back up `/srv/paperless/data` and `/srv/paperless/media`.
- To restore, import the SQL dump and restore the data directories.

## Maintenance
- Update:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Logs:
  ```bash
  docker compose logs -f webserver
  ```

## Troubleshooting
- Ensure `PAPERLESS_URL` uses the correct external hostname/URL.
- Verify Redis and Postgres are healthy (compose healthchecks).
- If OCR fails, check `PAPERLESS_OCR_LANGUAGE` and install language packs if needed.

## Security
- Keep `docker-compose.env` out of version control; commit only `docker-compose.env.example`.
- Use strong unique passwords and rotate them periodically.
- Restrict access to the `consume` folder if shared on the network.