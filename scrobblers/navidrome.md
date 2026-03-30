# 🟦 Navidrome Scrobbling Guide

Navidrome allows you to **track your listening activity** and **your track Loved** using scrobbling services like **ListenBrainz** and **Last.fm**.

---

## 🔌 How to Enable Scrobbling

### ListenBrainz

[jellyfin-plugin-listenbrainz](https://github.com/lyarenei/jellyfin-plugin-listenbrainz)

- Easy to set up directly from the **Navidrome web interface**.  
- Just enter your **ListenBrainz account credentials "API key"** and enable scrobbling.  
- Tracks will automatically be sent while you listen.

### Last.fm (Docker Users)

[jellyfin-plugin-lastfm](https://github.com/danielfariati/jellyfin-plugin-lastfm)

---

## ⚠️ Notes / Disclaimer

- Navidrome scrobbling requires **NodeDoc / proper Docker environment** when using Last.fm.  
- Ensure your **Navidrome version** is up-to-date to support scrobbling features.  
- Offline playback may only scrobble when the server reconnects to the internet.  
- Always check your **API keys and environment variables** for correctness.

---

> Tip: ListenBrainz is the simpler option if you want scrobbling without configuring Docker environment variables.
