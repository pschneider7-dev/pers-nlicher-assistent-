# Kommandozentrum

Schlichte persönliche App fürs iPhone (PWA): Was steht heute an, welche Termine kommen, welche Aufgaben sind offen, dazu oft genutzte Links.

**Alle Daten bleiben auf dem Gerät.** Es gibt keinen Server, kein Konto und keine Synchronisation.

## Funktionen

- **Heute**: alles von heute in einer Liste, in dieser Reihenfolge:
  - überfällige Aufgaben
  - ganztägige Termine
  - Termine und Aufgaben nach Uhrzeit
  - Aufgaben ohne Uhrzeit

  Darunter ein kurzer Ausblick auf morgen. Erledigtes bleibt bis zum Abend durchgestrichen stehen.
- **Termine**: chronologische Liste, nach Tagen gruppiert. Vergangene Termine lassen sich einblenden.
- **Aufgaben**:
  - Priorität Muss/Soll/Kann, Kategorie Dienst/Lernen/Training/Privat, Fälligkeit mit optionaler Uhrzeit.
  - Bei Bedarf zusätzlich Ort, Checkliste, Notiz und Anhang. Diese Felder erscheinen erst, wenn man sie antippt.
- **Ort**: Termine und Aufgaben können einen Ort haben. „Route“ öffnet Apple Karten mit der Route vom aktuellen Standort. Die Fahrzeit rechnet Apple Karten selbst aus.
- **Links**: oft genutzte Seiten, z. B. die Deutsche Bahn, mit einem Tipp öffnen (unter „Mehr“).
- **Anhänge** pro Aufgabe oder Termin, nur lokal in IndexedDB. Hinweis: keine eingestuften Dokumente ablegen.
- **Sichern / Wiederherstellen** als JSON-Datei, wahlweise mit Anhängen und mit Passwort verschlüsselt (AES-256-GCM, PBKDF2-SHA-256).
- **Kalender**: Ein Termin lässt sich als .ics in den iPhone-Kalender übernehmen.
- **Plus-Button**: auf „Heute“ Auswahl zwischen Aufgabe und Termin, sonst direkt das Passende (Termin, Aufgabe oder Link).
- Offline-fähig, dunkles Design, für die Bedienung mit dem Daumen gebaut.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | gesamte App (HTML, CSS und JS in einer Datei) |
| `manifest.webmanifest` | PWA-Manifest |
| `sw.js` | Service Worker (cacht nur die App-Dateien) |
| `icons/` | App-Icons |
| `.nojekyll` | GitHub Pages liefert die Dateien unverändert aus |

Alle Pfade sind relativ (`./`). Die App läuft deshalb in jedem Unterpfad.

## Updates

**Nach jeder Änderung `CACHE_VERSION` in `sw.js` erhöhen.** Dann gilt:

- Die installierte App findet die neue Version beim nächsten Start oder spätestens eine Stunde nach dem Zurückholen aus dem Hintergrund.
- Sie meldet „Neue Version verfügbar – Aktualisieren“.
- Die Daten bleiben erhalten.

## Deployment über GitHub Pages

1. Änderungen nach `main` mergen.
2. *Settings → Pages*: „Deploy from a branch“, Branch `main`, Ordner `/ (root)`.
3. Die Seite auf dem iPhone in **Safari** öffnen: *Teilen → Zum Home-Bildschirm*.

## Datenschutz

| Was | Wo | Öffentlich? |
|---|---|---|
| Programmcode | GitHub-Repo und GitHub Pages | ja; enthält keine persönlichen Daten |
| Aufgaben, Termine, Links | `localStorage` im Browser des Geräts | nein |
| Anhänge | IndexedDB im Browser des Geräts | nein |
| Sicherungen | dort, wo du die Datei speicherst | nein |

- **Content-Security-Policy** mit `connect-src 'none'`: Die Seite kann technisch keine Netzwerkanfragen absetzen. Es werden keine externen Skripte, Schriften oder Bilder geladen.
- Kein Tracking und keine Cookies. Dazu `noindex` und `no-referrer`.
- „Route“ und Links öffnen erst beim Antippen Apple Karten bzw. die jeweilige Seite. Nur dann verlassen Daten (Ziel bzw. Adresse) das Gerät. Erlaubt sind nur Web-, Telefon- und Mail-Adressen.
- Angehängte HTML- oder SVG-Dateien werden nie im Ursprung der App dargestellt.
- Die `.gitignore` schließt Sicherungen, `.ics`-, PDF- und Office-Dateien aus.
- Das Repo ist öffentlich, weil GitHub Pages im kostenlosen Tarif das voraussetzt. Deshalb nie echte Daten committen.
