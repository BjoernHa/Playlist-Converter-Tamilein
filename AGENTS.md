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
| iPhone/iPad (iOS 16+) | `tamilein-<version>.ipa` | **unsigned** — the user installs it with SideStore (or AltStore Classic) and their free Apple ID; setting up SideStore needs a computer (Windows, macOS or Linux), Wi-Fi and the LocalDevVPN app (steps in [README.md](README.md)). Preferred: in SideStore **Sources** → **+** → add `https://github.com/BjoernHa/Playlist-Converter-Tamilein/releases/latest/download/altstore-quelle.json`, then **FREE** next to Playlist-Converter — only an app installed from this source gets updates under **My Apps**; an `.ipa` shared to SideStore by hand gets none. Not yet tried on a real iPhone — checked in the simulator only. |

Do not install anything without the user's consent, and never use `sudo` on
their behalf without asking. On the iPhone, only the user can install: SideStore
signs with their Apple ID on their device — never ask for their Apple ID password.
With a free Apple ID each sideloaded app runs 7 days (SideStore refreshes it), at
most 3 such apps at a time including SideStore. SideStore itself must be opened at
least every 7 days; if it or the pairing file expires, the user needs the computer
and iloader again.

## Run

- Linux: `tamilein` starts the local server (or reuses a running one) and opens
  the browser; `tamilein --port 9000` uses another port. `tamilein-update`
  installs a newer `.deb`.
- Windows: Start menu → *Playlist-Converter*.
- Android, iPhone: open the app. It runs its own server on `127.0.0.1` (port
  picked by the app, always with an access key) and shows the UI itself; on the
  iPhone, iOS pauses the app in the background, so long jobs need the app open.
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

Update only: after loading a source that was transferred before,
`GET /api/state` → `update_lauf.verfuegbar` says whether
`POST /api/update-lauf` can append just the new songs to the playlists from
last time (a job; its result has `zustand`: `uebertragen`, `pruefen` — open
matches wait in the review — or `nichts_neu`). Remembered manual matches show
up in `GET /api/review` with `gemerkt`; `POST /api/review/gemerkte-uebernehmen`
accepts all of them.

Spotify without Premium: `POST /api/providers/spotify/dataporter` `{"aktiv": true}`
switches Spotify to file mode (source = import of a data-porter export via
`POST /api/import`, target = one data-porter file).
`POST /api/alles-trennen` `{"zugangsdaten": false, "gedaechtnis": false}` disconnects **every** service
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
