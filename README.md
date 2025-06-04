# it-events-bonn

> **IT-Veranstaltungen in Bonn** – Eine schlanke, statische Web-App, die alle IT-Vereins-Kalender in Bonn anzeigt.
Live-Website: https://bmmmm.github.io/it-events-bonn/

---

## 📌 Projektbeschreibung

Diese Anwendung lädt alle `.ics`-Dateien aus dem Ordner `/calendars`, die per GitHub Actions-Workflow aktualisiert werden, und stellt sie in einer Listenansicht dar. Die wichtigsten Funktionen:

- **Statische HTML-Seite (`index.html`)**:
  - Liest zur Laufzeit das Objekt `last_updates.json` aus, um alle verfügbaren Kalendernamen zu ermitteln.
  - Baut daraus automatisch das interne `CALENDARS`-Array mit `{ name, url, color }`-Einträgen auf.
  - Parsen der `.ics`-Feeds per [ical.js](https://github.com/mozilla-comm/ical.js/) und Darstellung mit [FullCalendar](https://fullcalendar.io/).
  - List-Ansicht: Alle kommenden Termine aller Kalender werden in einer Monatsübersicht untereinander angezeigt.
  - Klick auf einen Termin öffnet ein Alert-Fenster mit Titel, Datum/Uhrzeit, Ort, URL und Beschreibung.

- **GitHub Actions Workflow (`ci_and_deploy.yml`)**:
  - Läuft stündlich (Cron) und bei Push auf `main` / manueller Ausführung.
  - Lädt über `calendars.json` alle konfigurierten `.ics`-Feeds nach `/calendars/*.ics`.
  - Vergleicht heruntergeladene Dateien mit den vorhandenen:
    - Wenn eine Datei neu oder geändert ist, wird sie verschoben, und `last_updates.json` wird mit Zeitstempeln aktualisiert.
    - Wenn mindestens eine Datei aktualisiert wurde, erfolgt ein automatischer Commit und Push.
  - Danach werden alle Dateien aus dem Repo (inkl. geänderter `.ics`-Dateien) auf den `gh-pages`-Branch deployed.

---

## 🚀 Einrichtung & Deployment

1. **Repository klonen**

   ```bash
   git clone https://github.com/bmmmm/it-events-bonn.git
   cd it-events-bonn
   ```

2. **Abhängigkeiten laden**

   Da die Seite rein statisch ist, sind keine Build-Schritte nötig. Einfach lokal den Webserver starten oder die `index.html` direkt öffnen.

3. **GitHub Actions**

   Der Workflow ist bereits in `.github/workflows/ci_and_deploy.yml` definiert. Er sorgt automatisch dafür, dass:
   - Alle `.ics`-Feeds aus `calendars.json` aktualisiert werden.
   - Bei Änderungen ein Commit ausgelöst wird.
   - Die Seite auf `gh-pages` deployed.

---

## ➕ Neuen Kalender hinzufügen

Um einen weiteren IT-Verein / Kalender einzubinden, ist nur folgender Schritt notwendig:

1. **Eintrag in `calendars.json` hinzufügen** (für den Download-Workflow):

   Öffne die Datei `calendars.json` und füge ein neues Objekt hinzu:

   ```json
   {
     "name": "neuerverein",
     "src": "https://beispiel.de/neuerverein.ics",
     "url": "calendars/neuerverein.ics",
     "color": "#4ade80"
   }
   ```

   - `name`: Ein eindeutiger Bezeichner (ohne Leer- oder Sonderzeichen), wird auch als Dateiname genutzt (`{name}.ics`).
   - `src`: Die URL zum `.ics`-Feed des Vereins.
   - `url`: Pfad unter `/calendars`, in dem die Datei gespeichert wird.
   - `color`: Hex-Code für die Farbe des Kalenders in FullCalendar.

Sobald `calendars.json` geändert ist und in `main` gemerged wird, lädt der CI-Workflow automatisch die entsprechende `.ics`-Datei in `/calendars/{name}.ics` und aktualisiert `last_updates.json`. Beim nächsten Laden von `index.html` erscheint der neue Kalender automatisch in der Liste.

---

## 📄 Beispiel Pull Request

Wenn du einen neuen Kalender hinzufügen willst, könntest du folgendes Repository forken und einen PR mit dieser Änderung öffnen:

1. **`calendars.json`**

   ```diff
   [
     {
       "name": "bc101",
       "src": "https://bitcircus101.de/bitcircus101.ics",
       "url": "calendars/bitcircus101.ics",
       "color": "#CC73E1"
     },
+    {
+      "name": "neuerverein",
+      "src": "https://beispiel.de/neuerverein.ics",
+      "url": "calendars/neuerverein.ics",
+      "color": "#4ade80"
+    }
   ]
   ```

Danach öffnest du einen Pull Request gegen den `main`-Branch. Sobald der PR gemerged wird, läuft der CI-Workflow und die neue Kalenderdatei wird automatisch im Repo unter `/calendars/neuerverein.ics` abgelegt. Anschließend taucht der Kalender auf der Live-Seite auf.

---

## 📜 Lizenz

MIT © Bartosz