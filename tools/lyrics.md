# 🎵 Adding Lyrics to Navidrome

This guide explains how Navidrome searches for lyrics and how you can easily add them to your music files.

---

## ✅ Quick Overview

Navidrome supports:

- 📄 External lyric files (`.lrc` or `.txt`)
- 🏷️ Embedded lyrics inside audio file tags

By default, Navidrome searches in this order:


.lrc → .txt → embedded


This is controlled by the environment variable:


ND_LYRICSPRIORITY


Default value:


.lrc,.txt,embedded


---

# 1️⃣ External Synced Lyrics (.lrc)

This is the best option if you want **synchronized (karaoke-style) lyrics**.

## 📌 Step 1 — Match the filename exactly

The lyric file must:

- Have the **same base name** as the audio file
- Be in the **same folder**

Example:


01 - My Song.mp3
01 - My Song.lrc


⚠️ On Linux systems, filenames are case-sensitive.

---

## 📌 Step 2 — Use correct LRC format

Example:


[00:12.00] First line of lyrics
[00:34.50] Second line of lyrics


Format:


[mm:ss.xx] Lyrics line


Navidrome will automatically sync the lyrics during playback.

---

# 2️⃣ Embedded Lyrics (Unsynchronized)

Use this if you only want **static lyrics (no timing)**.

Lyrics are stored inside the audio file metadata:

- MP3 → `USLT` tag  
- FLAC / OGG → `LYRICS` or `UNSYNCEDLYRICS` field  

---

## 🛠 Tools to Add Embedded Lyrics

### Mp3tag (Windows)
- Open file  
- Go to **Extended Tags**  
- Add or edit the `LYRICS` field  

### Kid3 (Windows / Linux / macOS)
- Edit the **Unsynchronised lyrics** field  

### CLI Tools (for advanced users)
- `eyeD3`
- `mid3v2`
- `vorbiscomment`

Useful for bulk scripting.

---

# 3️⃣ Configure Lyrics Priority

You can change the search order using:


ND_LYRICSPRIORITY


Example values:


.lrc,.txt,embedded
embedded,.lrc


---

## 🐳 Docker Example

```yaml
services:
  navidrome:
    image: deluan/navidrome:latest
    environment:
      - ND_LYRICSPRIORITY=.lrc,.txt,embedded
🪟 Windows PowerShell (temporary)
$env:ND_LYRICSPRIORITY = ".lrc,.txt,embedded"
Restart-Service navidrome
4️⃣ Force a Library Rescan

After adding lyrics:

Restart Navidrome

Use the Scan option in the Admin panel

Modify the file date to trigger re-indexing

5️⃣ Minimal LRC Example
[00:00.00] Intro
[00:15.20] Verse 1 line 1
[00:22.50] Verse 1 line 2
🔧 Troubleshooting

✔️ Filenames must match exactly
✔️ Check ND_LYRICSPRIORITY
✔️ If embedded lyrics don’t show, confirm correct tag field
✔️ Enable debug logs with:

ND_LOGLEVEL=debug
🔗 Useful Link

Navidrome Configuration Options:
https://www.navidrome.org/docs/usage/configuration/options/