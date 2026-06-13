# HYDA Sentinel

**HYDA Sentinel** is an enterprise-grade **passive** VoIP, SIP, RTP, and network monitoring platform for telecom infrastructure. It observes mirrored or PCAP traffic, analyzes behavior, detects anomalies, generates alerts, and visualizes health — **without modifying production systems**.

## Architecture

```
Network/SIP Traffic → Port Mirror / PCAP → Collector → Analysis → PostgreSQL
                                                      ↓
                                              Alert Engine → Email / Telegram / WhatsApp
                                                      ↓
                                              Dashboard (React) + WebSocket
```

## Stack

| Layer | Technology |
|-------|------------|
| Frontend | React, Tailwind CSS, Chart.js |
| Backend | FastAPI, WebSocket, ARQ workers |
| Database | PostgreSQL, Redis |
| Capture | tshark, pyshark, scapy, tcpdump |

## Quick Start (Docker)

```bash
cp .env.example .env
# Edit JWT_SECRET_KEY and POSTGRES_PASSWORD

docker compose up -d --build
```

- **Dashboard:** http://localhost:5173  
- **API docs:** http://localhost:8000/api/docs  
- **Default login:** `admin@hydra-sentinel.com` / value of `ADMIN_PASSWORD` in `.env` (default `HydraSentinel2026`)

### Sample PCAP

```bash
pip install scapy
python samples/generate_sample_pcap.py
docker compose restart collector
```

## Project Structure

```
HYDA SENTINEL/
├── backend/          # FastAPI, collectors, analysis, workers
├── frontend/         # React NOC dashboard
├── db/init.sql       # PostgreSQL schema
├── nginx/            # HTTPS reverse proxy (optional profile)
├── samples/          # PCAP generators
└── docker-compose.yml
```

## Core Modules

1. **Packet Collection** — port mirror (live) or PCAP folder/upload, SIP/RTP parsing
2. **Traffic Analysis** — SIP/RTP/network health, sessions, SLA snapshots
3. **Threat Detection** — Brute-force, scanning, geo map, suspicious IPs
4. **Alert Engine** — Deduped, throttled, escalated multi-channel notify
5. **Dashboard** — Sessions, packet inspector, server health, RTP heatmap, global search
6. **Platform** — Multi-tenant admin, encrypted notification channels, Grafana export

## Security

- JWT authentication + RBAC (admin, operator, viewer, auditor)
- API rate limiting (SlowAPI)
- Audit logging
- Environment-based secrets (no hardcoded credentials)
- HTTPS via nginx profile: `docker compose --profile https up`

## Documentation

- [INSTALLATION.md](INSTALLATION.md) — Local and Docker setup
- [DEPLOYMENT.md](DEPLOYMENT.md) — Production deployment guide

## Passive Monitoring Guarantee

HYDA Sentinel does **not**:

- Modify telecom configs or dialplans
- Route, forward, or block traffic
- Require SSH/root on production switches
- Act as SBC, firewall, or router

It only **observes** mirrored/PCAP input from a dedicated monitoring tap.

## License

Proprietary — HYDA Sentinel © 2026
