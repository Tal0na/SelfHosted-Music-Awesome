# 🎧 Quick Music Server (Docker)

Guia minimalista para subir um servidor de música (Navidrome/Jellyfin) via Docker.

## 📁 Estrutura e Setup

```bash
mkdir -p music-server/{data,music,plugins} && cd music-server

🐳 Docker Compose (docker-compose.yml)

services:
  music:
    image: deluan/navidrome:latest
    container_name: music-server
    ports: [ "4533:4533" ]
    restart: unless-stopped
    environment:
      ND_SCANINTERVAL: 1h
      ND_LOGLEVEL: info
    volumes:
      - ./data:/data
      - ./music:/music:ro
      - ./plugins:/plugins

🛠️ C🛠️ Comandos Essenciais

    Subir: docker compose up -d

    Parar: docker compose down

    Logs: docker compose logs -f

    Update: docker compose pull && docker compose up -d

⚙️ Configuração


Variável,Função,Exemplo
SCAN_INTERVAL,Frequência de scan,"1h, 30m"
BASE_URL,Caminho para Proxy,/music
TRANSCODING,Conversão de áudio,true


Acesso: http://localhost:4533 (Crie o admin no primeiro login).

    [!TIP]
    Use :ro no volume de músicas para proteger seus arquivos contra escrita acidental.

