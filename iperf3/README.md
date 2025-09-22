# iperf3 — Docker Compose

A minimal containerized iperf3 server for network throughput testing.

## Prerequisites
- Docker and Docker Compose v2
- Optional: host network access if you want maximum throughput

## Configuration
- This setup defaults to running iperf3 in server mode (`-s`).
- Networking options (choose one in `docker-compose.yml`):
  - Host mode: uncomment `network_mode: host` for best performance.
  - Bridge mode: comment host mode and expose ports `5201/tcp` and `5201/udp`.

## Usage
Start the server:
```bash
docker compose up -d
```

Connect from a client to test:
```bash
# TCP test (10 seconds default)
iperf3 -c <server-ip>

# UDP test at 1 Gbit/s
iperf3 -u -b 1G -c <server-ip>

# Reverse direction (server -> client)
iperf3 -c <server-ip> -R
```

## Common Options
- `-t <sec>`: test duration (default 10)
- `-P <n>`: number of parallel streams
- `-b <rate>`: bandwidth for UDP tests (e.g., `100M`, `1G`)
- `-R`: reverse test
- `-J`: JSON output

## Logs
Follow container logs:
```bash
docker compose logs -f iperf3
```

## Tips
- For most accurate results, use wired connections and host networking.
- Ensure firewalls allow port `5201` (TCP/UDP) if using bridge mode.
- Pin image versions for reproducible setups (e.g., `networkstatic/iperf3:3.16`).