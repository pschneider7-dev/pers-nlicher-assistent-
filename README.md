# Kommandozentrum

Persönliche PWA für den Alltag: Heute-Ansicht, Aufgaben (Muss/Soll/Kann, Frist, Kategorie Dienst/Lernen/Training/Privat), Termine mit Ort, „Navigation starten“ über Apple Karten und Fahrzeit-Puffer pro Ort mit automatischem „Losfahren um …“-Alarm.

**Alle Daten bleiben auf dem Gerät.** Es gibt keinen Server, kein Konto und keine Synchronisation.

## Phase 1 (dieser Stand)

- **Heute**: nächster Aufbruch mit Countdown und Navigation, Termine heute, überfällige und heute fällige Aufgaben, weitere Muss-Aufgaben, Vorschau auf morgen
- **Aufgaben**: Priorität, Kategorie, Frist mit optionaler Uhrzeit, Notizen, Filter nach Kategorie, Rückgängig beim Abhaken
- **Termine**: Datum, Beginn/Ende, Ort (mit Vorschlägen aus gespeicherten Orten), Fahrzeit-Puffer, Losfahr-Alarm, Kalenderdatei (.ics) mit Erinnerung zur Losfahrzeit
- **Orte**: Name, optionale Adresse für die Navigation, Puffer in Minuten; wird beim Speichern eines Termins automatisch gemerkt
- **Anhänge** pro Aufgabe/Termin in IndexedDB, nur lokal, mit Hinweis „keine eingestuften Dokumente“
- **Export/Import** als JSON, wahlweise mit Anhängen und optional verschlüsselt (AES-256-GCM, Schlüssel per PBKDF2-SHA-256 mit 600 000 Iterationen)
- Offline-fähig (Service Worker), dunkles Design, Bedienung mit dem Daumen (Tab-Leiste und Plus-Button unten, Eingabemasken als Bottom-Sheets)

Geplant für später: Routinen, Vorlagen mit Vorlauf, Wochenplan, Schnell-Eingabe, Kalender- und Erinnerungs-Anbindung.

### Hinweis zum Alarm auf dem iPhone

Eine Web-App kann ohne Server keine Benachrichtigungen planen. Der Alarm (Ton, Vollbild-Hinweis, Systembenachrichtigung) löst deshalb nur aus, solange die App geöffnet ist oder kurz im Hintergrund läuft. Verpasste Alarme werden bis zu 30 Minuten danach beim Öffnen nachgeholt. Wenn du dich auf die Erinnerung verlassen musst, tippe im Termin auf **„In Kalender (.ics)“**: Die Datei enthält eine Erinnerung zur Losfahrzeit, und die übernimmt dann der iOS-Kalender.

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | gesamte App (HTML, CSS und JS in einer Datei) |
| `manifest.webmanifest` | PWA-Manifest |
| `sw.js` | Service Worker (cacht nur die App-Dateien) |
| `icons/` | App-Icons |
| `.nojekyll` | GitHub Pages liefert die Dateien unverändert aus |

Alle Pfade sind relativ (`./`). Die App läuft deshalb in jedem Unterpfad, z. B. `https://<nutzer>.github.io/pers-nlicher-assistent-/`.

**Nach jeder Änderung** `CACHE_VERSION` in `sw.js` erhöhen, sonst sehen installierte Geräte die neue Version nicht. Die App zeigt dann „Neue Version verfügbar – Aktualisieren“ an.

## Deployment über GitHub Pages

1. Diesen Branch nach `main` mergen.
2. Unter *Settings → Pages* bei *Source* „Deploy from a branch“ wählen, dann Branch `main` und Ordner `/ (root)`.
3. Die Seite auf dem iPhone in Safari öffnen, dann *Teilen → Zum Home-Bildschirm*.
4. In der App unter *Mehr → Losfahr-Alarm* die Benachrichtigungen erlauben. Auf dem iPhone geht das erst, nachdem die App zum Home-Bildschirm hinzugefügt wurde.

## Datenschutz: Was ist online, was nicht?

| Was | Wo | Öffentlich? |
|---|---|---|
| Programmcode (diese Dateien) | GitHub-Repo und GitHub Pages | ja; enthält keine persönlichen Daten |
| Aufgaben, Termine, Orte, Einstellungen | `localStorage` im Browser des Geräts | nein |
| Anhänge | IndexedDB im Browser des Geräts | nein |
| Backups (JSON) | dort, wo du die Datei speicherst | nein |

Technische Absicherungen:

- **Content-Security-Policy** mit `connect-src 'none'`: Die Seite kann technisch keine Netzwerkanfragen absetzen (kein `fetch`, XHR, Beacon oder WebSocket). Es werden keine externen Skripte, Schriften oder Bilder geladen.
- **Kein Tracking**, keine Analyse und keine Cookies.
- `referrer: no-referrer` und `robots: noindex`.
- Der **Service Worker** cacht nur die eigenen App-Dateien und fasst Anfragen an fremde Adressen nicht an.
- Angehängte Dateien vom Typ HTML oder SVG werden nie im Ursprung der App dargestellt. Eine Vorschau gibt es nur für Bilder, PDF und Text.
- Die **`.gitignore`** schließt Backup-Dateien, `.ics`, PDF- und Office-Dateien aus. So landen sie nicht versehentlich im öffentlichen Repo.
- **„Navigation starten“** übergibt das Ziel erst beim Antippen an Apple Karten. Das ist der einzige Moment, in dem Daten das Gerät verlassen, und der Anstoß kommt immer von dir.

Diese Einstellungen in GitHub kann die App nicht selbst setzen. Bitte einmal prüfen:

- *Settings → Emails*: „Keep my email addresses private“ **und** „Block command line pushes that expose my email“ aktivieren.
- Das Repo ist **öffentlich**, weil GitHub Pages im kostenlosen Tarif ein öffentliches Repo voraussetzt. Deshalb nie echte Daten, Backups oder Screenshots mit Inhalten committen.
- Die Pages-Adresse enthält den GitHub-Nutzernamen.
