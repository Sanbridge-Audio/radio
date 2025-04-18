# Logitech Media Server (LMS) Command Reference

A comprehensive list of commands for controlling LMS via CLI or JSON-RPC.

---

## Table of Contents
1. [Player Control](#player-control)
2. [Playlist Management](#playlist-management)
3. [Mixer Controls](#mixer-controls)
4. [Status Queries](#status-queries)
5. [Library Browsing](#library-browsing)
6. [System/Server Commands](#systemserver-commands)
7. [Notifications/Alarms](#notificationsalarms)
8. [Favorites/Radio](#favoritesradio)

---

## Player Control

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `play`           | -                              | Start playback               | `["play"]`                                  |
| `pause`          | -                              | Pause playback               | `["pause"]`                                 |
| `stop`           | -                              | Stop playback                | `["stop"]`                                  |
| `power`          | `0` (off), `1` (on), `?` (query) | Power control                | `["power", "1"]`                            |
| `mode`           | `play`, `pause`, `stop`        | Set playback mode            | `["mode", "play"]`                          |

---

## Playlist Management

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `playlist`       | `load <playlist_name>`         | Load a playlist              | `["playlist", "load", "My Playlist"]`       |
| `playlist`       | `add <track_url>`              | Add track to queue           | `["playlist", "add", "spotify://track/123"]`|
| `playlist`       | `clear`                        | Clear playlist               | `["playlist", "clear"]`                     |
| `playlist`       | `shuffle 0`/`1`                | Toggle shuffle               | `["playlist", "shuffle", "1"]`              |
| `playlist`       | `repeat 0`/`1`/`2`             | Repeat mode                  | `["playlist", "repeat", "2"]`               |
| `playlist`       | `index +1`/`-1`                | Skip tracks                  | `["playlist", "index", "+1"]`               |

---

## Mixer Controls

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `mixer`          | `volume <0-100>`               | Set volume                   | `["mixer", "volume", "75"]`                 |
| `mixer`          | `muting 0`/`1`/`toggle`        | Mute control                 | `["mixer", "muting", "toggle"]`             |
| `mixer`          | `bass <-100-100>`              | Adjust bass                  | `["mixer", "bass", "+5"]`                   |
| `mixer`          | `treble <-100-100>`            | Adjust treble                | `["mixer", "treble", "-3"]`                 |

---

## Status Queries

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `status`         | `- 1 tags:...`                 | Full player status           | `["status", "-", "1", "tags:cgABbehldiqtyrSuoKLN"]` |
| `current_title`  | `?`                            | Current track title          | `["current_title", "?"]`                    |
| `time`           | `?`                            | Elapsed time                 | `["time", "?"]`                             |
| `duration`       | `?`                            | Track duration               | `["duration", "?"]`                         |

---

## Library Browsing

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `artists`        | `0 100`                        | List artists                 | `["artists", "0", "100"]`                   |
| `albums`         | `0 100 artist_id:...`          | List albums by artist        | `["albums", "0", "100", "artist_id:123"]`   |
| `titles`         | `0 100 album_id:...`           | List tracks in album         | `["titles", "0", "100", "album_id:456"]`    |
| `genres`         | `0 100`                        | List genres                  | `["genres", "0", "100"]`                    |

---

## System/Server Commands

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `players`        | `0 99`                         | List all players             | `["players", "0", "99"]`                    |
| `serverstatus`   | `0 99`                         | Server status                | `["serverstatus", "0", "99"]`               |
| `rescan`         | `?`                            | Trigger library rescan       | `["rescan", "?"]`                           |
| `sync`           | `- <player_id>`                | Sync players                 | `["sync", "-", "00:04:20:ab:cd:ef"]`        |

---

## Notifications/Alarms

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `alarm`          | `add time:... days:...`        | Add alarm                    | `["alarm", "add", "time:07:00", "days:12345"]` |
| `alarm`          | `update id:... enabled:0`      | Disable alarm                | `["alarm", "update", "id:1", "enabled:0"]`  |

---

## Favorites/Radio

| Command          | Parameters                     | Description                  | Example (JSON-RPC)                          |
|------------------|--------------------------------|------------------------------|---------------------------------------------|
| `favorites`      | `0 100`                        | List favorites               | `["favorites", "0", "100"]`                 |
| `favorites`      | `play item_id:...`             | Play favorite                | `["favorites", "play", "item_id:789"]`      |
| `radio`          | `play url:...`                 | Play radio stream            | `["radio", "play", "url:http://icecast.com/stream"]` |

---

## Special Parameters
- `?`: Query a value (e.g., `["mixer", "volume", "?"]`).
- `toggle`: Toggle a state (e.g., `["mixer", "muting", "toggle"]`).
- Use `0` as the player ID for server-wide commands.

---

## Tips
1. Use `players 0 99` to discover player MAC addresses.
2. For radio streams, use direct URLs (e.g., `http://icecast.com/stream.mp3`).
3. Include `tags:...` in `status` to control returned metadata.
4. Use `curl` or Node-RED to send JSON-RPC commands:
   ```bash
   curl -X POST -d '{"id":1,"method":"slim.request","params":["PLAYER_ID",["playlist","index","+1"]]}' http://SERVER_IP:9000/jsonrpc.js