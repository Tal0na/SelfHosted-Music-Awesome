# 🟦 Navidrome Scrobbling Guide

## The Last.fm and ListenBrainz plugins will export and store your scrobbles

Navidrome allows you to **track your listening activity** and **your track Loved** using scrobbling services like **ListenBrainz** and **Last.fm**.

---

[docs navidrome scrobbling](https://www.navidrome.org/docs/usage/features/scrobbling/)

### 🔌 How to Enable Scrobbling

1. **Open your Jellyfin dashboard.**
2. Search for the **scrobbling** you want (e.g., Last.fm or ListenBrainz).  
3. settings:
   - Enter your **API key** if required.  
   - Log in with your **scrobbling account** (Last.fm / ListenBrainz).  
4. Save your settings and restart Jellyfin if prompted.  

### ⚠️ Notes / Disclaimer

- Navidrome scrobbling requires **NodeDoc / proper Docker environment** when using Last.fm.  
- Ensure your **Navidrome version** is up-to-date to support scrobbling features.  
- Offline playback may only scrobble when the server reconnects to the internet.  
- Always check your **API keys and environment variables** for correctness.

---
> Tip: ListenBrainz is the simpler option if you want scrobbling without configuring Docker environment variables.
