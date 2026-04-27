# 🔌 Shelly Controller

> Browserbasierte Steuerung für **Shelly Gen3** Geräte über **MQTT** und **HiveMQ Cloud**

Eine moderne, plattformübergreifende Web-App zur Fernsteuerung und Überwachung
von Shelly-Smart-Home-Geräten. Läuft im Desktop-Browser und auf Android,
ohne Installation.

🚀 **Live-Demo:** [rwhexa.github.io/shelly-controller](https://rwhexa.github.io/shelly-controller/Project1.html)

---

## ✨ Features

- 🔐 **HiveMQ Cloud** Verbindung via WebSocket (WSS, TLS-verschlüsselt)
- 📊 **Live-Messwerte:** Leistung · Spannung · Strom · Verbrauch
- 💡 **Ein/Aus schalten** mit Echtzeit-Statusanzeige
- 📱 **Mobile-ready** – funktioniert auf Android und iOS Browsern
- 💾 **Persistente Speicherung** (Zugangsdaten + Geräte bleiben erhalten)
- 🎨 **Dark-Theme UI** mit Tab-Navigation
- 🔄 **30-Sekunden Auto-Refresh** für Status-Updates
- 🐛 **MQTT-Log** zur Diagnose

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
Browser (TMS Web Core → JavaScript)
    ↕ WSS Port 8884
HiveMQ Cloud (MQTT Broker)
    ↕ MQTT Port 8883 (TLS)
Shelly 1PM Mini Gen3
```

---

## 📂 Projektstruktur

| Datei | Zweck |
|-------|-------|
| `Project1.dpr` | Delphi Projekt-Hauptdatei |
| `Unit1.pas` | Hauptformular: UI-Logik, MQTT-Callbacks |
| `Unit1.dfm` | Formular-Layout (Timer für Auto-Refresh) |
| `Unit1.html` | Frontend: HTML, CSS, Tab-Navigation |
| `Project1.html` | TMS-Template (Einstiegspunkt) |
| `MQTTBridge.pas` | Wrapper um MQTT.js JavaScript-Library |
| `ShellyDevice.pas` | Shelly Gen3 Geräte-Logik (Topics, RPC) |

---

## 🚀 Schnellstart

### Live-Version nutzen

Einfach im Browser öffnen:

**[https://rwhexa.github.io/shelly-controller/Project1.html](https://rwhexa.github.io/shelly-controller/Project1.html)**

1. Tab **VERBINDUNG** → HiveMQ-Zugangsdaten eintragen
2. **VERBINDEN** klicken
3. Tab **GERÄTE** → Shelly Device-ID hinzufügen
4. Status erscheint sofort, schalten und überwachen

> 💡 Zugangsdaten und Geräte werden im Browser gespeichert (localStorage) –
> beim nächsten Aufruf ist alles vorausgefüllt.

### Selber kompilieren

**Voraussetzungen:**
- Delphi 12.1 (RAD Studio)
- TMS Web Core v2.4.6.1 oder neuer

**Schritte:**
1. Repository klonen: `git clone https://github.com/rwhexa/shelly-controller.git`
2. `ShellyController.dproj` in Delphi öffnen
3. **Project → Clean → Build All**
4. Output liegt in `TMSWeb\Debug\`
5. Mit Browser öffnen oder lokalen Server starten:
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

---

## 🛠️ Technologie-Stack

- **Frontend-Framework:** [TMS Web Core](https://www.tmssoftware.com/site/tmswebcore.asp)
- **Sprache:** Object Pascal (kompiliert zu JavaScript via pas2js)
- **MQTT-Library:** [MQTT.js](https://github.com/mqttjs/MQTT.js) (CDN)
- **Broker:** HiveMQ Cloud
- **Hardware:** Shelly 1PM Mini Gen3
- **Hosting:** GitHub Pages
- **Fonts:** Rajdhani (UI), JetBrains Mono (Daten)

---

## 📜 Versionshistorie

Vollständige Versionshistorie siehe [CHANGELOG.md](CHANGELOG.md).

**Aktuell:** v1.1.2

---

## 📄 Lizenz

Privates Projekt – frei zur Nutzung und Anpassung.

---

## 👤 Autor

**Ronny** – Smart Home & IoT Projekte mit Delphi / Lazarus / Free Pascal
