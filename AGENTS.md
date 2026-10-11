# AGENTS.md — Playlist-Converter

Guide for autonomous agents (AI assistants, automation tools) that install or
operate Playlist-Converter on behalf of a user. For humans, see
[README.md](README.md).

## What this repository is

- Release-only mirror of Playlist-Converter (project name *Tamilein*). It contains
  **no source code** — only `README.md`, `llms.txt`, this file and the
  [releases](https://github.com/BjoernHa/Playlist-Converter-Tamilein/releases/latest).
- The app transfers playlists between Spotify, Deezer, TIDAL, YouTube Music,
  SoundCloud and Apple Music. It runs locally; the user interface is available
  in **English and German** (language button at the top right; German is the
  default).
- Deutsch: Leitfaden für Agenten, die Playlist-Converter für einen Nutzer
  installieren oder bedienen. Die Regeln unten gelten unverändert.

## Install

Pick the newest release programmatically — asset names contain the version:

```bash
# list the newest files
curl -s https://api.github.com/repos/BjoernHa/Playlist-Converter-Tamilein/releases/latest \
  | grep -o '"browser_download_url": *"[^"]*"'
```

| Platform | Asset | Install |
|---|---|---|
| Debian / Ubuntu / Linux Mint | `tamilein_<version>_all.deb` | `sudo apt install ./tamilein_<version>_all.deb` (needs `sudo`; the installation sets up a Python environment and needs internet once) |
| Windows 10/11 | `Tamilein-<version>-Setup.exe` | run it; per-user install, no admin rights |
| Android 7+ | `tamilein-<version>.apk` | install on the device; the user must allow installs from this source |

Do not install anything without the user's consent, and never use `sudo` on
their behalf without asking.

## Run

- Linux: `tamilein` starts the local server (or reuses a running one) and opens
  the browser; `tamilein --port 9000` uses another port. `tamilein-update`
  installs a newer `.deb`.
- Windows: Start menu → *Playlist-Converter*.
- The web UI and a JSON API listen on `http://127.0.0.1:8000`.
  `GET /healthz` answers without authentication:
  `{"app": "tamilein", "version": "<version>"}`.

## Local HTTP API (for automation)

The UI talks to a local JSON API. It is **internal and may change between
versions**; prefer guiding the user through the UI. Typical sequence:

0. `POST /api/sprache` `{"sprache": "en"}` — switch the UI (and messages) to English; `"de"` for German. The choice is saved.
1. `GET /api/state` — language (`sprache`), services, connection state (`session == "ok"` means verified; `dataporter: true` on Spotify means "connected without Premium", i.e. file mode), selection, plan, job.
2. `POST /api/library/load` `{"source": "deezer"}` — returns a job; poll `GET /api/jobs/{job_id}` until `status != "running"`.
3. `POST /api/selection` `{"target": "tidal", "playlist_ids": ["…"], "include_liked": false}`
4. `POST /api/analyse` → job (writes a JSON backup, detects name collisions).
5. `POST /api/match` → job (searches every song on the target).
6. `GET /api/review` — uncertain matches; decisions via `POST /api/review/{index}/accept` or `/skip`.
7. `POST /api/transfer` → job; the report is in `GET /api/state` → `report`.

Spotify without Premium: `POST /api/providers/spotify/dataporter` `{"aktiv": true}`
switches Spotify to file mode (source = import of a data-porter export via
`POST /api/import`, target = one data-porter file).
`POST /api/alles-trennen` `{"zugangsdaten": false}` disconnects **every** service
and resets selection and plan — destructive; only on the user's explicit request.

Cancel a running job with `POST /api/jobs/{job_id}/cancel`. On `127.0.0.1` no
key is needed. With `--host 0.0.0.0` every request needs the access key shown at
start, and the Android app always uses one.
The server only answers requests addressed to an IP address or `localhost`
(other host names get HTTP 421), and state-changing requests carrying a
foreign `Origin` header are rejected (HTTP 403). Plain `curl` against
`http://127.0.0.1:8000` works as before.

## Rules for agents

- **Never auto-confirm uncertain matches** on the user's behalf unless they
  explicitly ask for it. The app deliberately leaves them for review; a wrong
  song in a playlist is worse than a missing one.
- **Treat all service credentials as secrets.** Deezer `arl`, SoundCloud
  `oauth_token`, YouTube Music headers, the Apple Music `media-user-token` (and
  any copied `amp-api` request) and TIDAL/Spotify client secrets grant full
  account access. Never log, echo, commit or send them anywhere.
- **Do not expose the server** (`--host 0.0.0.0`) unless the user asks; it then
  requires the access key.
- Deezer, YouTube Music, SoundCloud and Apple Music are connected through their
  websites' interfaces (unofficial). Respect the services' terms and rate limits.
  Apple Music needs an active Apple Music subscription. As a source, its "liked
  songs" are the songs in the user's library (Apple offers no request that lists
  favorites); as a target, liked songs are added to the library and marked as
  favorites.
- Spotify without Premium cannot be written to directly. Connect it "without
  Premium" and choose it as the target anyway: the app generates one file for
  the Spicetify app [data-porter](https://github.com/Prog-Jacob/spicetify-apps),
  which the user imports in the Spotify desktop client.
- **Never disconnect services or reset the app** (`/api/alles-trennen`, logout
  routes) unless the user explicitly asks for it.
- Report outcomes honestly: the transfer report lists skipped songs and errors.

## Support

Questions and source-code requests: open an issue in this repository.
