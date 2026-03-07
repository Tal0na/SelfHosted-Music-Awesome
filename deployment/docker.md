# 🐳 Selfhost Music with Docker & Docker Compose

A lightweight, self-hosted music server and streamer. This guide covers setting up Selfhost Music using Docker or Docker Compose, including support for external plugins.

---

## 📦 Option 1 — Docker CLI

If you prefer running a single command to get things moving, follow these steps.

### 1️⃣ Create the Directory Structure

First, set up the folders where your data and music will live.

mkdir -p ~/selfhost-music/data ~/selfhost-music/music ~/selfhost-music/plugins

data: Database and internal configurations.

music: Your audio library (MP3, FLAC, etc.).

plugins: External binary plugins.

2️⃣ Start the Container
Run the following command to pull the image and start the server:

docker run -d \
 --name selfhost-music \
 -p 4533:4533 \
 -v ~/selfhost-music/data:/data \
 -v ~/selfhost-music/music:/music \
 -v ~/selfhost-music/plugins:/plugins \
 -e ND_SCANINTERVAL=1h \
 -e ND_LOGLEVEL=info \
 --restart unless-stopped \
 deluan/navidrome:latest

🔌 Adding Plugins
Selfhost Music allows external plugins for metadata or integration. Here is how to install them:

1 Download the plugin binary.

2 Move it to the plugins folder: ~/selfhost-music/plugins/

3 Make it executable:
chmod +x ~/navidrome/plugins/your-plugin-name

4 Restart the container to apply changes:
docker restart selfhost-music

🐳 Option 2 — Docker Compose (Recommended)
Docker Compose is the best way to manage your stack for long-term use.

1️⃣ Create docker-compose.yml Navigate to your ~/selfhost-music/ folder and create the file:

services:
selfhost-music:
image: deluan/navidrome:latest
container_name: selfhost-music
user: 1000:1000 # Recommended: adjust to your UID/GID
ports: - "4533:4533"
restart: unless-stopped
environment:
ND_SCANINTERVAL: 1h
ND_LOGLEVEL: info
ND_BASEURL: ""
volumes: - ./data:/data - ./music:/music:ro # Read-only for safety - ./plugins:/plugins

2️⃣ Management Commands

Launch: docker compose up -d
Stop: docker compose down
View Logs: docker compose logs -f

📁 Recommended File StructureYour project directory should look like this:

navidrome/
├── docker-compose.yml
├── data/ # Auto-generated database files
├── music/ # Drop your albums here
└── plugins/ # External plugin binaries
⚙️ Useful Environment VariablesVariableDescriptionExample

ND_SCANINTERVALHow often to scan for new music.1h, 30m
ND_LOGLEVELDetail level of logs.info, debug, error
ND_BASEURLSet this if using a Reverse Proxy./music
ND_ENABLETRANSCODINGEnable on-the-fly audio conversion.true

🚀 Get Started!
Once the container is running, open your browser and go to:
👉 <http://localhost:4533> (or your server's IP)

Note: On your first visit, you will be prompted to create an admin account.🎵
