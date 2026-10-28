# Kommandozentrum

Persönliche PWA für den Alltag: Heute-Ansicht, Aufgaben (Muss/Soll/Kann, Frist, Kategorie Dienst/Lernen/Training/Privat), Termine mit Ort, „Navigation starten“ über Apple Karten und Fahrzeit-Puffer pro Ort mit automatischem „Losfahren um …“-Alarm.

**Alle Daten bleiben auf dem Gerät.** Es gibt keinen Server, kein Konto und keine Synchronisation.

## Funktionen

**Phase 1**
- **Heute**: nächster Aufbruch mit Countdown und Navigation, überfällige und heute fällige Aufgaben, weitere Muss-Aufgaben, Vorschau auf morgen
- **Aufgaben**: Priorität in drei Stufen (Muss/Soll/Kann), Kategorie, Frist mit optionaler Uhrzeit, Notizen, Filter nach Kategorie, Rückgängig beim Abhaken
- **Termine**: Datum, Beginn/Ende, Ort (mit Vorschlägen aus gespeicherten Orten), Fahrzeit-Puffer, Losfahr-Alarm, Kalenderdatei (.ics) mit Erinnerung zur Losfahrzeit
- **Orte**: Name, optionale Adresse für die Navigation, Puffer in Minuten; wird beim Speichern eines Termins automatisch gemerkt
- **Anhänge** pro Aufgabe/Termin in IndexedDB, nur lokal, mit Hinweis „keine eingestuften Dokumente“
- **Export/Import** als JSON, wahlweise mit Anhängen und optional verschlüsselt (AES-256-GCM, Schlüssel per PBKDF2-SHA-256 mit 600 000 Iterationen)
- Offline-fähig (Service Worker), dunkles Design, Bedienung mit dem Daumen (Tab-Leiste und Plus-Button unten, Eingabemasken als Bottom-Sheets)

**Phase 2**
- **Stundenplan (Plan → Tag)**: Zeitleiste mit Terminen und eingeplanten Aufgaben. Was sich zeitlich überschneidet, steht nebeneinander, z. B. Unterricht 07:30–16:00 und parallel „Mails 09:45–10:15“. Ein Tipp auf eine freie Stelle plant dort etwas ein.
- **Aufgaben einplanen**: Jede Aufgabe kann zusätzlich zur Frist ein Zeitfenster bekommen (Tag, von–bis) und erscheint dann im Stundenplan.
- **Jetzt-Karte auf „Heute“**: zeigt alles, was gerade läuft, auch mehreres parallel, mit Restzeit und dem nächsten Punkt.
- **Tagesbriefing**: erscheint beim ersten Öffnen am Tag (abschaltbar). Es zeigt Kennzahlen (Termine, fällig, überfällig, Muss offen), den ersten Aufbruch, den Ablauf mit Uhrzeiten, die wichtigsten Aufgaben und einen Ausblick auf morgen. Jederzeit erreichbar über das Listen-Symbol oben rechts.
- **Wochenplan (Plan → Woche)**: sieben Tage untereinander mit Kalenderwoche. Ein Tipp auf einen Tag öffnet seinen Stundenplan.
- **Routinen**:
  - Wiederkehrende Termine (z. B. werktags Unterricht, Di/Do Training) und Aufgaben (z. B. täglich Vokabeln, monatlich Bericht).
  - Möglich sind täglich, werktags, wöchentlich an bestimmten Tagen, monatlich oder alle n Tage, mit optionalem Enddatum.
  - Einzelne Serientermine lassen sich ausfallen lassen oder abweichend ändern.
  - Aufgaben-Routinen haben einen **Vorlauf**: Die Aufgabe erscheint so viele Tage vor dem Termin.
- **Vorlagen mit Vorlauf**: Eine Vorlage, z. B. „Prüfung“, legt zu einem Stichtag alle Vorbereitungsaufgaben mit passender Frist an (21 Tage vorher Lernplan, 7 Tage vorher Probeklausur …) und optional den Termin selbst. Drei Beispielvorlagen sind eingebaut.
- **Schnell-Eingabe** (Plus-Button): eine Zeile Text, eine Live-Vorschau, fertig. Siehe unten.
- **Backup-Erinnerung**, wenn die letzte Sicherung länger als 14 Tage zurückliegt.

**Phase 2.1**
- **Vorwarnung vor dem Losfahren**:
  - Einstellbar unter Mehr → Losfahr-Alarm: 5–30 Minuten vor der Losfahrzeit kommt ein kürzerer, gelber Hinweis „Fertig machen“. Standard sind 10 Minuten.
  - Die Vorwarnung erscheint auch auf der Aufbruch-Karte, im Briefing und im Termin.
  - Die .ics-Datei enthält sie als zweiten Kalender-Alarm.
- **Checklisten in Aufgaben**:
  - Unterpunkte mit Haken. Ein Haken wird sofort gespeichert, auch ohne „Speichern“.
  - In der Liste zeigt jede Aufgabe ihren Fortschritt (z. B. 2/5). Sind alle Punkte erledigt, bietet die App an, die Aufgabe abzuhaken.
  - Aufgaben-Routinen können eine Checkliste haben; jeder neue Eintrag startet mit leerer Liste.
  - In der Schnell-Eingabe: `Tasche packen: Ausweis, Laptop, Ladekabel`.
- **Abend-Check „Morgen vorbereiten“**:
  - Erscheint ab einer einstellbaren Uhrzeit (Standard 20:00) beim Öffnen der App, einmal pro Abend. Danach bleibt er als Karte auf „Heute“ erreichbar.
  - Inhalt: Aufbruch morgen mit Vorwarnung, Ablauf morgen, was vorzubereiten ist (mit Checklisten-Stand) und was heute noch offen ist.
  - Offene Aufgaben lassen sich mit einem Tipp auf morgen verschieben.
  - Läuft die App abends im Hintergrund und sind Benachrichtigungen erlaubt, kommt eine Systembenachrichtigung.

### Schnell-Eingabe

| Was | Schreibweise |
|---|---|
| Datum | `heute`, `morgen`, `übermorgen`, `Fr`, `Montag`, `30.10.`, `in 3 Tagen`, `nächste Woche` |
| Zeit | `18 Uhr`, `7:30`, `7:30-16:00`, `von 9 bis 11 Uhr` |
| Frist | `bis Fr`, `bis 31.12.`, `bis morgen 12 Uhr` |
| Ort (am Ende) | `@Schule` · Fahrzeit-Puffer: `+20` |
| Priorität | `!muss`, `!soll`, `!kann` (oder `!1`, `!2`, `!3`; `!` allein = Muss) |
| Kategorie | `#dienst`, `#lernen`, `#training`, `#privat` |
| Wiederholung | `täglich`, `werktags`, `jeden Di und Do`, `wöchentlich`, `monatlich`, `alle 3 Tage` |

Mit Uhrzeit oder Ort wird ein **Termin** daraus, mit Priorität, Kategorie oder Frist eine **Aufgabe**. Die Art lässt sich vor dem Speichern umschalten.

Beispiele:
- `werktags 7:30-16:00 Unterricht @Schule +35`
- `morgen 9:45-10:15 Mails beantworten #dienst !muss`: Aufgabe, im Stundenplan parallel zum Unterricht
- `Reiseantrag bis Fr !muss #dienst`
- `Training jeden Di und Do 18-19:30 Uhr @Halle`

Geplant für später: Kalender- und Erinnerungs-Anbindung (eine einzelne .ics-Datei lässt sich schon heute pro Termin erzeugen).

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
