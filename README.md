# Playlist-Converter

**Transfer playlists and liked songs between Spotify, Deezer, TIDAL, YouTube Music
and SoundCloud — locally on your own device, with no transfer limits and no
third-party cloud service.**

Playlist-Converter (project name *Tamilein*) is a free desktop and mobile app for
Linux, Windows and Android. It reads playlists from one music streaming service,
finds each song on another service (ISRC first, then careful fuzzy matching) and
creates the playlists there. Everything runs on your machine; your logins never
leave it.

This repository contains **only the ready-to-install release files**. The user
interface is in German.

➡️ **Download: [latest release](../../releases/latest)** (`.deb`, `.exe`, `.apk`)

---

## When to use this tool / Use Cases

- **Switching streaming services** — move your whole library (hundreds of
  playlists, tens of thousands of songs) from e.g. Spotify to TIDAL or Deezer.
- **Using two services side by side** — copy selected playlists from Deezer to
  YouTube Music, from SoundCloud to Spotify, and so on (any direction between the
  five supported services).
- **Spotify without Premium** — use Spotify as a *source* by importing an export
  file, and as a *target* by generating a file for the Spicetify app
  [data-porter](https://github.com/Prog-Jacob/spicetify-apps).
- **Large libraries** — online converters usually cap free transfers; this tool
  has no limits (its file import was tested with a real library of 434 playlists
  and 98,517 songs).
- **Privacy-sensitive users** — no account at an intermediary service, no upload
  of your library, tokens stored encrypted on your own device.
- **Backups** — every transfer first writes a readable JSON backup of the source
  playlists.

## Quickstart

**Linux (Debian, Ubuntu, Linux Mint)** — download the newest `.deb` and install it:

```bash
curl -s https://api.github.com/repos/BjoernHa/Playlist-Converter-Tamilein/releases/latest \
  | grep -o 'https://[^"]*_all\.deb' | xargs curl -LO
sudo apt install ./tamilein_*_all.deb
tamilein            # opens the app at http://127.0.0.1:8000 in your browser
```

**Windows** — PowerShell (or just download `…-Setup.exe` from the release and double-click it; no admin rights needed):

```powershell
$r = Invoke-RestMethod https://api.github.com/repos/BjoernHa/Playlist-Converter-Tamilein/releases/latest
$a = $r.assets | Where-Object name -like '*-Setup.exe'
Invoke-WebRequest $a.browser_download_url -OutFile $a.name; Start-Process $a.name
```

**Android** — open the [latest release](../../releases/latest) on the phone,
download `tamilein-<version>.apk`, tap it and allow installing from this source.

Then, in the app:

1. **Connect** the services you want to use (each card explains how).
2. **Source** — load your playlists (or import a file) and tick the ones to copy.
3. **Target** — choose the destination service and analyse.
4. **Search** — every song is looked up on the target service.
5. **Review** — confirm or correct uncertain matches.
6. **Transfer** — the playlists are created; a report shows what was skipped.

## Comparison / Why this Tool?

| | Playlist-Converter | Typical online converters (e.g. TuneMyMusic, Soundiiz) |
|---|---|---|
| Where it runs | on your own device (Linux, Windows, Android) | in the provider's cloud |
| Transfer limits | none | free tiers are limited |
| Account at a third party | not needed | usually required |
| Access to your streaming accounts | stays on your device, stored encrypted | granted to the provider |
| Matching | ISRC first, then title, artist, duration and version tags; uncertain matches go to a review step | varies |
| Wrong-match protection | live/remix/karaoke/instrumental versions are never accepted automatically | varies |
| Spotify without Premium | source via file import, target via Spicetify file | depends on the provider |
| Backup before transfer | always (JSON, human-readable) | usually not |
| Resume after interruption | yes, already created playlists are not duplicated | varies |
| Price | free | full use usually needs a subscription |

What it does **not** do: Apple Music is not supported, and there is no automatic
background sync (every transfer is started by you).

## Frequently Asked Questions (FAQ)

**Which services are supported?**
Spotify, Deezer, TIDAL, YouTube Music and SoundCloud — in any direction.

**Is it free?**
Yes. There are no subscriptions, no limits and no ads.

**Do I need Spotify Premium?**
No. Since February 2026 Spotify only lets Premium users create the developer app
needed for its Web API. Without Premium, Playlist-Converter reads Spotify
playlists from an export file (Spicetify data-porter, Spotify's own data export,
or a CSV such as Exportify) and writes to Spotify by generating a file that the
Spicetify app data-porter imports in Spotify on a computer.

**How does it connect to Deezer, YouTube Music and SoundCloud?**
These services no longer offer open API registration. The app uses the same
interface as their websites; you paste one value from your browser (Deezer:
the `arl` cookie, SoundCloud: `oauth_token`, YouTube Music: request headers).
The app marks this as unofficial — a change on the service's side can break it.
TIDAL uses its official API with your own free developer app.

**How accurate is the matching?**
A song is taken over automatically only on an identical ISRC, or when title,
artist, duration and version tags all match closely. Everything else goes to a
review screen with two or three alternatives. Wrong matches are treated as worse
than missing ones.

**Where are my data and passwords stored?**
Only on your device: tokens in the system keyring, the Android Keystore or an
encrypted file; playlists and backups as local JSON files. Nothing is sent to
anyone except the music services themselves.

**Can I move my logins to another device?**
Yes — the app can save all connections into a password-protected, encrypted file
and read it in on another device.

**What happens if a transfer is interrupted?**
The plan is saved on disk. On the next start the app offers to continue;
playlists that were already created are completed, not created twice.

**Does the Android app need a computer?**
No. Each of the three versions (Linux, Windows, Android) runs completely on its
own.

**How do I update?**
The app checks for a newer version at start and offers it; there is also a
“search for updates” button at the bottom of the help page. No GitHub account is
needed.

**Is Apple Music supported?**
No, and it is not planned (it requires a paid Apple developer account).

**Is it open source?**
It is licensed under GPL-3.0-or-later. The source code is not published in this
repository; anyone who received a program from here can request it via an issue.

## Files in this repository

- [`README.md`](README.md) — this overview
- [`llms.txt`](llms.txt) — compact summary for AI assistants and search engines
- [`AGENTS.md`](AGENTS.md) — guide for autonomous agents that install or operate the app
- [Releases](../../releases/latest) — the installation files

---

## Deutsch — Kurzfassung

**Playlists frei übertragen** zwischen Spotify, Deezer, TIDAL, YouTube Music und
SoundCloud — ohne Limit, ohne Konto bei einem Zwischendienst, alles lokal auf
deinem Gerät. Die Dateien liegen unter **[Releases](../../releases/latest)**:
`.deb` für Linux Mint/Ubuntu/Debian (Doppelklick → „Paket installieren“),
`…-Setup.exe` für Windows (Doppelklick, kein Administrator nötig) und `.apk` für
Android (antippen, Installation aus dieser Quelle erlauben). Aktualisieren geht
im Programm selbst, ohne Anmeldung. Lizenz: GPL-3.0-or-later; den Quellcode gibt
es auf Anfrage über ein Issue.
