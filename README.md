# it-events-bonn

![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-deployed-green)  
![License: MIT](https://img.shields.io/badge/License-MIT-blue)

> **IT-Veranstaltungen in Bonn** – Eine schlanke Web-App, die alle IT-Vereins-Kalender in Bonn bündelt.  
Live-Website: https://bmmmm.github.io/it-events-bonn/

---

## 📌 Projektbeschreibung

Ein zentrales Dashboard, das alle öffentlichen .ics-Feeds von IT-Vereinen in Bonn zusammenführt. Zum Beispiel:
- [**Datenburg**](https://datenburg.org/) (der Hackspace in Bonn)
- [**Makerspace**](https://makerspacebonn.de/) (wie der Name sagt, ein Makerspace)
- [**Bitcircus101**](https://bitcircus101.de/) (ein Ort für Technik-Enthusiasten)

Die Seite:

- Lädt **`calendars.json`** zur Konfiguration (Name, ICS-Link, Farbe).
- Parsen der `.ics`-Feeds per **ical.js** und Darstellung in **FullCalendar**.
- Zeigt pro Verein bei Erfolg einen Zeitstempel `(Zuletzt: …)` aus **`last_updates.json`**.
- Markiert `(Nicht erreichbar)`, falls ein `.ics`-Feed beim Laden fehlschlägt.
- Läuft vollständig statisch auf **GitHub Pages**.

### Vorteile dieses Aufbaus

- **Erweiterbar per Pull Request**: Neue Vereine einfach zu `calendars.json` hinzufügen.  
- **Keine CORS-Probleme**: `.ics`-Feeds liegen unter `/calendars/` im gleichen Origin.  
- **Regelmäßige Aktualisierung**: Ein GitHub Actions Workflow aktualisiert `/calendars/*.ics` und `last_updates.json` automatisch.

---

## 🚀 Einrichtung & Deployment

### 1. Repository klonen

```bash
git clone https://github.com/bmmmm/it-events-bonn.git
cd it-events-bonn</code>