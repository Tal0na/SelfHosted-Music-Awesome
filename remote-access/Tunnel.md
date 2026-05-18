# 🚇 Tunnel
Expose your local server securely to the internet without opening router ports or needing a static IP.

---

## ☁️ Cloudflare Tunnel
One of the most popular and reliable options. Routes traffic through Cloudflare's global network.

➡️ See the full guide here:
**[Cloudflare Tunnel](./tunnel/cloudflare.md)**

Examples include:
- No open ports or static IP required
- Free tier available with generous limits
- Integrated with Cloudflare DNS and DDoS protection
- Works great behind CGNAT

---

## 🔒 Tailscale
A mesh VPN built on WireGuard. Ideal for private access between your own devices.

➡️ See the full guide here:
**[Tailscale](./tunnel/tailscale.md)**

Examples include:
- Private access without exposing anything to the public internet
- Zero-config — works out of the box on most systems
- Share access with trusted users via Tailscale accounts
- Funnel feature allows limited public exposure if needed

---

## 🌐 Ngrok
A quick and easy tunneling tool, great for testing and temporary access.

➡️ See the full guide here:
**[Ngrok](./tunnel/ngrok.md)**

Examples include:
- Instant public URL for your local server
- Useful for development and short-term sharing
- Free tier available (with session/bandwidth limits)
- Supports HTTP, HTTPS, and TCP tunnels

---

## ⚡ bore / frp / rathole
Lightweight self-hosted tunnel alternatives for advanced users.

➡️ See the full guide here:
**[Self-hosted Tunnels](./tunnel/selfhosted.md)**

Examples include:
- Full control over your tunnel infrastructure
- No third-party dependency or data passing through external servers
- `bore` and `rathole` are simple and Rust-based (fast and low overhead)
- `frp` (Fast Reverse Proxy) is highly configurable and widely used

---

## ⚠️ Notes
- Cloudflare Tunnel and Tailscale are recommended for most users
- Tunnels can be combined with a Reverse Proxy for more control
- Self-hosted tunnels require a VPS with a public IP as the relay
- Always restrict access where possible — avoid exposing admin panels publicly
