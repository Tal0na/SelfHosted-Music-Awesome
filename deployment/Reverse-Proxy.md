# 🟣 Reverse Proxy

A reverse proxy is a key component for exposing your services in a clean, secure, and scalable way.

---

## 🚀 What is a Reverse Proxy?

A reverse proxy sits between the client (browser) and your server, forwarding requests to the correct service.

Instead of accessing services via IP and ports, users connect through a domain name.

---

## ✅ Why use a Reverse Proxy?

- 🌐 Access services using custom domains  
- 🔒 Enable HTTPS (SSL certificates)  
- 🧭 Route multiple services on a single server  
- 🛡️ Add an extra layer of security  

---

## 🧰 Popular Reverse Proxies

- Caddy (automatic HTTPS, very easy to use)  
- Nginx (powerful and widely adopted)  
- Traefik (perfect for Docker environments)  

---

## 🛠️ Basic Setup

1. Install a reverse proxy (Caddy, Nginx, or Traefik)  
2. Point your domain to your server IP  
3. Configure routing rules for each service  
4. Enable HTTPS (usually automatic with Caddy/Traefik)  

---

## 📦 Example Use Cases

- `music.yourdomain.com` → Navidrome  
- `media.yourdomain.com` → Jellyfin  
- `dashboard.yourdomain.com` → Admin panel  

---

## ⚠️ Notes

- Requires basic networking and DNS knowledge  
- Incorrect configuration can expose services publicly  

---

## 🧠 Tip

If you're using Docker, Caddy or Traefik are the easiest options due to automatic configuration and HTTPS support.