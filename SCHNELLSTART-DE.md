# Schnellstartanleitung - SnipeIT Professional Inventory v2.2.0 (DE)

![Quick Start](https://img.shields.io/badge/Quick%20Start-v2.2.0-brightgreen.svg)
![Time to Setup](https://img.shields.io/badge/Setup%20Time-5%20minutes-blue.svg)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-green.svg)

## 🚀 Sofort loslegen - 3 einfache Schritte

### Schritt 1: Download und Entpacken (1 Minute)
```bash
# Repository herunterladen
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git

# ODER: ZIP-Datei herunterladen und entpacken
# Dateien in gewünschtes Verzeichnis kopieren (z.B. C:\Scripts\SnipeIT)
```

### Schritt 2: Konfiguration anpassen (2 Minuten)
```powershell
# SnipeConfig.json mit Ihren Daten bearbeiten
notepad SnipeConfig.json
```

**Mindestens diese Werte anpassen:**
```json
{
  "Snipe": {
    "Url": "http://IHR-SNIPEIT-SERVER/api/v1",
    "Token": "IHR-API-TOKEN-HIER",
    "StandardCompanyName": "Ihre Firma GmbH"
  }
}
```

### Schritt 3: Ersten Test starten (2 Minuten)
```bash
# Einfach doppelklicken:
Test-Modus.bat
```

**🎉 Fertig! Das war's schon!**

---

## 🎯 Die 3 Startmethoden im Überblick

| Methode | Zielgruppe | Schwierigkeit | Zeit |
|---------|------------|---------------|------|
| **🖱️ Batch-Dateien** | Endbenutzer | ⭐ Sehr einfach | 30 Sekunden |
| **💻 PowerShell** | Power-User | ⭐⭐ Einfach | 1 Minute |
| **🔧 VS Code** | Entwickler | ⭐⭐⭐ Mittel | 2 Minuten |

---

## 🖱️ Methode 1: One-Click für Jedermann

### Verfügbare Batch-Dateien

**🧪 Test-Modus.bat**
- ✅ **100% sicher** - Keine echten Änderungen
- ✅ Vollständige Simulation
- ✅ Ideal für Tests und Lernen
- 🎯 **Einfach doppelklicken!**

**🚀 Produktions-Modus.bat**
- ⚠️ **Live-System** - Echte API-Aufrufe
- ✅ Erstellt/aktualisiert Assets
- ✅ Sicherheitsbestätigung erforderlich
- 🎯 **Nur nach erfolgreichem Test verwenden!**

**💬 Interaktiv-Starten.bat**
- ✅ Geführtes Menü
- ✅ Integrierte Hilfe
- ✅ Modusauswahl im Dialog
- 🎯 **Perfekt für Einsteiger!**

## 💻 Methode 2: PowerShell für Power-User

### Intelligenter Starter
```powershell
# Interaktives Menü mit Modusauswahl
.\Start-SnipeInventory.ps1
```

### Direkte Parameter-Verwendung
```powershell
# Alle verfügbaren Parameter anzeigen
Get-Help .\SnipeIT-Professional-Inventory.ps1 -Detailed

# Test-Modus mit ausführlicher Protokollierung
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging

# Produktions-Modus für bestimmten Kunden
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "ACME GmbH"

# Mit simulierter Hardware (für Demos)
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware
```

## 🔧 Methode 3: VS Code für Entwickler

### Workspace öffnen
```bash
# VS Code mit komplettem Workspace starten
code workspace.code-workspace
```

### Erste Schritte in VS Code

1. **Extensions installieren** (automatisch vorgeschlagen)
   - PowerShell Extension
   - GitLens (für Git-Integration)
   - Error Lens (für bessere Fehleranzeige)

2. **Integrierte Tasks verwenden**
   - `Strg+Shift+P` → "Tasks: Run Task"
   - Auswahl der verfügbaren Modi:
     - 🧪 Test Mode (Safe)
     - 🚀 Production Mode
     - 💬 Interactive Mode
     - 📊 Code Analysis

3. **Debugging aktivieren**
   - Breakpoints setzen (Klick links neben Zeilennummer)
   - `F5` für Debug-Start
   - Variablen in Echtzeit überwachen

---

## ⚙️ Konfiguration - Das Minimum

### SnipeConfig.json - Nur 3 Werte ändern!

```json
{
  "Snipe": {
    "Url": "http://snipeit.ihrefirma.de/api/v1",     ← IHR SNIPE-IT SERVER
    "Token": "YOUR_API_TOKEN_HERE...",         ← IHR API TOKEN  
    "StandardCompanyName": "Ihre Firma GmbH"         ← IHR FIRMENNAME
  }
}
```

### Wo finde ich meine Snipe-IT Daten?

**1. API Token erstellen**
```
Snipe-IT → Einstellungen → API → Neuen Token erstellen
↓
Berechtigung: [Alle] oder [Lesen/Schreiben für Assets]
↓
Token kopieren und in SnipeConfig.json einfügen
```

**2. Server URL bestimmen**
```
Ihre Snipe-IT URL: http://snipeit.ihrefirma.de
API URL: http://snipeit.ihrefirma.de/api/v1
                                    ^^^^^^^^
                                    Wichtig: /api/v1 anhängen!
```

**3. Firmenname festlegen**
```
Der Name, unter dem Assets in Snipe-IT angelegt werden
Beispiele:
- "ACME GmbH"
- "Mustermann IT Solutions" 
- "Hauptstandort Berlin"
```

---

## 🧪 Erster Test - Schritt für Schritt

### Test-Durchlauf (empfohlen für Erstkonfiguration)

1. **Test-Modus starten**
   ```bash
   # Doppelklick auf:
   Test-Modus.bat
   ```

2. **Was Sie sehen werden:**
   ```
   +=================================================================+
   |         SNIPE-IT PROFESSIONAL INVENTORY SYSTEM                 |
   |                    Professional Edition                         |
   +=================================================================+
   
   Version: 2.2.0
   Client: Ihre Firma GmbH
   Mode: TEST MODE
   
   [INFO] Loading configuration...
   [SUCCESS] Configuration loaded from: .\SnipeConfig.json
   [INFO] Initializing custom field management...
   [SUCCESS] Custom field management initialized successfully
   [INFO] Hardware Detection - Collecting system information
   ```

3. **Erfolgreicher Test zeigt:**
   ```
   +- SYSTEM SUMMARY -----------------------------------------------+
   | Computer: DESKTOP-ABC123
   | Hardware: Dell OptiPlex 7090
   | Serial: ABC123XYZ
   | Type: Desktop
   | CPU: Intel Core i7-10700 (8 cores, 16 threads)
   | RAM: 16.0 GB
   | OS: Windows 11 Pro Build 22000
   +----------------------------------------------------------------+
   
   +- INVENTORY RESULTS --------------------------------------------+
   | Computer Asset: [OK] Synchronized
   | Custom Fields: [OK] Updated (23 fields)
   +----------------------------------------------------------------+
   
   +=================================================================+
   |           INVENTORY COMPLETED SUCCESSFULLY                     |
   |                    Professional Signature                      |
   +=================================================================+
   ```

---

## 🔍 Problembehandlung - Häufige Fälle

### Problem 1: "PowerShell Execution Policy Fehler"
```
Fehler: Die Ausführung von Skripts ist auf diesem System deaktiviert.
```
**Lösung:** Batch-Dateien verwenden (lösen das automatisch) oder:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Problem 2: "API Authentication failed"
```
[ERROR] API Authentication failed - check your API token
```
**Lösung:**
1. Snipe-IT Token prüfen (abgelaufen?)
2. Token-Berechtigung prüfen (Assets lesen/schreiben?)
3. URL korrekt? (mit /api/v1 am Ende?)

### Problem 3: "Cannot connect to server"
```
[ERROR] Failed to connect to: http://ihr-server/api/v1
```
**Lösung:**
1. Server erreichbar? `ping ihr-server`
2. Firewall/Proxy blockiert?
3. URL korrekt geschrieben?

---

## 📞 Support und Hilfe

### Dokumentation
- **README-DE.md**: Vollständige deutsche Dokumentation
- **CHANGELOG-v2.2.0-DE.md**: Alle neuen Features
- **GitHub Wiki**: Erweiterte Anleitungen

### Community
- **GitHub Issues**: [Probleme melden](https://github.com/Enrique3482/SnipeIT-Professional-Inventory/issues)
- **Discussions**: Fragen und Austausch
- **Pull Requests**: Eigene Verbesserungen beitragen

---

**✅ Schnellstart abgeschlossen!**

*Sie sind jetzt bereit, das SnipeIT Professional Inventory System produktiv zu nutzen.*

**🎯 Nächster Schritt:** Schauen Sie sich die vollständige [README-DE.md](README-DE.md) für erweiterte Features an!