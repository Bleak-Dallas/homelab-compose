Plex (Docker Compose)

Overview
- Runs Plex using the `lscr.io/linuxserver/plex` image with optional NVIDIA GPU acceleration.
- Compose file is parameterized via `.env` variables for easy customization.

Prerequisites
- Docker and Docker Compose (v2).
- Media directories created and mounted on the host.
- Optional: NVIDIA drivers and NVIDIA Container Toolkit if using GPU.

Files
- `docker-compose.yml` — service definition.
- `.env` — environment variables consumed by Compose (not committed).
- `.env.example` — template of required/optional variables.

Environment Variables
Copy `plex/.env.example` to `plex/.env` and adjust values.
- `PUID` / `PGID`: Host user/group IDs that should own Plex files.
- `TZ`: Timezone, e.g. `America/Denver`.
- `NVIDIA_VISIBLE_DEVICES`: GPU UUID or `all` (optional; requires NVIDIA stack).
- `ADVERTISE_IP`: URL that Plex advertises to clients, e.g. `http://10.42.42.14:32400/`.
- `ALLOWED_NETWORKS`: CIDR(s) allowed for LAN discovery, e.g. `10.42.42.0/24`.

Volumes (adjust to your host paths)
- `/home/<user>/plex/config:/config` — Plex config and metadata.
- `/mnt/plex/media/transcode:/transcode` — transcode temp; SSD or tmpfs recommended.
- `/mnt/plex/media/movies:/movies` — movies library.
- `/mnt/plex/media/tvshows:/tv` — TV library.
- `/mnt/plex/media/music:/music` — music library.

Usage
From the `plex/` directory (auto-loads `.env`):
1) `docker compose up -d`
2) `docker compose logs -f` (watch startup)

From the repo root (explicit env and compose path):
- `docker compose --env-file plex/.env -f plex/docker-compose.yml up -d`

Validate configuration (shows resolved values):
- From `plex/`: `docker compose config`
- From root: `docker compose --env-file plex/.env -f plex/docker-compose.yml config`

GPU Acceleration (optional)
- Install NVIDIA drivers and NVIDIA Container Toolkit on the host.
- Set `runtime: nvidia` (already present) and `NVIDIA_VISIBLE_DEVICES` in `.env`.
- Healthcheck uses `nvidia-smi` to confirm GPU visibility.

Troubleshooting
- Variables not substituting: ensure you run from `plex/` or use `--env-file plex/.env`.
- Permissions on config: match `PUID`/`PGID` to the host user that owns `/config`.
- Network discovery: verify `ADVERTISE_IP` and `ALLOWED_NETWORKS` are correct for your LAN.
- Transcoding performance: place `/transcode` on fast SSD or consider tmpfs.

