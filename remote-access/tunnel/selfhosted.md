# ⚡ Self-hosted Tunnels

Run your own tunnel infrastructure using open-source tools. Full control, no third-party dependencies, no data passing through external servers.

> **Requirements:** A VPS with a public IP to act as the relay server.

---

## Overview

| Tool      | Language | Best For                         | Complexity  |
| --------- | -------- | -------------------------------- | ----------- |
| `bore`    | Rust     | Simple HTTP/TCP tunnels          | Low         |
| `rathole` | Rust     | High-performance, production use | Medium      |
| `frp`     | Go       | Feature-rich, configurable       | Medium–High |

---

## bore

The simplest option. Expose a local port through a relay server in one command.

### Install

#### On both server and client:
```bash
cargo install bore-cli
```

Or download prebuilt binaries from [https://github.com/ekzhang/bore/releases](https://github.com/ekzhang/bore/releases):
```bash
# Linux x86_64
wget https://github.com/ekzhang/bore/releases/latest/download/bore-v0.5.0-x86_64-unknown-linux-musl.tar.gz
tar xzf bore-*.tar.gz
sudo mv bore /usr/local/bin/
```

### Setup

#### On your VPS (relay server):
```bash
bore server
```
Listens on port `7835` by default.

#### On your local machine (client):
```bash
bore local 3000 --to your-vps-ip
```

Output:
```
listening at your-vps-ip:XXXXX
```

Traffic to `your-vps-ip:XXXXX` is now forwarded to `localhost:3000`.

### With a secret (prevent unauthorized use)
```bash
# Server
bore server --secret mysecret

# Client
bore local 3000 --to your-vps-ip --secret mysecret
```

### Systemd service (VPS)
```ini
# /etc/systemd/system/bore.service
[Unit]
Description=bore relay server
After=network.target

[Service]
ExecStart=/usr/local/bin/bore server
Restart=always
User=nobody

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl enable --now bore
```

---

## rathole

High-performance, production-ready reverse proxy tunnel. Better suited for persistent services.

### Install

Download from [https://github.com/rapiz1/rathole/releases](https://github.com/rapiz1/rathole/releases):
```bash
wget https://github.com/rapiz1/rathole/releases/latest/download/rathole-x86_64-unknown-linux-musl.zip
unzip rathole-*.zip
sudo mv rathole /usr/local/bin/
```

### Setup

#### Server config (`server.toml`) — on your VPS:
```toml
[server]
bind_addr = "0.0.0.0:2333"

[server.services.my-app]
token = "mysecrettoken"
bind_addr = "0.0.0.0:8080"   # public port on VPS
```

#### Client config (`client.toml`) — on your local machine:
```toml
[client]
remote_addr = "your-vps-ip:2333"

[client.services.my-app]
token = "mysecrettoken"
local_addr = "127.0.0.1:3000"  # your local service
```

#### Run

```bash
# On VPS
rathole server.toml

# On local machine
rathole client.toml
```

Traffic to `your-vps-ip:8080` is now forwarded to `localhost:3000`.

### Multiple services
```toml
# server.toml
[server.services.web]
token = "token-web"
bind_addr = "0.0.0.0:80"

[server.services.api]
token = "token-api"
bind_addr = "0.0.0.0:8080"
```

```toml
# client.toml
[client.services.web]
token = "token-web"
local_addr = "127.0.0.1:3000"

[client.services.api]
token = "token-api"
local_addr = "127.0.0.1:4000"
```

---

## frp (Fast Reverse Proxy)

The most feature-rich option. Widely used, highly configurable, supports HTTP/HTTPS virtual hosts, TCP, UDP, STCP, and more.

### Install

Download from [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases):

```bash
wget https://github.com/fatedier/frp/releases/latest/download/frp_0.58.0_linux_amd64.tar.gz
tar xzf frp_*.tar.gz
cd frp_*_linux_amd64
```

Binaries:
- `frps` — server (runs on VPS)
- `frpc` — client (runs locally)

### Setup

#### Server config (`frps.toml`) — on your VPS:
```toml
bindPort = 7000
vhostHTTPPort = 80
vhostHTTPSPort = 443

auth.token = "mysecrettoken"
```

#### Client config (`frpc.toml`) — on your local machine:
```toml
serverAddr = "your-vps-ip"
serverPort = 7000

auth.token = "mysecrettoken"

[[proxies]]
name = "web"
type = "http"
localPort = 3000
customDomains = ["app.yourdomain.com"]
```

#### Run
```bash
# VPS
./frps -c frps.toml

# Local machine
./frpc -c frpc.toml
```

### TCP tunnel (no domain needed)
```toml
[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000   # public port on VPS
```

Then: `ssh -p 6000 user@your-vps-ip`

### HTTPS with Let's Encrypt
frp can terminate TLS if you add certificate paths to `frps.toml`:
```toml
vhostHTTPSPort = 443

[[tlsConfig]]
certFile = "/etc/letsencrypt/live/yourdomain.com/fullchain.pem"
keyFile = "/etc/letsencrypt/live/yourdomain.com/privkey.pem"
```

### Web dashboard (frps)
```toml
webServer.addr = "0.0.0.0"
webServer.port = 7500
webServer.user = "admin"
webServer.password = "admin"
```
Access at `http://your-vps-ip:7500`

---

## Choosing the Right Tool

```
Need something simple and fast to set up?
  → bore

Need reliability and performance for a permanent service?
  → rathole

Need virtual hosts, dashboards, UDP, or advanced routing?
  → frp
```

---

## Firewall Notes

Open the required ports on your VPS:

```bash
# bore
sudo ufw allow 7835

# rathole — tunnel control port + each service port
sudo ufw allow 2333
sudo ufw allow 8080

# frp — control port + vhost ports
sudo ufw allow 7000
sudo ufw allow 80
sudo ufw allow 443
```

---

## Notes

- All three tools require a **VPS with a public IP** as the relay
- Combine with **Nginx** or **Caddy** on the VPS for TLS termination and virtual hosting
- For high-availability, run the server binary as a systemd service with `Restart=always`
- None of these route your traffic through third-party networks — everything goes through your VPS

---

> **See also:** [Cloudflare Tunnel](./cloudflare.md) for managed public access · [Tailscale](./tailscale.md) for private mesh VPN · [Ngrok](./ngrok.md) for quick testing
