# 🟦 Navidrome Scrobbling Guide

Navidrome allows you to **track your listening activity** using scrobbling services like **ListenBrainz** and **Last.fm**.

---

## 🔌 How to Enable Scrobbling

### ListenBrainz

- Easy to set up directly from the **Navidrome web interface**.  
- Just enter your **ListenBrainz account credentials** and enable scrobbling.  
- Tracks will automatically be sent while you listen.

### Last.fm (Docker Users)

- Slightly more complex if running Navidrome via **Docker**:
  1. You need to set your **Last.fm API key and secret** as **environment variables** in your Docker container.  
  2. Then log in via the **Navidrome web UI** to enable scrobbling.  
- Once configured, Navidrome will send all played tracks to Last.fm.

---

## ⚠️ Notes / Disclaimer

- Navidrome scrobbling requires **NodeDoc / proper Docker environment** when using Last.fm.  
- Ensure your **Navidrome version** is up-to-date to support scrobbling features.  
- Offline playback may only scrobble when the server reconnects to the internet.  
- Always check your **API keys and environment variables** for correctness.

---

> Tip: ListenBrainz is the simpler option if you want scrobbling without configuring Docker environment variables.
