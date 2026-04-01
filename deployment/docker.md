# 🎧 Quick Music Server (Docker)

Minimal guide to deploy a music server (Navidrome/Jellyfin) using Docker.

## 📁 Estrutura e Setup

```bash
mkdir -p music-server/{data,music,plugins} && cd music-server
```
🐳 Docker Compose (docker-compose.yml)
```bash
services:
  music:
    image: deluan/navidrome:latest
    container_name: music-server
    ports:
      - "4533:4533"
    restart: unless-stopped
    environment:
      ND_SCANINTERVAL: 1h
      ND_LOGLEVEL: info
    volumes:
      - ./data:/data
      - ./music:/music:ro
      - ./plugins:/plugins
```
🛠️ C🛠️ Comandos Essenciais
```bash
    Subir: docker compose up -d

    Parar: docker compose down

    Logs: docker compose logs -f

    Update: docker compose pull && docker compose up -d
```
## ⚙️ Configuration

Variable,Description,Example
SCAN_INTERVAL,Scan frequency,"1h, 30m"
BASE_URL,Reverse proxy base path,/music
TRANSCODING,Enable audio transcoding,true


## 🌐 Access
Open in your browser:
http://localhost:4533

Create your admin account on first login.

[!TIP]
Use :ro on the music volume to prevent accidental file modifications.

