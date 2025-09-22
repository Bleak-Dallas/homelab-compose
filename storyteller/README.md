# Storyteller — Docker Compose

Self-hosted Storyteller container with secret management via Docker secrets.

## Prerequisites
- Docker and Docker Compose v2
- (Optional) NVIDIA GPU drivers + container runtime if you plan to use GPU (`runtime: nvidia`)

## Files
- `docker-compose.yaml` — service definition (port 8001, data volume, secret)
- `STORYTELLER_SECRET_KEY.txt` — real secret file (ignored by git)
- `STORYTELLER_SECRET_KEY.example.txt` — example template with placeholder

## Secret Setup
1. Copy the example file and set a strong secret:
   ```bash
   cp STORYTELLER_SECRET_KEY.example.txt STORYTELLER_SECRET_KEY.txt
   # Edit and set a 64+ char random value
   ```
2. Compose uses it as a Docker secret named `secret_key`, mounted at `/run/secrets/secret_key`.
3. The container reads it via env `STORYTELLER_SECRET_KEY_FILE=/run/secrets/secret_key`.

## Volumes
- `./data` (host) -> `/data` (container) for app data persistence

## Ports
- `8001:8001` — access the web UI at `http://<host>:8001`

## Usage
Start Storyteller:
```bash
docker compose up -d
```
Check logs:
```bash
docker compose logs -f web
```

## GPU Support
- The compose file sets `runtime: nvidia`. Ensure:
  - NVIDIA drivers installed on host
  - NVIDIA Container Toolkit installed and configured
  - If not using GPU, remove `runtime: nvidia` from the service

## Maintenance
- Update the image:
  ```bash
  docker compose pull && docker compose up -d
  ```
- Back up `./data` and keep your secret safe and off backups if required by policy.

## Security
- `STORYTELLER_SECRET_KEY.txt` is git-ignored; keep it private.
- Generate a long random secret. Example Linux/WSL:
  ```bash
  openssl rand -base64 64 | tr -d '\n'
  ```
  PowerShell:
  ```powershell
  [Convert]::ToBase64String((New-Object byte[] 64 | %{$_=0}; (New-Object System.Security.Cryptography.RNGCryptoServiceProvider).GetBytes($_); $_))
  ```
- Restrict access to port 8001 if exposed beyond your LAN.