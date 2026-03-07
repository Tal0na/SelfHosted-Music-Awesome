# 🎵 Metadata Best Practices for Music Libraries

This guide explains essential metadata tags, format differences, and practical strategies to maintain a clean, searchable, and server-friendly music library compatible with Selfhost Music and other players.

---

## 📌 Why Metadata Matters

Well-structured metadata ensures:

- Accurate browsing and searching
- Proper album grouping (including compilations and multi-disc releases)
- Reliable scrobbling
- Consistent playlist behavior
- Correct cover art and lyrics display
- Compatibility across servers and players

---

## 🏷️ Core Metadata Tags

## Essential Fields

| Field        | Purpose                           | ID3 (MP3/AAC) | Vorbis/FLAC     |
| ------------ | --------------------------------- | ------------- | --------------- |
| Title        | Track title                       | `TIT2`        | `TITLE`         |
| Artist       | Track performing artist           | `TPE1`        | `ARTIST`        |
| Album        | Album/release name                | `TALB`        | `ALBUM`         |
| Album Artist | Groups compilations correctly     | `TPE2`        | `ALBUMARTIST`   |
| Track Number | Track order                       | `TRCK`        | `TRACKNUMBER`   |
| Disc Number  | Disc index in multi-disc releases | `TPOS`        | `DISCNUMBER`    |
| Date / Year  | Release date                      | `TDRC`        | `DATE` / `YEAR` |
| Genre        | Musical category                  | `TCON`        | `GENRE`         |

---

## Advanced / Recommended Fields

- **Composer / Conductor / Publisher** — important for classical and soundtrack collections.
- **MusicBrainz IDs**
  - `musicbrainz_trackid`
  - `musicbrainz_albumid`
  - `musicbrainz_artistid`
  - Helps with persistent identification and reduces ambiguity.
- **ReplayGain**
  - `replaygain_track_gain`
  - `replaygain_album_gain`
  - Enables normalized playback where supported.
- **Cover Art**
  - Embedded image (`APIC` in ID3)
  - Or sidecar `cover.jpg` inside album folder
- **Lyrics**
  - Unsynchronized → `USLT` (ID3) / `LYRICS`
  - Synchronized → `.lrc` files

---

## 🎧 Format-Specific Notes

## ID3 (MP3 / AAC)

Common frames:

- `TIT2` — Title
- `TPE1` — Artist
- `TALB` — Album
- `USLT` — Lyrics
- `APIC` — Embedded artwork

---

## Vorbis / FLAC / OGG

Simple key-value structure:

- `TITLE`
- `ARTIST`
- `ALBUM`
- `LYRICS`

More flexible and easier to edit in bulk.

---

## MP4 / M4A

Uses different internal atoms:

- `©nam` — Title
- `©ART` — Artist
- `©alb` — Album

GUI tools automatically translate these for you.

---

## ✅ Practical Tagging Tips

- Always fill **Album Artist** for compilations.
- Use zero-padded track numbers: `01`, `02`, `03`.
- Prefer full dates: `YYYY-MM-DD`.
- Avoid mixing multiple genres in a single string.
- Use MusicBrainz IDs when relying on automated tools.
- Keep consistent separators for multi-artist tracks.

---

## 🛠 Recommended Tools

## beets (CLI, automated)

Powerful importer with MusicBrainz integration.
