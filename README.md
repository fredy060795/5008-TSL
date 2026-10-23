# TSL – Tactical Situation Log

Leichtgewichtiges Lagetagebuch für taktische Lagen.

## Funktion

- **Einheitenliste** (Sidebar): Einheiten hinzufügen / entfernen / verschieben; werden automatisch in den Dropdowns verfügbar
- **Neuer Eintrag**: Zeit (automatisch), Von, An, Art, Inhalt — mit Enter oder Schaltfläche eintragen
- **Arten**: Anforderung · Truppenmeldung · Feindmeldung · Meldung · Sonstiges
- **Lagetagebuch**: fortlaufend nummeriert, farbige Badges pro Art, scrollbar
- **Medical-Panel**: eigener Bereich für medizinische Notfälle mit Abfrageschema, Dokumentation und automatischem Log-Eintrag
- **Export**: CSV-Export aller Einträge
- **Persistenz**: Daten werden auf dem Server in einer SQLite-Datenbank gespeichert (Fallback: IndexedDB / localStorage)

## Nutzung (Server-Betrieb)

### Voraussetzungen

- [Node.js](https://nodejs.org/) (v18 oder neuer)

### Installation

```bash
npm install
```

### Starten

**Windows:**
```
start.bat
```

`start.bat` prüft fehlende Node-Abhängigkeiten automatisch und führt bei Bedarf `npm install` aus.

**Linux / macOS / CMD:**
```bash
node server.js
```

Der Server läuft auf **Port 5008**.  
Im Browser öffnen: [http://localhost:5008](http://localhost:5008)

Der Port kann über die Umgebungsvariable `PORT` geändert werden:
```bash
PORT=8080 node server.js
```

### Datenbank

Die Daten werden in `tsl.db` (SQLite) im Projektverzeichnis gespeichert.  
Die Datei wird beim ersten Start automatisch erstellt.

### Dateistruktur

```
TSL/
├── public/          # Frontend-Dateien (HTML, Bilder)
│   ├── index.html
│   ├── KARTE.jpg
│   └── SITREP.jfif
├── server.js        # Backend (Express + SQLite)
├── start.bat        # Windows-Startskript
├── package.json
└── tsl.db           # Datenbank (wird automatisch erstellt)
```

## Anbindung an LPU5 Tactical

LPU5 kann Funk-Mitschriften (Whisper), Notrufe, Meldungen (MEDEVAC, SALUTE, Lagemeldung)
und KI-Videoalarme automatisch ins Lagetagebuch schreiben (`POST /api/log` mit `ext: true`).

- In LPU5 `config.json`: `"tsl_url": "http://<TSL-Rechner>:5008"` (optional `"tsl_api_key"`).
- Optional hier: Umgebungsvariable `TSL_API_KEY` setzen – dann werden externe Einträge nur
  mit passendem Schlüssel (`X-API-Key`) angenommen.
- Externe Einträge sind markiert und bleiben erhalten, auch wenn ein TSL-Arbeitsplatz mit
  älterem Stand speichert. Geöffnete TSL-Seiten holen neue externe Einträge alle 5 Sekunden.
