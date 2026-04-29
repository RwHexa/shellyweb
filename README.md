# 🏠 SmartHomeRw

> Browserbasierte Smart-Home-Steuerung für **Shelly Gen3** Geräte über **MQTT** und **HiveMQ Cloud** — mit interaktivem Wohnungs-Grundriss

Eine moderne, plattformübergreifende Web-App zur Fernsteuerung und Überwachung
von Shelly-Smart-Home-Geräten. Zwei Ansichten: klassische Steuerung mit Status-Karten
oder visueller Grundriss mit klickbaren Lampen. **Läuft im Desktop- und Android-Browser
ohne Installation!**

🚀 **Live-Demo:** [rwhexa.github.io/shellyweb/Project1.html](https://rwhexa.github.io/shellyweb/Project1.html)

---

## ✨ Features

### Steuerungs-Seite
- 🔐 **HiveMQ Cloud** Verbindung via WebSocket (WSS, TLS-verschlüsselt)
- 📊 **Live-Messwerte:** Leistung · Spannung · Strom · Verbrauch
- 💡 **Ein/Aus schalten** mit Echtzeit-Statusanzeige
- ➕ **Mehrere Geräte** verwaltbar
- 🐛 **MQTT-Log** zur Diagnose

### Grundriss-Seite (NEU v1.2.0)
- 🏠 **Interaktiver Wohnungs-Grundriss** mit 3D-Bild
- 💡 **Klickbare Lampen-Overlays** in den Räumen
- ⚡ **Wohnzimmer-Lampe** schaltet echten Shelly direkt
- 🎨 **3 Demo-Lampen** für andere Räume
- 📊 **Status-Anzeige:** Online/Offline + Watt-Verbrauch

### Allgemein
- 📱 **Mobile-ready** – funktioniert auf Android und iOS Browsern
- 💾 **Persistente Speicherung** (Zugangsdaten + Geräte bleiben erhalten)
- 🎨 **Dark-Theme UI** mit Tab-Navigation und eigenem Logo
- 🔄 **Auto-Refresh** für Status-Updates

---

## 🖥️ Plattformen

| Plattform | Status |
|-----------|:------:|
| Windows Chrome / Edge / Firefox | ✅ |
| macOS Safari / Chrome | ✅ |
| Linux Chrome / Firefox | ✅ |
| Android Chrome | ✅ |
| iOS Safari | ✅ |

---

## 🏗️ Architektur

```
┌─────────────────────────────┐    ┌─────────────────────────────┐
│  Project1.html              │    │  wohnung.html               │
│  (TMS Web Core App)         │←──→│  (eigenständige HTML-Seite) │
│                             │    │                             │
│  - Tab Verbindung           │    │  - Wohnungs-Bild            │
│  - Tab Geräte               │    │  - Klickbare Lampen         │
│  - Schalten + Status        │    │  - Eigener MQTT-Client      │
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

---

## 📂 Projektstruktur

| Datei | Zweck | Quelle |
|-------|-------|--------|
| `Project1.dpr` | Delphi Projekt-Hauptdatei | Delphi |
| `Unit1.pas` | Hauptformular, MQTT-Logik, localStorage | Delphi |
| `Unit1.dfm` | Formular-Layout (Auto-Refresh Timer) | Delphi |
| `Unit1.html` | Frontend mit Tab-Nav + Grundriss-Link | Delphi Build |
| `Project1.html` | TMS-Template (Einstiegspunkt) | Delphi Build |
| `ShellyController.js` | Kompilierter Pascal-Code | Delphi Build |
| `MQTTBridge.pas` | Wrapper um MQTT.js | Delphi |
| `ShellyDevice.pas` | Shelly Gen3 Geräte-Logik | Delphi |
| `wohnung.html` | Eigenständige Grundriss-Seite | Manuell |
| `grundriss.png` | Wohnung 3D-Bild | Manuell |
| `logorw.png` | Logo (Rw) | Manuell |

---

## 🚀 Schnellstart

### Live-Version nutzen

Einfach im Browser öffnen:

**[https://rwhexa.github.io/shellyweb/Project1.html](https://rwhexa.github.io/shellyweb/Project1.html)**

1. Tab **VERBINDUNG** → HiveMQ-Zugangsdaten eintragen
2. **VERBINDEN** klicken
3. Tab **GERÄTE** → Shelly Device-ID hinzufügen
4. Status erscheint sofort, schalten und überwachen
5. Im Header oben rechts auf **🏠 Grundriss** klicken
6. Wohnung-Bild mit klickbaren Lampen erscheint
7. Wohnzimmer-Lampe klicken → echter Shelly schaltet

> 💡 Zugangsdaten und Geräte werden im Browser gespeichert (localStorage) –
> beim nächsten Aufruf ist alles vorausgefüllt.

### Selber kompilieren

**Voraussetzungen:**
- Delphi 12.1 (RAD Studio)
- TMS Web Core v2.4.6.1 oder neuer

**Schritte:**
1. Repository klonen: `git clone https://github.com/rwhexa/shellyweb.git`
2. `ShellyController.dproj` in Delphi öffnen
3. **Project → Clean → Build All**
4. Output liegt in `TMSWeb\Debug\`
5. Folgende manuelle Dateien dazukopieren in `TMSWeb\Debug\`:
   - `wohnung.html`
   - `grundriss.png`
   - `logorw.png`
6. Lokal testen:
   ```bash
   cd TMSWeb\Debug
   python -m http.server 8000
   ```
   → `http://localhost:8000/Project1.html`

---

## ⚙️ HiveMQ Cloud konfigurieren

1. Account anlegen auf [hivemq.cloud](https://www.hivemq.cloud/) (kostenlos)
2. Cluster erstellen → Hostname, Benutzer, Passwort notieren
3. **Wichtig:** Im Cluster den **WebSocket Listener (Port 8884)** aktivieren
4. Im Shelly-Gerät MQTT auf den HiveMQ-Server konfigurieren (Port 8883, TLS)

---

## 📝 Wichtige Hinweise

⚠️ **Device-IDs immer kleinschreiben!**
MQTT-Topics sind case-sensitive. Eine Device-ID wie
`Shelly1pmminig3-...` mit großem `S` führt dazu, dass keine Status-Nachrichten ankommen.
Auf Mobilgeräten besonders aufpassen, da Auto-Korrektur den ersten Buchstaben
gerne automatisch großschreibt.

✅ **Korrekt:** `shelly1pmminig3-34b7dac507e0`

⚡ **Seitenwechsel zwischen Steuerung und Grundriss:**
Beim Wechsel wird die MQTT-Verbindung neu aufgebaut (~1-2 Sekunden) – technisch
bedingt durch Browser-Seitenwechsel. Da Zugangsdaten und Geräte im localStorage
gespeichert sind, geschieht das automatisch.

---

## 🎨 Grundriss anpassen

Die Lampen-Positionen auf dem Grundriss sind als Prozentangaben in `wohnung.html`
definiert:

```html
<div class="lamp" id="lampWohnzimmer" style="left:51%;top:33%">
```

Anpassen für eigene Wohnung: `left` und `top` ändern.
Eigenes Wohnungs-Bild: `grundriss.png` ersetzen (empfohlene Breite: 800px).

---

## 🛠️ Technologie-Stack

- **Frontend-Framework:** [TMS Web Core](https://www.tmssoftware.com/site/tmswebcore.asp) (Steuerungs-Seite)
- **Pure HTML/JavaScript** (Grundriss-Seite)
- **Sprache:** Object Pascal (kompiliert zu JavaScript via pas2js)
- **MQTT-Library:** [MQTT.js](https://github.com/mqttjs/MQTT.js) (CDN)
- **Broker:** HiveMQ Cloud
- **Hardware:** Shelly 1PM Mini Gen3
- **Hosting:** GitHub Pages
- **Fonts:** Rajdhani (UI), JetBrains Mono (Daten)

---

## 📜 Versionshistorie

Vollständige Versionshistorie siehe [CHANGELOG.md](CHANGELOG.md).

**Aktuell:** v1.2.0 – Grundriss-Seite mit interaktiven Lampen

---

## 🔗 Repositories

| Repository | Zweck |
|------------|-------|
| [`shelly-controller`](https://github.com/rwhexa/shelly-controller) | Stabile Backup-Version |
| [`shellyweb`](https://github.com/rwhexa/shellyweb) | Aktuelle Entwicklungs-Version |

---

## 📄 Lizenz

Privates Projekt – frei zur Nutzung und Anpassung.

---

## 👤 Autor

**Reiny** – Smart Home & IoT Projekte mit Delphi / Lazarus / Free Pascal
