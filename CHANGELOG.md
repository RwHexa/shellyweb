# Changelog – SmartHomeRw (Shelly Controller)

Browserbasierte Anwendung zur Steuerung von Shelly Gen3 Geräten via MQTT
über HiveMQ Cloud. Läuft im Desktop- und Android-Browser.

---

## [v1.2.0] – 28.04.2026 (aktuell)

### Hinzugefügt
- **Grundriss-Seite** als eigenständige HTML-Datei (`wohnung.html`)
  - Eigenes Wohnung-Bild mit klickbaren Lampen-Overlays
  - Wohnzimmer-Lampe steuert echten Shelly (über localStorage geteilt)
  - Drei Demo-Lampen (Schlafzimmer, Bad, Küche) zum Testen
  - Eigene MQTT-Verbindung (unabhängig von TMS Web Core)
  - Auto-Connect beim Seitenaufruf via gespeicherten Zugangsdaten
- **Link-Navigation** im Header
  - "🏠 Grundriss" auf der Steuerungs-Seite
  - "⚡ Steuerung →" auf der Grundriss-Seite
- Status-Anzeige (Online/Offline/Watt) auch auf Grundriss-Seite

### Behoben
- **localStorage-Bug** in `Unit1.pas` `SaveDevices`
  - Pascal-Variable `JSON: string` kollidierte mit JS-eingebautem `JSON`-Objekt
  - Result: localStorage enthielt `[object JSON]` statt der Geräteliste
  - Fix: Variable umbenannt zu `JSONStr`
  - Geräte werden jetzt korrekt persistiert und über Seiten geteilt

### Architektur
- Zwei eigenständige HTML-Seiten teilen sich Daten über localStorage
  - `Project1.html` = TMS Web Core Steuerungs-App
  - `wohnung.html` = pure HTML-Seite mit eigenem MQTT-Client
- Lösung umgeht TMS Web Core Form-Layout-Probleme mit Bildern komplett

### Hinweise
- Beim Wechsel zwischen Steuerungs- und Grundriss-Seite wird MQTT-Verbindung
  neu aufgebaut (~1-2 Sekunden) – technisch bedingt durch Browser-Seitenwechsel

---

## [v1.1.2] – 27.04.2026

### Aufgeräumt
- `MQTTBridge.pas`: Debug-Logging vollständig entfernt
- Saubere Produktivversion ohne `[DBG]`-Meldungen im MQTT-Log

### Hinweis
- Topic-Vergleich ist case-sensitive: Device-IDs immer in **Kleinbuchstaben** eingeben
  (z.B. `shelly1pmminig3-34b7dac507e0`)

---

## [v1.1.1 DEBUG] – 27.04.2026

### Hinzugefügt (temporär für Diagnose)
- Ausführliches Debug-Logging in `MQTTBridge.pas`
  - Subscribe-Erfolg/-Fehler mit Topic und QoS
  - Alle eingehenden MQTT-Pakete (`packetreceive`)
  - Connect/Reconnect/Close/Offline Events
  - Publish-Bestätigungen
- Hat geholfen den Case-Sensitive-Bug bei Device-ID auf Android zu identifizieren

---

## [v1.1.0] – 27.04.2026

### Behoben
- **Android Chrome**: Status-Updates wurden nicht angezeigt
- `pas.MQTTBridge.TMQTTState.msConnected` durch direkte Enum-Werte (Zahlen) ersetzt
  → Desktop Chrome war tolerant, Android Chrome nicht
- Message-Callback: `if (self.FOnMessage) self.FOnMessage(...)` statt `var cb = ...`
- QoS von 1 auf 0 reduziert (zuverlässiger auf mobilen Verbindungen)
- `try/catch` um Message-Handler verhindert stille Abstürze

### Behoben (Build-Fehler)
- `this.FClient.end(true)` → `this.FClient["end"](true)`
  (`end` ist reserviertes Wort in pas2js)
- CRLF Zeilenenden für Delphi/pas2js Compiler

---

## [v1.0.5] – 27.04.2026

### Hinzugefügt
- **Versionsanzeige im Header** (klein in Akzentfarbe unter dem Titel)
- Versionsschema definiert:
  - `v1.0.x` = nur HTML/CSS Änderungen (kein Kompilieren)
  - `v1.x.0` = Pascal-Code Änderungen (Kompilieren + GitHub)
  - `v2.0.0` = größere neue Features

### Branding
- Titel von "SHELLY CONTROLLER" zu **"SMARTHOMERW"** geändert
- Eigenes Logo (`logorw.png`) im Header

---

## [v1.0.4] – 27.04.2026

### Veröffentlichung
- App auf **GitHub Pages** gehostet
  - Repository: `rwhexa/shelly-controller` (stabile Backup-Version)
  - Repository: `rwhexa/shellyweb` (Entwicklungs-Version)
  - URL: `https://rwhexa.github.io/shellyweb/Project1.html`
- Weltweit erreichbar von jedem Gerät mit Browser
- Updates: einfach Dateien im Repository ersetzen

---

## [v1.0.3] – 27.04.2026

### Geändert (Anzeige)
- **Sensor-Werte angepasst** für Shelly 1PM Mini Gen3
  - Helligkeit (`%`) entfernt
  - Luftfeuchte (`%`) entfernt
  - **Spannung (`V`)** hinzugefügt aus `voltage`
  - **Strom (`A`)** hinzugefügt aus `current`
  - **Verbrauch (`kWh`)** hinzugefügt aus `aenergy.total`
- Anzeige-Reihenfolge: Leistung · Spannung · Strom · Verbrauch

### Hinzugefügt
- `TShellyStatus` um `Voltage` und `Current` erweitert
- `ParseEventMessage` parst jetzt alle vier Werte aus `switch:0`

---

## [v1.0.2] – 27.04.2026

### Hinzugefügt
- **Tab-Navigation** in zwei Seiten getrennt:
  - Tab 1: **VERBINDUNG** (HiveMQ Broker-Einstellungen)
  - Tab 2: **GERÄTE** (Shelly steuern + Log)
- Verbindungs-Status-Dot im Tab (grün/gelb/rot, immer sichtbar)
- Automatischer Wechsel zu Tab GERÄTE nach erfolgreichem Verbinden
- **Responsive CSS** für Mobile (< 600px → einspaltig, große Touch-Targets)
- Touch-freundliche Button-Größen (mindestens 44px Höhe)

---

## [v1.0.1] – 27.04.2026

### Hinzugefügt
- **localStorage** für persistente Speicherung:
  - Host, Port, Benutzer, Passwort werden beim Verbinden automatisch gespeichert
  - Geräteliste (ID + Name) wird gespeichert
  - Beim Browser-Neustart sind alle Felder vorausgefüllt
  - Geräte werden nach erneutem Verbinden automatisch wieder angelegt

---

## [v1.0.0] – 27.04.2026 (Initiale Funktionsfähigkeit)

### Behoben (kritische Bugs der Vorversion)

#### Bug 1: Topic-Präfix in `Unit1.pas`
```pascal
// Vorher (falsch):
if ATopic.StartsWith('shellies/' + FDevices[I].DeviceID) then
// Nachher (richtig):
if ATopic.StartsWith(FDevices[I].DeviceID) then
```
Gen3 Shellys senden ohne `shellies/`-Präfix → MQTT-Nachrichten kamen nie beim
Parser an, Status blieb auf OFFLINE.

#### Bug 2: Falscher RPC `src` in `ShellyDevice.pas`
```
Vorher: src = 'tms-webcore-app'
        → Antwort kam auf Topic 'tms-webcore-app' (niemand abonniert)
Nachher: src = 'shelly1pmminig3-.../rpc/reply'
        → Antwort kommt auf abonniertes Topic
```
RPC-Antworten (z.B. `Switch.GetStatus`) gingen ins Leere.

#### Bug 3: Pas2js-Methodenaufruf in `ShellyDevice.pas`
```javascript
// Vorher (funktioniert nicht):
pas.ShellyDevice.TShellyDevice.ParseSwitchStatus(this, params['switch:0']);
// Nachher: alles direkt im asm-Block inline parsen
```

#### Bug 4: Pas2js-Property-Zugriff in `Unit1.pas`
```javascript
// Vorher: dev.Name (existiert in JS nicht)
// Nachher: dev.FName (pas2js kompiliert Properties als Felder)
```

### Funktionsumfang dieser Version
- ✅ HiveMQ Cloud Verbindung über WSS Port 8884
- ✅ Online/Offline Status-Anzeige
- ✅ Ein/Aus schalten mit Status-Rückmeldung
- ✅ Leistungsanzeige (W)
- ✅ Mehrere Geräte verwaltbar
- ✅ Gerät hinzufügen / entfernen
- ✅ MQTT-Log für Debugging
- ✅ 30-Sekunden Status-Refresh-Timer
- ✅ Dark-Theme UI mit Rajdhani-Font

---

## Architektur (v1.2.0)

```
┌─────────────────────────────┐    ┌─────────────────────────────┐
│  Project1.html              │    │  wohnung.html               │
│  (TMS Web Core)             │    │  (pure HTML/JavaScript)     │
│  - Tab Verbindung           │    │  - Wohnung-Bild             │
│  - Tab Geräte               │←──→│  - Klickbare Lampen         │
│  - Schalten + Status        │    │  - Eigener MQTT-Client      │
│  - Logo & Versionsanzeige   │    │  - Auto-Connect             │
│  - Link "🏠 Grundriss"      │    │  - Link "⚡ Steuerung →"    │
└──────────────┬──────────────┘    └──────────────┬──────────────┘
               │                                  │
               └─────────► localStorage ◄─────────┘
                          (Host, User, Pass, Devices)

                              ↕ WSS Port 8884

                    ┌─────────────────────┐
                    │  HiveMQ Cloud       │
                    │  MQTT Broker (TLS)  │
                    └──────────┬──────────┘
                               │
                               ↕ MQTT Port 8883
                    ┌─────────────────────┐
                    │  Shelly 1PM Mini    │
                    │  Gen3               │
                    └─────────────────────┘
```

## Projektdateien (v1.2.0)

| Datei | Zweck | Quelle |
|-------|-------|--------|
| `Project1.dpr` | Delphi Projekt-Hauptdatei | Delphi |
| `Unit1.pas` | Hauptformular, MQTT-Logik, localStorage | Delphi |
| `Unit1.dfm` | Formular-Layout (Auto-Refresh Timer) | Delphi |
| `Unit1.html` | Frontend mit Tab-Nav + Grundriss-Link | Delphi |
| `Project1.html` | TMS-Template (Einstiegspunkt) | Delphi |
| `MQTTBridge.pas` | Wrapper um MQTT.js | Delphi |
| `ShellyDevice.pas` | Shelly Gen3 Geräte-Logik | Delphi |
| `wohnung.html` | Eigenständige Grundriss-Seite | Manuell |
| `grundriss.png` | Wohnung 3D-Bild | Manuell |
| `logorw.png` | Logo (Rw) | Manuell |

## Hosting

- **Lokal (Entwicklung):** Delphi Run, oder `python -m http.server 8000` im `TMSWeb\Debug` Ordner
- **Produktiv:**
  - GitHub Pages Backup: `https://rwhexa.github.io/shelly-controller/Project1.html`
  - GitHub Pages Aktuell: `https://rwhexa.github.io/shellyweb/Project1.html`
  - Update: alle Dateien aus `TMSWeb\Debug\` + `wohnung.html`/`grundriss.png` hochladen

## Verwendete Bibliotheken

- **MQTT.js** (CDN: `unpkg.com/mqtt`) – MQTT-Client für Browser via WebSocket
- **Google Fonts** – Rajdhani (UI), JetBrains Mono (Monospace)
- **TMS Web Core** v2.4.6.1 – Pascal-zu-JavaScript Compiler
- **pas2js** v2.3.1
