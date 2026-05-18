# 🌐 Ngrok

Instant public URLs for your local server. Perfect for development, testing, and short-term sharing.

---

## What is Ngrok?

Ngrok creates a secure tunnel from a public URL to a port on your local machine. It's the fastest way to share a local server with someone on the internet — no domain, no setup, no firewall changes.

---

## Prerequisites

- A free Ngrok account at [https://ngrok.com](https://ngrok.com)
- The `ngrok` CLI installed

---

## Installation

### Linux
```bash
curl -sSL https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
  | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
  && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
  | sudo tee /etc/apt/sources.list.d/ngrok.list \
  && sudo apt update && sudo apt install ngrok
```

Or download the binary directly:
```bash
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar xvzf ngrok-v3-stable-linux-amd64.tgz
sudo mv ngrok /usr/local/bin/
```

### macOS
```bash
brew install ngrok/ngrok/ngrok
```

### Windows
Download from [https://ngrok.com/download](https://ngrok.com/download) or use Chocolatey:
```bash
choco install ngrok
```

---

## Setup

### 1. Authenticate
Get your auth token from [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken):

```bash
ngrok config add-authtoken <YOUR_AUTH_TOKEN>
```

### 2. Start a tunnel
```bash
ngrok http 3000
```

Output:
```
Forwarding  https://a1b2c3d4.ngrok-free.app -> http://localhost:3000
```

Share that URL with anyone — it's publicly accessible immediately.

---

## Common Usage

### HTTP tunnel
```bash
ngrok http 8080
```

### HTTPS only (redirect HTTP)
```bash
ngrok http 3000 --scheme https
```

### TCP tunnel (for raw TCP, SSH, databases)
```bash
ngrok tcp 22       # expose SSH
ngrok tcp 5432     # expose PostgreSQL
```

### Custom subdomain (paid plans)
```bash
ngrok http --subdomain=myapp 3000
# → https://myapp.ngrok.io
```

### With a custom domain (paid plans)
```bash
ngrok http --hostname=tunnel.yourdomain.com 3000
```

---

## Static Domains (Free Tier)

Ngrok free accounts get **one free static domain** so your URL doesn't change on restart:

1. Go to **Dashboard → Domains → New Domain**
2. Copy your static domain (e.g., `yourname.ngrok-free.app`)
3. Use it:

```bash
ngrok http --hostname=yourname.ngrok-free.app 3000
```

---

## Config File

For reusable configurations, create `~/.config/ngrok/ngrok.yml`:

```yaml
version: "2"
authtoken: <YOUR_AUTH_TOKEN>

tunnels:
  web:
    proto: http
    addr: 3000
    hostname: yourname.ngrok-free.app
  api:
    proto: http
    addr: 8080
  ssh:
    proto: tcp
    addr: 22
```

Start all tunnels at once:
```bash
ngrok start --all
```

Start a specific one:
```bash
ngrok start web
```

---

## Web Inspection Interface

Ngrok includes a local dashboard for inspecting and replaying requests:

```
http://127.0.0.1:4040
```

Useful for debugging webhooks — you can see every request body, headers, and replay them instantly.

---

## Webhook Testing

Ngrok is the go-to tool for testing webhooks locally:

```bash
ngrok http 3000
# Point your webhook provider (Stripe, GitHub, etc.) to the ngrok URL
```

Use the inspection UI at `localhost:4040` to replay failed webhook deliveries.

---

## Request Filtering & Auth

### Basic auth (protect your tunnel)
```bash
ngrok http 3000 --basic-auth="user:password"
```

### IP allowlist (paid)
```bash
ngrok http 3000 --cidr-allow=203.0.113.0/24
```

---

## Run as a Background Service

### Linux (systemd)
```bash
ngrok service install --config ~/.config/ngrok/ngrok.yml
sudo systemctl start ngrok
sudo systemctl enable ngrok
```

---

## Free Tier Limits

| Limit                | Free      | Paid      |
| -------------------- | --------- | --------- |
| Simultaneous tunnels | 1         | Multiple  |
| Static domains       | 1         | Multiple  |
| Custom domains       | ❌         | ✅         |
| TCP tunnels          | 1         | Multiple  |
| Bandwidth            | Limited   | Higher    |
| Session duration     | Unlimited | Unlimited |

---

## Troubleshooting

```bash
# Check ngrok version
ngrok version

# Test config
ngrok config check

# Verbose output
ngrok http 3000 --log=stdout --log-level=debug
```

---

> **See also:** [Cloudflare Tunnel](./cloudflare.md) for production/permanent setup · [Tailscale](./tailscale.md) for private access · [Self-hosted Tunnels](./selfhosted.md) for full control
