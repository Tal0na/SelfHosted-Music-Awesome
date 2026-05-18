# ☁️ Cloudflare Tunnel

Route traffic from the internet to your local server through Cloudflare's global network — no open ports, no static IP required.

---

## Prerequisites

- A domain added to Cloudflare (free account is enough)
- `cloudflared` CLI installed on your server
- A local service running (e.g., `http://localhost:3000`)

---

## Installation

### Linux (Debian/Ubuntu)
```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb -o cloudflared.deb
sudo dpkg -i cloudflared.deb
```

### macOS
```bash
brew install cloudflared
```

### Windows
Download the installer from [https://github.com/cloudflare/cloudflared/releases](https://github.com/cloudflare/cloudflared/releases)

---

## Setup

### 1. Authenticate
```bash
cloudflared tunnel login
```
A browser window will open. Select your domain and authorize.

### 2. Create a tunnel
```bash
cloudflared tunnel create my-tunnel
```
Note the **Tunnel ID** shown in the output.

### 3. Create the config file

Create `~/.cloudflared/config.yml`:
```yaml
tunnel: <TUNNEL_ID>
credentials-file: /root/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: app.yourdomain.com
    service: http://localhost:3000
  - service: http_status:404
```

### 4. Add DNS record
```bash
cloudflared tunnel route dns my-tunnel app.yourdomain.com
```

### 5. Run the tunnel
```bash
cloudflared tunnel run my-tunnel
```

---

## Run as a System Service

```bash
sudo cloudflared service install
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
```

---

## Multiple Services

You can expose multiple local services under different subdomains:

```yaml
ingress:
  - hostname: app.yourdomain.com
    service: http://localhost:3000
  - hostname: api.yourdomain.com
    service: http://localhost:8080
  - hostname: grafana.yourdomain.com
    service: http://localhost:3001
  - service: http_status:404
```

---

## Access Policies (Zero Trust)

Restrict access to specific users or groups via Cloudflare Access:

1. Go to **Cloudflare Zero Trust → Access → Applications**
2. Add an application linked to your tunnel hostname
3. Set allowed emails, GitHub orgs, Google Workspace domains, etc.

This adds an authentication layer before your app is even reached.

---

## Notes

| Feature                | Detail                                 |
| ---------------------- | -------------------------------------- |
| Free tier              | Yes — generous limits for personal use |
| Ports to open          | None                                   |
| Works behind CGNAT     | Yes                                    |
| DDoS protection        | Included via Cloudflare network        |
| Custom domain required | Yes (free Cloudflare domain works)     |

---

## Troubleshooting

```bash
# Check tunnel status
cloudflared tunnel info my-tunnel

# List all tunnels
cloudflared tunnel list

# View live logs
cloudflared tunnel run my-tunnel --loglevel debug
```

---

> **See also:** [Tailscale](./tailscale.md) for private mesh access · [Ngrok](./ngrok.md) for quick testing · [Self-hosted Tunnels](./selfhosted.md) for full control
