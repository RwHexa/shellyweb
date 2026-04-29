# 🌍 GitHub Pages als kostenloser Webserver

> Anleitung zum Hosten einer Web-Anwendung auf GitHub Pages — kostenlos, weltweit
> erreichbar, ohne eigene Domain oder Server

Dokumentation des Vorgehens für das **SmartHomeRw**-Projekt (TMS Web Core App).
Übertragbar auf jede statische Web-Anwendung (HTML, CSS, JavaScript, Bilder).

---

## 🎯 Was ist GitHub Pages?

GitHub Pages ist ein **kostenloser Hosting-Service** von GitHub, der automatisch
aus einem Repository einen Webserver macht. Jede HTML/CSS/JS/Bild-Datei im
Repository wird unter einer öffentlichen URL erreichbar.

### Vorteile

- ✅ **Kostenlos** – keine versteckten Gebühren
- ✅ **Weltweit erreichbar** – via globalem CDN
- ✅ **HTTPS automatisch** – kein SSL-Zertifikat nötig
- ✅ **Keine eigene Domain nötig** – kostenlose `*.github.io` Subdomain
- ✅ **Automatisches Deployment** – Datei-Upload reicht
- ✅ **Versionierung** – Git-Historie aller Änderungen

### Einschränkungen

- Nur **statische Dateien** (HTML, CSS, JS, Bilder)
- Kein PHP, Python, Datenbanken serverseitig
- Repository muss **public** sein (für kostenloses GitHub Pages)
- Maximal 1 GB Repository-Größe, 100 GB Bandbreite/Monat

> 💡 **Wichtig:** Die App selbst kann durchaus mit Server-Diensten kommunizieren
> (wie HiveMQ Cloud bei SmartHomeRw) – nur die App-Dateien selbst sind statisch.

---

## 📋 Voraussetzungen

- GitHub-Account (kostenlos auf [github.com](https://github.com))
- Webbrowser
- Zu hostende Dateien (HTML, CSS, JS, Bilder)

---

## 🚀 Schritt-für-Schritt Anleitung

### Schritt 1: GitHub Account anlegen

1. Auf [github.com](https://github.com) gehen
2. **Sign up** klicken
3. Username, E-Mail, Passwort eintragen
4. E-Mail-Bestätigung durchführen

### Schritt 2: Neues Repository anlegen

1. Nach dem Login oben rechts auf das **"+"** klicken → **New repository**
2. Folgendes ausfüllen:
   ```
   Repository name:  shellyweb         (oder beliebig)
   Description:      Smart Home App    (optional)
   Visibility:       ● Public          (wichtig für kostenloses Pages!)
   Initialize:       ☑ Add a README file
   ```
3. **Create repository** klicken

### Schritt 3: Dateien hochladen

1. Im neuen Repository auf **Add file** → **Upload files** klicken
2. Dateien per **Drag & Drop** ins Browser-Fenster ziehen
3. Beispiel für SmartHomeRw:
   ```
   Project1.html
   Unit1.html
   ShellyController.js
   wohnung.html
   grundriss.png
   logorw.png
   ```
4. Unten **Commit changes** klicken
5. Warten bis Upload abgeschlossen

> 💡 Auch ganze Ordner können per Drag & Drop hochgeladen werden.

### Schritt 4: GitHub Pages aktivieren

Das ist der entscheidende Schritt zur Webserver-Aktivierung:

1. Im Repository oben in der Menüleiste auf **Settings** klicken
2. In der linken Seitenleiste runterscrollen bis zum Abschnitt
   **Code and automation** → **Pages** klicken
3. Unter **Build and deployment** folgendes einstellen:
   ```
   Source:   [ Deploy from a branch  ▼ ]
   Branch:   [ main  ▼ ]  [ / (root)  ▼ ]
   ```
4. **Save** klicken
5. **1-2 Minuten warten** und Seite neu laden

Dann erscheint oben ein grünes Banner:
```
✓ Your site is live at
  https://USERNAME.github.io/REPOSITORY/
```

### Schritt 5: App im Browser aufrufen

Die App ist jetzt unter folgender URL erreichbar:

```
https://USERNAME.github.io/REPOSITORY/HAUPT-DATEI.html
```

**Beispiel SmartHomeRw:**
```
https://rwhexa.github.io/shellyweb/Project1.html
```

URL als Lesezeichen speichern – fertig!

---

## 🔄 Updates einspielen

### Methode 1: Datei ersetzen (einfachste)

1. Im Repository auf die zu ersetzende Datei klicken
2. Oben rechts auf das **Stift-Symbol** (✏️) für "Edit this file" klicken
   – ODER auf **Add file** → **Upload files** → gleichnamige Datei hochladen
3. **Commit changes** klicken
4. Nach 1-2 Minuten ist die neue Version live

### Methode 2: Mehrere Dateien gleichzeitig

1. **Add file** → **Upload files**
2. Alle geänderten Dateien per Drag & Drop hochladen
   (gleichnamige Dateien werden automatisch ersetzt)
3. **Commit changes**

### Methode 3: Datei löschen

1. Datei im Repository öffnen
2. Oben rechts auf die **drei Punkte** (⋯) → **Delete file**
3. **Commit changes**

---

## 🛡️ Backup-Strategie mit zwei Repositories

Bewährter Ansatz für Produktiv-Anwendungen:

### Konzept

```
Repository 1: stabile-version  ← Backup, läuft immer, nicht anrühren
Repository 2: dev-version      ← Entwicklung, hier wird experimentiert
```

### Vorteil

- Während du an Repository 2 entwickelst, **läuft Repository 1 weiter**
- Falls Repository 2 kaputt geht, hast du die letzte funktionierende Version
- Beide Versionen sind über separate URLs erreichbar

### Beispiel SmartHomeRw

| Repository | URL | Zweck |
|-----------|-----|-------|
| `shelly-controller` | `https://rwhexa.github.io/shelly-controller/` | Stabile Version |
| `shellyweb` | `https://rwhexa.github.io/shellyweb/` | Entwicklung |

### Workflow

1. Neue Funktion in `shellyweb` entwickeln und testen
2. Wenn alles läuft: gleiche Dateien zusätzlich nach `shelly-controller` kopieren
3. So ist `shelly-controller` immer der "letzte funktionierende Stand"

---

## 🔍 Fehlersuche

### Fehler: Seite zeigt 404

- Pages-Aktivierung in **Settings → Pages** prüfen
- Branch muss `main` sein (oder der Standard-Branch)
- 1-2 Minuten warten nach Aktivierung
- Hauptdatei (z.B. `index.html` oder `Project1.html`) muss im Root liegen

### Fehler: Bilder werden nicht geladen

- **Groß-/Kleinschreibung der Dateinamen** beachten – GitHub Pages ist case-sensitive
- `Image.PNG` ≠ `image.png`
- Dateinamen ohne Sonderzeichen oder Leerzeichen verwenden
- Pfade in HTML relativ angeben: `<img src="bild.png">` (kein `/bild.png` oder `C:\...`)

### Änderungen werden nicht angezeigt

- **Browser-Cache leeren:** Strg+Shift+R (Hard Reload)
- **Inkognito-Modus** öffnen zum Testen
- 1-2 Minuten warten – Pages braucht etwas Zeit

### Status der letzten Aktualisierung prüfen

1. Repository → Tab **Actions** öffnen
2. Letzten Workflow "pages build and deployment" ansehen
3. Grünes Häkchen ✓ = erfolgreich deployed
4. Rotes X = Fehler, Details ansehen

---

## 🎯 Tipps & Best Practices

### Lokales Testen vor Upload

Vor dem Hochladen lokal testen mit Python:

```bash
cd projektordner
python -m http.server 8000
```

Browser: `http://localhost:8000/Project1.html`

Wenn lokal alles läuft, dann erst auf GitHub.

### Dateinamen klein schreiben

Konvention: Alle Dateinamen klein, mit Bindestrich getrennt:
```
✅ wohnung.html
✅ grundriss.png
✅ shelly-controller.js
❌ Wohnung.HTML
❌ Grundriss Bild.png
```

### Sensible Daten nicht hochladen

- Keine Passwörter, API-Keys oder private Daten ins Repository
- Auch nicht in JavaScript-Dateien (sind öffentlich lesbar!)
- Repository ist **public** – jeder kann den Code sehen

> 💡 **SmartHomeRw-Beispiel:** Die HiveMQ-Zugangsdaten werden vom Nutzer
> selbst eingegeben und nur in seinem Browser-localStorage gespeichert –
> nie im Repository.

### Domain-Erweiterung (optional)

Falls später eine eigene Domain gewünscht:

1. Domain bei einem Provider kaufen
2. In Repository → Settings → Pages → **Custom domain** eintragen
3. DNS-Records beim Domain-Provider setzen (Anleitung von GitHub befolgen)

Aber: für **persönliche Projekte ist `*.github.io` völlig ausreichend!**

---

## 📊 Was läuft tatsächlich wo?

```
┌──────────────────────────────────────────────────────────┐
│                     GitHub Pages                         │
│                                                          │
│  Repository: shellyweb                                   │
│  ├── Project1.html        ← App-Einstieg (statisch)     │
│  ├── ShellyController.js  ← Kompilierter Pascal-Code    │
│  ├── wohnung.html          ← Grundriss-Seite            │
│  ├── grundriss.png         ← Wohnung-Bild               │
│  └── logorw.png            ← Logo                       │
│                                                          │
│  URL: https://rwhexa.github.io/shellyweb/                │
└────────────────────┬─────────────────────────────────────┘
                     │
                     │ Browser lädt App
                     ↓
┌──────────────────────────────────────────────────────────┐
│                Nutzer-Browser                            │
│                                                          │
│  - HTML/CSS gerendert                                    │
│  - JavaScript läuft                                      │
│  - Eingaben in localStorage                              │
└────────────────────┬─────────────────────────────────────┘
                     │
                     │ MQTT über WebSocket (WSS)
                     ↓
┌──────────────────────────────────────────────────────────┐
│                   HiveMQ Cloud                           │
│              (separater Service, kostenlos)              │
└────────────────────┬─────────────────────────────────────┘
                     │
                     │ MQTT
                     ↓
┌──────────────────────────────────────────────────────────┐
│                   Shelly Gen3                            │
│                  (im Heimnetz)                           │
└──────────────────────────────────────────────────────────┘
```

GitHub Pages liefert **nur** die statischen App-Dateien aus.
Die Live-Kommunikation läuft über HiveMQ und der Browser direkt zum Shelly.

---

## 💰 Kosten-Übersicht

| Komponente | Kosten |
|-----------|--------|
| GitHub Account | Kostenlos |
| GitHub Pages Hosting | Kostenlos |
| `*.github.io` Subdomain | Kostenlos |
| HTTPS-Zertifikat | Kostenlos (automatisch) |
| HiveMQ Cloud (Free Tier) | Kostenlos (bis 100 Connections) |
| Shelly Gen3 Hardware | Einmalig ~20-30€ |
| **Gesamtkosten Hosting** | **0,00 €** |

---

## 🔗 Hilfreiche Links

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Desktop](https://desktop.github.com/) – grafische Git-Oberfläche (optional)
- [Markdown Guide](https://www.markdownguide.org/) – für README/CHANGELOG Formatierung

---

## ✅ Zusammenfassung

Mit GitHub Pages kann jede statische Web-App **kostenlos und global verfügbar**
gemacht werden. Der Workflow ist denkbar einfach:

1. Repository anlegen
2. Dateien hochladen
3. Pages aktivieren
4. URL nutzen

Updates: einfach Dateien neu hochladen – fertig.

Für **persönliche Projekte, Prototypen, Smart-Home-Steuerungen, Dashboards
oder Visualisierungen** ist GitHub Pages eine perfekte Lösung.

---

*Erstellt für SmartHomeRw v1.2.0 – April 2026*
