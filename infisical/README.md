# Infisical (Self-Hosted) — Docker Compose

This folder contains a minimal, production-ready Docker Compose setup to self-host Infisical along with Postgres and Redis.

## Prerequisites
- Docker and Docker Compose v2
- A reverse proxy/ingress (e.g., Traefik) if using custom domains
- DNS entry for `INFISICAL_HOST`

## Configuration
- Copy `.env.example` to `.env` and update values:
  - `INFISICAL_HOST`, `SITE_URL`, `NEXTAUTH_URL`: external URL
  - `ENCRYPTION_KEY`: 32+ char secret for crypto
  - `AUTH_SECRET`: random base64 secret for NextAuth
  - `POSTGRES_*`: DB name/user/password and data path
  - `REDIS_URL`: connection string
  - `DB_CONNECTION_URI`: uses the `POSTGRES_*` vars by default
  - `PROXY_NETWORK`, `CERTRESOLVER`: match your proxy stack
  - `SMTP_*`: mail settings for invites/password resets

## Usage
1. Create and edit `.env`.
2. Start the stack:
   ```bash
   docker compose up -d
   ```
3. Access Infisical at `https://<INFISICAL_HOST>`.

## Data Persistence
- Postgres data volume/path comes from `POSTGRES_DATA` in `.env`.
- Back up DB and any external volumes regularly.

## SMTP Notes
- For Gmail, enable an App Password and use it for `SMTP_PASSWORD`.
- Set `SMTP_SECURE=true` for SMTPS (port 465) or keep `false` for STARTTLS (587).

## Maintenance
- Update images:
  ```bash
  docker compose pull && docker compose up -d
  ```
- View logs:
  ```bash
  docker compose logs -f
  ```

## Troubleshooting
- Ensure `DB_CONNECTION_URI` matches Postgres service name and credentials.
- Confirm the container network matches your reverse proxy network.
- Check Traefik labels in `docker-compose.yml` if routing fails.

## Security
- Keep secrets out of version control; only commit `.env.example`.
- Rotate `ENCRYPTION_KEY` and `AUTH_SECRET` if compromised.
