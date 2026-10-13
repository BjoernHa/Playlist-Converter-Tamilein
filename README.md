# Playlist-Converter

**🇬🇧 Free playlist converter that runs on your own device: transfer playlists
and liked songs between Spotify, Deezer, TIDAL, YouTube Music, SoundCloud and
Apple Music — on Linux, Windows and Android, with no limits and no cloud.**

**🇩🇪 Kostenloser Playlist-Konverter, der auf deinem eigenen Gerät läuft:
Playlists und Lieblingssongs zwischen Spotify, Deezer, TIDAL, YouTube Music,
SoundCloud und Apple Music übertragen — unter Linux, Windows und Android, ohne
Limit und ohne Cloud.**

➡️ **[Download / Herunterladen](../../releases/latest)** — `.deb` (Linux) ·
`…-Setup.exe` (Windows) · `.apk` (Android) · [English](#english) ·
[Deutsch](#deutsch)

This repository holds only the release files of Playlist-Converter (project
name *Tamilein*). The app speaks **English and German** (language button at the
top right). License: GPL-3.0-or-later.

---

## English

**What it does**

- Copies whole playlists and liked songs in **any direction** between Spotify,
  Deezer, TIDAL, YouTube Music, SoundCloud and Apple Music — tested with 434
  playlists and 98,517 songs. Liked songs become a playlist or favorites.
- **Accurate matching:** identical ISRC first, then title, artist, duration and
  version tags. Uncertain matches go to a review step; live, remix, karaoke and
  instrumental versions are never accepted automatically.
- **Private:** runs on `127.0.0.1`; logins stay on your device, stored encrypted.
  No account at a third party, no upload of your library.
- **Safe:** JSON backup before every transfer; interrupted transfers resume
  without duplicate playlists.
- **Spotify without Premium:** *Connect without Premium* uses files — Spotify’s
  own data export (`my_spotify_data.zip`) or a
  [data-porter](https://github.com/Prog-Jacob/spicetify-apps) export as source,
  one data-porter file as target.
- **Update only:** after loading a playlist file you transferred before, one
  click appends just the new songs to the existing playlists in the target.
  Matches you picked by hand are remembered and suggested next time (you
  still confirm them).
- Built-in updates without a GitHub login. Three standalone apps: Linux `.deb`,
  Windows installer (no admin rights), Android `.apk` (no computer needed).

**Install**

```bash
# Linux (Debian, Ubuntu, Linux Mint)
curl -s https://api.github.com/repos/BjoernHa/Playlist-Converter-Tamilein/releases/latest \
  | grep -o 'https://[^"]*_all\.deb' | xargs curl -LO
sudo apt install ./tamilein_*_all.deb && tamilein   # opens http://127.0.0.1:8000
```

Windows: run `Tamilein-<version>-Setup.exe` from the
[latest release](../../releases/latest). Android: download
`tamilein-<version>.apk` on the phone and install it.

**How it works:** Connect → Source (pick playlists) → Analyze → Search →
Review uncertain matches → Transfer; analyzing and searching start on their
own. A report lists skipped songs.

| | Playlist-Converter | Online converters (TuneMyMusic, Soundiiz …) |
|---|---|---|
| Runs | on your device | in the provider’s cloud |
| Limits | none | free tiers are limited |
| Account at a third party | no | usually yes |
| Backup / resume | always / yes | varies |
| Price | free | subscription for full use |

**FAQ**

- *Which services?* Spotify, Deezer, TIDAL, YouTube Music, SoundCloud and
  Apple Music (needs an active Apple Music subscription; as the source, liked
  songs are the songs in your library — Apple offers no request that lists
  favorites; as the target, if you choose favorites instead of a playlist,
  they are added to the library and marked as favorites).
- *Free?* Yes — no subscription, no limits, no ads.
- *Deezer, YouTube Music, SoundCloud, Apple Music without an API key?* You
  paste one value from your browser (Deezer `arl` cookie, SoundCloud
  `oauth_token`, YouTube Music request copied as cURL, Apple Music `amp-api`
  request copied as cURL or the `media-user-token` cookie). Unofficial — a
  change on the service’s side can break it. TIDAL uses its official API with
  your free developer app.
- *Move logins to another device?* The *Import/Export* button at the top
  saves logins and remembered matches as a password-encrypted file; import it
  there.
- *Source code?* Not in this repository; request it via an issue (GPL).

---

## Deutsch

**Was es kann**

- Überträgt ganze Playlists und Lieblingssongs **in jede Richtung** zwischen
  Spotify, Deezer, TIDAL, YouTube Music, SoundCloud und Apple Music — erprobt
  mit 434 Playlists und 98.517 Songs. Lieblingssongs: als Playlist oder Favoriten.
- **Genaue Zuordnung:** zuerst gleicher ISRC, dann Titel, Interpret, Dauer und
  Fassung. Unsichere Treffer kommen in die Prüfansicht; Live-, Remix-, Karaoke-
  und Instrumentalfassungen werden nie automatisch übernommen.
- **Privat:** läuft auf `127.0.0.1`; Anmeldungen bleiben verschlüsselt auf
  deinem Gerät. Kein Konto bei einem Zwischendienst, kein Hochladen der
  Bibliothek.
- **Sicher:** JSON-Backup vor jeder Übertragung; abgebrochene Übertragungen
  setzen fort, ohne Playlists doppelt anzulegen.
- **Nur updaten:** Nach dem Einlesen einer schon einmal übertragenen
  Playlist-Datei hängt ein Klick nur die neuen Songs an die bisherigen
  Playlists im Ziel. Von Hand gewählte Zuordnungen werden gemerkt und beim
  nächsten Mal vorgeschlagen (bestätigt wird weiter von dir).
- **Spotify ohne Premium:** „Ohne Premium verbinden“ arbeitet mit Dateien —
  Spotifys eigener Datenexport oder ein data-porter-Export als Quelle, eine
  Datei für data-porter als Ziel.
- Aktualisieren im Programm, ohne GitHub-Anmeldung. Drei eigenständige
  Programme: Linux-`.deb`, Windows-Installer (ohne Administratorrechte),
  Android-`.apk` (kein Rechner nötig).

**Installieren:** unter **[Releases](../../releases/latest)** die passende
Datei laden — `.deb` doppelt anklicken → „Paket installieren“,
`…-Setup.exe` doppelt anklicken, `.apk` am Telefon antippen und die
Installation aus dieser Quelle erlauben.

**Ablauf:** Verbinden → Quelle (Playlists wählen) → Analysieren → Suchen →
unsichere Treffer prüfen → Übertragen; Analysieren und Suchen starten von
selbst. Ein Bericht nennt, was fehlt.

**Häufige Fragen:** Kosten: keine, kein Abo, kein Limit. Deezer, YouTube
Music, SoundCloud und Apple Music werden über ihre Websites angebunden (ein
Wert aus dem Browser; inoffiziell). Apple Music braucht ein aktives
Apple-Music-Abo; als Quelle gelten dort die Titel der Mediathek als
Lieblingssongs (Favoriten lassen sich nicht auflisten); als Ziel-Favoriten
kommen sie in die Mediathek und werden als Favorit markiert. Anmeldungen und gemerkte
Zuordnungen lassen sich als verschlüsselte Datei (Knopf „Import/Export“ oben)
auf ein anderes Gerät mitnehmen. Den Quellcode gibt es auf
Anfrage über ein Issue (GPL).

---

Files: [`llms.txt`](llms.txt) (summary for AI assistants) ·
[`AGENTS.md`](AGENTS.md) (guide for agents and automation) ·
[Releases](../../releases/latest)

<sub>Keywords: playlist converter, transfer playlists, Spotify to TIDAL, Spotify
to Deezer, Spotify to Apple Music, Apple Music to Spotify, Deezer to Spotify,
YouTube Music to Spotify, SoundCloud playlist export, Soundiiz alternative,
TuneMyMusic alternative, free, offline, privacy · Playlist übertragen,
Playlist-Konverter, Spotify zu TIDAL, Spotify zu Deezer, Spotify zu Apple Music,
kostenlos, ohne Limit</sub>
