# 🔒 Tailscale

A mesh VPN built on WireGuard. Connect your devices privately and securely — no port forwarding, no static IP, no complex config.

---

## What is Tailscale?

Tailscale creates a private encrypted network (a **tailnet**) between all your authorized devices. Every device gets a stable private IP (`100.x.x.x`) that works across networks, firewalls, and CGNAT.

---

## Prerequisites

- A free Tailscale account at [https://tailscale.com](https://tailscale.com)
- Devices where you want to install the agent

---

## Installation

### Linux
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### macOS
```bash
brew install tailscale
```
Or download the app from the Mac App Store.

### Windows
Download the installer from [https://tailscale.com/download](https://tailscale.com/download)

### Raspberry Pi / ARM
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

---

## Basic Setup

### 1. Start and authenticate
```bash
sudo tailscale up
```
A login URL will appear — open it and authenticate with your Tailscale account.

### 2. Check your IP
```bash
tailscale ip -4
# → 100.x.x.x
```

### 3. Verify connectivity
From another device on your tailnet:
```bash
ping 100.x.x.x
# or use the MagicDNS hostname:
ping my-server
```

That's it. Your devices can now communicate privately.

---

## MagicDNS

Tailscale provides automatic DNS for your devices. Instead of IPs, use hostnames:

```
http://my-server:3000
ssh my-server
```

Enable it in: **Tailscale Admin Console → DNS → Enable MagicDNS**

---

## Subnet Routing

Expose an entire local subnet (e.g., your home LAN) to other tailnet devices:

```bash
sudo tailscale up --advertise-routes=192.168.1.0/24
```

Then approve the route in the Admin Console under the machine settings.

---

## Tailscale Funnel (Limited Public Access)

Funnel allows you to expose a local service to the **public internet** via a Tailscale-managed HTTPS URL:

```bash
tailscale funnel 3000
```

This provides a public URL like `https://my-server.tail1234.ts.net`.

> ⚠️ Funnel is intended for temporary or limited sharing. For production public access, prefer [Cloudflare Tunnel](./cloudflare.md).

---

## Access Control (ACLs)

Fine-grained control over which devices can talk to each other.

Edit your ACL policy in the Admin Console:
```json
{
  "acls": [
    {
      "action": "accept",
      "src": ["tag:dev"],
      "dst": ["tag:server:*"]
    }
  ]
}
```

Use tags to group devices by role (e.g., `tag:server`, `tag:laptop`).

---

## Share Access with Others

You can share individual devices with other Tailscale users:

```bash
tailscale share <machine-name>
```

Or manage sharing via the Admin Console → Machines → Share.

---

## Run as a System Service

On Linux, Tailscale runs as a systemd service automatically:

```bash
sudo systemctl enable --now tailscaled
```

---

## Useful Commands

```bash
# Show all devices in your tailnet
tailscale status

# Disconnect (keeps the agent running)
tailscale down

# Reconnect
tailscale up

# View current IP
tailscale ip

# Check logs
journalctl -u tailscaled -f
```

---

## Notes

| Feature            | Detail                            |
| ------------------ | --------------------------------- |
| Free tier          | Up to 100 devices, 3 users        |
| Encryption         | WireGuard (end-to-end)            |
| Ports to open      | None                              |
| Public exposure    | Only via Funnel (optional)        |
| Works behind CGNAT | Yes                               |
| Self-hostable      | Yes — via Headscale (open source) |

---

## Self-hosting with Headscale

[Headscale](https://github.com/juanfont/headscale) is an open-source, self-hosted Tailscale control server. It allows you to run your own tailnet without relying on Tailscale's infrastructure.

```bash
# Install headscale (see official docs)
headscale namespaces create myhome
headscale --namespace myhome preauthkeys create --reusable --expiration 24h
```

---

> **See also:** [Cloudflare Tunnel](./cloudflare.md) for public traffic · [Ngrok](./ngrok.md) for quick testing · [Self-hosted Tunnels](./selfhosted.md) for full control
