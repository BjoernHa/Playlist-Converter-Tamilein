# Playlist-Converter

**🇬🇧 Free playlist converter that runs on your own device: transfer playlists
and liked songs between Spotify, Deezer, TIDAL, YouTube Music, SoundCloud and
Apple Music — on Linux, Windows, Android and iPhone, with no limits and no cloud.**

**🇩🇪 Kostenloser Playlist-Konverter, der auf deinem eigenen Gerät läuft:
Playlists und Lieblingssongs zwischen Spotify, Deezer, TIDAL, YouTube Music,
SoundCloud und Apple Music übertragen — unter Linux, Windows, Android und auf dem
iPhone, ohne Limit und ohne Cloud.**

➡️ **[Download / Herunterladen](../../releases/latest)** — `.deb` (Linux) ·
`…-Setup.exe` (Windows) · `.apk` (Android) · `.ipa` (iPhone) ·
[English](#english) · [Deutsch](#deutsch)

This repository holds only the release files of Playlist-Converter (project
name *Tamilein*). The app speaks **English and German** (language button at the
top right). License: GPL-3.0-or-later.

---

## English

**What it does**

- Copies whole playlists and liked songs in **any direction** between Spotify, Deezer, TIDAL, YouTube Music, SoundCloud and Apple Music — tested with 434 playlists and 98,517 songs. Liked songs become a playlist or favorites.
- **Accurate matching:** identical ISRC first, then title, artist, duration and version tags. Uncertain matches go to a review step; live, remix, karaoke and instrumental versions are never accepted automatically.
- **Private:** runs on `127.0.0.1`; logins stay on your device, stored encrypted. No account at a third party, no upload of your library.
- **Safe:** JSON backup before every transfer; interrupted transfers resume without duplicate playlists.
- **Spotify without Premium:** *Connect without Premium* uses files — Spotify’s own data export (`my_spotify_data.zip`) or a [data-porter](https://github.com/Prog-Jacob/spicetify-apps) export as source, one data-porter file as target.
- **Update only:** after loading a playlist file you transferred before, one click appends just the new songs to the existing playlists in the target. Matches you picked by hand are remembered and suggested next time (you still confirm them).
- **Live overview while searching:** see how many songs are matched, still need review or weren’t found — and decide the first uncertain ones while the search runs.
- **Follow instead of copy:** within the same service (e.g. Spotify to another Spotify account), playlists you only follow can be followed again instead of rebuilt — you get a list of URLs to follow them; following automatically is being tried in a test version first.
- Built-in updates without a GitHub login. Four standalone apps: Linux `.deb`, Windows installer (no admin rights), Android `.apk` and iPhone/iPad `.ipa` (no computer needed to run them).

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

**iPhone / iPad (iOS 16+):** `tamilein-<version>.ipa` is **unsigned** — **SideStore** signs it on the device with your **free Apple ID** (no paid developer account). You need a computer for the setup — Windows, macOS or Linux (e.g. Linux Mint) — and again if the pairing file or SideStore itself expires; plus a passcode on the iPhone, an Apple ID and Wi-Fi (mobile data isn’t enough). SoundCloud and YouTube Music are connected on a computer.

1. iPhone: install **LocalDevVPN** from the App Store, tap **Connect** (allow the VPN configuration, enter the passcode). It must be on whenever SideStore installs, updates or refreshes apps.
2. Computer: install iloader (https://iloader.app). Linux: `sudo apt install usbmuxd`, then https://github.com/nab138/iloader/releases/latest/download/iloader-linux-amd64.deb (no iTunes needed). macOS: https://github.com/nab138/iloader/releases/latest/download/iloader-darwin-universal.dmg. Windows (8 or later, 64-bit, not Windows 10 on ARM): first iTunes straight from Apple (https://www.apple.com/itunes/download/win64; otherwise from the Microsoft Store; if neither works, uninstall iTunes and *Apple Mobile Device Support*, then install the *Apple Devices* app from the Microsoft Store), then the MSI https://github.com/nab138/iloader/releases/latest/download/iloader-windows-x64.msi.
3. Connect the iPhone by USB, tap **Trust**, enter the passcode. In iloader sign in with your Apple ID (case-sensitive; needn’t be the iPhone’s), pick the iPhone → under *Installers* click **SideStore (Stable)**.
4. iPhone: Settings → General → **VPN & Device Management** → your Apple ID under *Developer App* → trust it → **Allow & Restart** (iOS 16–17: **Trust**); then Settings → Privacy & Security → **Developer Mode** on and tap **Restart** in the alert; after the restart confirm the next alert: swipe up, **Enable**, enter the passcode.
5. LocalDevVPN → **Connect**; open SideStore, sign in with the same Apple ID; **My Apps** → tap **7 DAYS** next to SideStore once and confirm the new certificate (**Refresh Now**).
6. SideStore → **Sources** → **+** → paste and add `https://github.com/BjoernHa/Playlist-Converter-Tamilein/releases/latest/download/altstore-quelle.json` (Wi-Fi and LocalDevVPN on), then tap **FREE** next to Playlist-Converter. Only installed this way does SideStore show new versions as updates under **My Apps**. Fallback without the source: download the `.ipa` in Safari → **Files** → **Share** → **SideStore** — SideStore then shows **no** updates; if you add the source later, install from it once via **FREE** (same identifier, data should stay — not tried on a device).

Limits of a free Apple ID: each app runs **7 days** — SideStore refreshes it in the background, or tap the day counter in **My Apps** (Wi-Fi and LocalDevVPN on); an expired app won’t open until refreshed, its data stays. SideStore can only refresh itself while it still opens — open it at least every 7 days; if it has expired, reinstall it with iloader (without deleting it first). At most **3** such apps at a time (SideStore counts, so 2 are left) and **10** app IDs per 7 days. If the pairing file expires (e.g. after an iOS update): iloader → *Manage Pairing File* → *Place* next to SideStore. iOS pauses apps in the background — keep the app open during long transfers. SoundCloud and YouTube Music: connect on a computer, then *Import/Export* (⇅) → password → *Export*, and in the app *Import/Export* → same password → *Import*. With a different Apple ID the app is a new one (no logins, backups or remembered matches) — export first.
Alternative: AltStore Classic with AltServer on the computer (Windows or macOS; on Windows iTunes and iCloud from Apple, not the Microsoft Store; refresh every 7 days on the same Wi-Fi; source link `altstore://source?url=` + the address above). AltStore PAL doesn’t work (only apps notarized by Apple). Not yet tried on a real iPhone — checked in the simulator only.

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

- *Which services?* Spotify, Deezer, TIDAL, YouTube Music, SoundCloud and Apple Music (needs an active Apple Music subscription; as the source, liked songs are the songs in your library — Apple offers no request that lists favorites; as the target, if you choose favorites instead of a playlist, they are added to the library and marked as favorites).
- *Free?* Yes — no subscription, no limits, no ads.
- *Deezer, YouTube Music, SoundCloud, Apple Music without an API key?* You paste one value from your browser (Deezer `arl` cookie, SoundCloud `oauth_token`, YouTube Music request copied as cURL, Apple Music `amp-api` request copied as cURL or the `media-user-token` cookie); for Apple Music (on a computer, in the Android app and on the iPhone) and Deezer (on a computer and on the iPhone) a login window reads it. Unofficial. TIDAL: official API with your free developer app (connecting via a code without it requires TIDAL Premium).
- *Move logins to another device?* The *Import/Export* button at the top saves logins and remembered matches as a password-encrypted file; import it there.
- *Source code?* Not in this repository; request it via an issue (GPL).

---

## Deutsch

**Was es kann**

- Überträgt ganze Playlists und Lieblingssongs **in jede Richtung** zwischen Spotify, Deezer, TIDAL, YouTube Music, SoundCloud und Apple Music — erprobt mit 434 Playlists und 98.517 Songs. Lieblingssongs: als Playlist oder Favoriten.
- **Genaue Zuordnung:** zuerst gleicher ISRC, dann Titel, Interpret, Dauer und Fassung. Unsichere Treffer kommen in die Prüfansicht; Live-, Remix-, Karaoke- und Instrumentalfassungen werden nie automatisch übernommen.
- **Privat:** läuft auf `127.0.0.1`; Anmeldungen bleiben verschlüsselt auf deinem Gerät. Kein Konto bei einem Zwischendienst, kein Hochladen der Bibliothek.
- **Sicher:** JSON-Backup vor jeder Übertragung; abgebrochene Übertragungen setzen fort, ohne Playlists doppelt anzulegen.
- **Nur updaten:** Nach dem Einlesen einer schon einmal übertragenen Playlist-Datei hängt ein Klick nur die neuen Songs an die bisherigen Playlists im Ziel. Von Hand gewählte Zuordnungen werden gemerkt und beim nächsten Mal vorgeschlagen (bestätigt wird weiter von dir).
- **Spotify ohne Premium:** „Ohne Premium verbinden“ arbeitet mit Dateien — Spotifys eigener Datenexport oder ein data-porter-Export als Quelle, eine Datei für data-porter als Ziel.
- **Live-Übersicht beim Suchen:** wie viele Songs zugeordnet sind, was du noch prüfen musst und was fehlt — die ersten unsicheren entscheidest du schon während der Suche.
- **Folgen statt kopieren:** innerhalb desselben Dienstes (etwa Spotify auf ein anderes Spotify-Konto) lassen sich Playlists, denen du nur folgst, wieder folgen statt nachbauen — du bekommst eine Liste mit Links; automatisches Folgen wird erst in einer Testfassung erprobt.
- Aktualisieren im Programm, ohne GitHub-Anmeldung. Vier eigenständige Programme: Linux-`.deb`, Windows-Installer (ohne Administratorrechte), Android-`.apk` und iPhone/iPad-`.ipa` (zum Benutzen kein Rechner nötig).

**Installieren:** unter **[Releases](../../releases/latest)** die passende
Datei laden — `.deb` doppelt anklicken → „Paket installieren“,
`…-Setup.exe` doppelt anklicken, `.apk` am Telefon antippen und die
Installation aus dieser Quelle erlauben.

**iPhone / iPad (ab iOS 16):** die `.ipa` ist **unsigniert** — **SideStore** signiert sie auf dem Gerät mit deiner **kostenlosen Apple-ID** (kein bezahltes Entwicklerkonto). Einen Rechner brauchst du zum Einrichten — Windows, macOS oder Linux (z. B. Linux Mint) — und wieder, wenn die Pairing-Datei oder SideStore selbst abläuft; dazu Code-Sperre am iPhone, eine Apple-ID und WLAN (Mobilfunk reicht nicht). SoundCloud und YouTube Music verbindest du am Rechner. SideStore ist nur englisch — Menünamen fett, Übersetzung in Klammern.

1. iPhone: **LocalDevVPN** aus dem App Store installieren, **Connect** tippen (bei „VPN-Konfigurationen hinzufügen“ → Erlauben, Code eingeben). Sie muss an sein, wann immer SideStore Apps installiert, aktualisiert oder auffrischt.
2. Rechner: iloader installieren (https://iloader.app). Linux Mint: `sudo apt install usbmuxd`, dann https://github.com/nab138/iloader/releases/latest/download/iloader-linux-amd64.deb (kein iTunes nötig). macOS: https://github.com/nab138/iloader/releases/latest/download/iloader-darwin-universal.dmg. Windows (ab Windows 8, 64 Bit, nicht Windows 10 auf ARM): erst iTunes direkt von Apple (https://www.apple.com/itunes/download/win64; sonst aus dem Microsoft Store; geht beides nicht: iTunes und „Apple Mobile Device Support“ deinstallieren, dann die App „Apple-Geräte“ aus dem Microsoft Store), dann das MSI https://github.com/nab138/iloader/releases/latest/download/iloader-windows-x64.msi.
3. iPhone per USB anschließen, **Vertrauen** tippen, Code eingeben. In iloader mit der Apple-ID anmelden (Groß-/Kleinschreibung beachten; muss nicht die des iPhones sein), das iPhone wählen → unter „Installer“ (Installers) auf **SideStore (Stable)** klicken.
4. iPhone: Einstellungen → Allgemein → **VPN & Geräteverwaltung** → unter „Entwickler-App“ die Apple-ID → vertrauen → **Erlauben & Neustarten** (iOS 16–17: **Vertrauen**); dann Einstellungen → Datenschutz & Sicherheit → **Entwicklermodus** einschalten und im Hinweis **Neustart** tippen; nach dem Neustart den nächsten Hinweis bestätigen: nach oben wischen, **Aktivieren**, Code eingeben.
5. LocalDevVPN → **Connect**; SideStore öffnen, mit derselben Apple-ID anmelden; **My Apps** (Meine Apps) → einmal auf **7 DAYS** neben SideStore tippen und das neue Zertifikat bestätigen (**Refresh Now**).
6. SideStore → **Sources** (Quellen) → **+** → `https://github.com/BjoernHa/Playlist-Converter-Tamilein/releases/latest/download/altstore-quelle.json` einfügen und hinzufügen (WLAN und LocalDevVPN an), dann bei Playlist-Converter auf **FREE** tippen. Nur so installiert meldet SideStore neue Fassungen unter **My Apps** als Update. Notlösung ohne Quelle: die `.ipa` in Safari laden → **Dateien** → **Teilen** → **SideStore** — dann zeigt SideStore **keine** Updates; wer die Quelle später hinzufügt, installiert dort einmal über **FREE** (gleiche Kennung, die Daten sollten bleiben — am Gerät nicht erprobt).

Grenzen der kostenlosen Apple-ID: jede App läuft **7 Tage** — SideStore frischt sie im Hintergrund auf, sonst in **My Apps** (Meine Apps) auf die Tage-Anzeige tippen (WLAN und LocalDevVPN an); eine abgelaufene App startet erst nach dem Auffrischen wieder, ihre Daten bleiben. SideStore frischt sich nur auf, solange es noch startet — spätestens alle 7 Tage öffnen; ist es abgelaufen, mit iloader erneut installieren (ohne es vorher zu löschen). Höchstens **3** solcher Apps gleichzeitig (SideStore zählt mit, es bleiben 2) und **10** App-IDs je 7 Tage. Läuft die Pairing-Datei ab (etwa nach einem iOS-Update): iloader → „Pairing-Datei verwalten“ (Manage Pairing File) → „Ablegen“ (Place) neben SideStore. iOS hält Apps im Hintergrund an — lange Übertragungen mit offener App laufen lassen. SoundCloud und YouTube Music: am Rechner verbinden, dann „Import/Export“ (⇅) → Passwort → „Exportieren“ und in der App „Import/Export“ → dasselbe Passwort → „Importieren“. Mit einer anderen Apple-ID ist es eine neue App (ohne Anmeldungen, Backups, gemerkte Zuordnungen) — vorher exportieren.
Alternative: AltStore Classic mit AltServer am Rechner (Windows oder macOS; unter Windows iTunes und iCloud von Apple, nicht aus dem Microsoft Store; Auffrischen alle 7 Tage im selben WLAN; Quelle per `altstore://source?url=` + obige Adresse). AltStore PAL geht nicht (nur von Apple notarisierte Apps). Am echten iPhone noch nicht erprobt — geprüft nur im Simulator.

**Ablauf:** Verbinden → Quelle (Playlists wählen) → Analysieren → Suchen →
unsichere Treffer prüfen → Übertragen; Analysieren und Suchen starten von
selbst. Ein Bericht nennt, was fehlt.

**Häufige Fragen:** Kosten: keine, kein Abo, kein Limit. Deezer, YouTube Music,
SoundCloud und Apple Music werden über ihre Websites angebunden (ein Wert aus
dem Browser; inoffiziell — bei Apple Music (am Rechner, in der Android-App und auf
dem iPhone) und Deezer (am Rechner und auf dem iPhone) liest ihn ein
Anmeldefenster selbst aus). TIDAL: eigene, kostenlose App (direkt per Code
erfordert TIDAL Premium). Apple Music braucht ein aktives Apple-Music-Abo; als
Quelle gelten dort die Titel der Mediathek als Lieblingssongs (Favoriten lassen
sich nicht auflisten); als Ziel-Favoriten kommen sie in die Mediathek und werden
als Favorit markiert. Anmeldungen und gemerkte Zuordnungen lassen sich als
verschlüsselte Datei (Knopf „Import/Export“ oben) auf ein anderes Gerät
mitnehmen. Den Quellcode gibt es auf Anfrage über ein Issue (GPL).

---

Files: [`llms.txt`](llms.txt) (summary for AI assistants) ·
[`AGENTS.md`](AGENTS.md) (guide for agents and automation) ·
[Releases](../../releases/latest)

<sub>Keywords: playlist converter, transfer playlists, Spotify to TIDAL, Spotify to Deezer, Spotify to Apple Music, Apple Music to Spotify, Deezer to Spotify, YouTube Music to Spotify, SoundCloud playlist export, Soundiiz alternative, TuneMyMusic alternative, iPhone, iOS, SideStore, free, offline, privacy · Playlist übertragen, Playlist-Konverter, Spotify zu TIDAL, Spotify zu Deezer, Spotify zu Apple Music, kostenlos, ohne Limit</sub>
