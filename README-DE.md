# SnipeIT Professional Inventory System 🖥️

[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/Enrique3482/SnipeIT-Professional-Inventory)
[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue.svg)](https://docs.microsoft.com/de-de/powershell/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## 📋 Überblick

Professionelles PowerShell-Inventarsystem für **SnipeIT** Asset-Management mit umfassender Hardware-Erkennung, intelligenter Status-Verwaltung und automatisierter Wartungsverfolgung.

### ✨ Hauptfunktionen

- 🔍 **Automatische Hardware-Erkennung** - Vollständige Systeminventur mit detaillierter Komponenten-Erkennung
- 🖥️ **Monitor-Verwaltung** - Externe Monitor-Erkennung und separate Asset-Verfolgung
- 🔌 **Docking-Station-Erkennung** - Automatische Erkennung und Komponenten-Verwaltung
- 📊 **Benutzerdefinierte Feld-Verwaltung** - Intelligente Feld-Zuordnung mit Kollisionserkennung
- 👤 **Intelligentes Checkout-System** - Automatische Benutzer-Computer-Zuordnung und Asset-Zuweisung
- 🔄 **Echtzeit-Synchronisation** - Live-Asset-Daten-Updates zu SnipeIT
- 📈 **Wartungsverfolgung** - Automatisierte Planung und Status-Überwachung
- 🎨 **Verbesserte Farbcodierung** - Optimierte Konsolen-Ausgabe mit professionellem Styling
- 🛡️ **Enterprise-Sicherheit** - Umfassende Fehlerbehandlung und Rollback-System

## 🚀 Schnellstart

### Voraussetzungen

- **PowerShell 5.1+** (Windows PowerShell oder PowerShell Core)
- **Windows 10/11** oder **Windows Server 2016+**
- **SnipeIT**-Instanz mit API-Zugang
- **Administrator-Rechte** für vollständige Hardware-Erkennung

### Installation

1. **Script herunterladen:**
   ```powershell
   # Repository klonen
   git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
   cd SnipeIT-Professional-Inventory
   ```

2. **SnipeIT-Verbindung konfigurieren:**
   ```powershell
   # Konfigurationsdatei bearbeiten
   notepad SnipeConfig.json
   ```

3. **Folgende Einstellungen aktualisieren:**
   ```json
   {
     "Snipe": {
       "Url": "https://ihre-snipeit-instanz.com/api/v1",
       "Token": "ihr-api-token-hier",
       "StandardCompanyName": "Ihr Firmenname"
     }
   }
   ```

4. **Inventur ausführen:**
   ```powershell
   # Produktionslauf
   .\SnipeIT-Professional-Inventory.ps1

   # Testmodus (empfohlen für ersten Lauf)
   .\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
   ```

## 📖 Verwendungsbeispiele

### Grundlegende Verwendung
```powershell
# Standard-Inventur-Scan
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Enterprise Corp"
```

### Erweiterte Optionen
```powershell
# Testmodus mit simulierter Hardware
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging

# Benutzerdefinierte Konfigurationsdatei
.\SnipeIT-Professional-Inventory.ps1 -ConfigurationFile "C:\Config\CustomConfig.json"

# Log-Speicherort angeben
.\SnipeIT-Professional-Inventory.ps1 -LogPath "C:\Logs\inventory.log"
```

### Automatisierte Bereitstellung
```powershell
# Bereitstellung über Gruppenrichtlinie oder SCCM
powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "Enterprise"
```

## 🔧 Konfiguration

### Wesentliche Einstellungen

| Parameter | Beschreibung | Beispiel |
|-----------|-------------|---------|
| `Url` | SnipeIT API-Endpunkt | `https://assets.firma.com/api/v1` |
| `Token` | API-Authentifizierungs-Token | `Bearer eyJ0eXAiOiJKV1Q...` |
| `StandardCompanyName` | Standard-Firma für Assets | `"Enterprise Organization"` |
| `StandardModelFieldsetId` | Benutzerdefinierte Feldset-ID | `2` |

### Feld-Zuordnung

Das System ordnet Hardware-Daten automatisch SnipeIT-benutzerdefinierten Feldern zu:

```json
{
  "_snipeit_mac_address_1": "MacAddress",
  "_snipeit_cpu_4": "Processor", 
  "_snipeit_ram_gb_5": "Memory",
  "_snipeit_os_version_2": "OperatingSystem",
  "_snipeit_uuid_9": "UUID"
}
```

### Erweiterte Funktionen

- **Automatisches Checkout**: Assets werden automatisch an Benutzer ausgegeben basierend auf Computer-Namenskonventionen
- **Monitor-Erkennung**: Externe Monitore werden als separate verfolgbare Assets erstellt
- **Docking-Stationen**: Erkannt als Komponenten und dem primären Computer zugeordnet
- **Wartungsplanung**: Automatische Berechnung der nächsten Wartungstermine

## 📊 Systemanforderungen

### Mindestanforderungen
- PowerShell 5.1
- Windows 10 (Build 1809+)
- 2GB RAM
- 100MB Festplattenspeicher
- Netzwerkverbindung zu SnipeIT

### Empfohlene Spezifikationen
- PowerShell 7.x
- Windows 11 oder Server 2022
- 4GB RAM
- Administrator-Rechte
- Domänen-verbundener Computer

## 🛠️ Fehlerbehebung

### Häufige Probleme

1. **API-Authentifizierungsfehler**
   ```powershell
   # Überprüfen Sie, ob Ihr Token ausreichende Berechtigungen hat
   Test-Connection ihre-snipeit-url.com
   ```

2. **Hardware-Erkennungsprobleme**
   ```powershell
   # Mit erhöhten Rechten ausführen
   Start-Process PowerShell -Verb RunAs
   ```

3. **Benutzerdefinierte Feld-Konflikte**
   ```powershell
   # Feld-Zuordnungen zurücksetzen
   .\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
   ```

### Log-Speicherorte

- **Haupt-Log**: `C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log`
- **Fehler-Log**: `C:\ProgramData\SnipeIT\Errorlog\SnipeIT-Errors.log`
- **Backup-Verzeichnis**: `C:\ProgramData\SnipeIT\Backups\`

## 🔄 Versionshistorie

### v2.0.0 (Aktuell)
- ✅ Verbesserte farbcodierte Konsolen-Ausgabe
- ✅ Verbesserte RAM-Erkennung mit Fallback-Methoden
- ✅ Eliminierte doppelte Speicher-Informationen
- ✅ Erweiterte externe Monitor-Erkennung
- ✅ Umfassende Docking-Station-Erkennung
- ✅ Automatische Benutzer-Computer-Zuordnung
- ✅ Enterprise-grade Fehlerbehandlung

### v1.x
- Grundlegende Hardware-Erkennung
- Einfache SnipeIT-Integration
- Manuelle Konfiguration

## 🤝 Mitwirken

Beiträge sind willkommen! Bitte befolgen Sie diese Richtlinien:

1. Repository forken
2. Feature-Branch erstellen (`git checkout -b feature/amazing-feature`)
3. Änderungen committen (`git commit -m 'Add amazing feature'`)
4. Zum Branch pushen (`git push origin feature/amazing-feature`)
5. Pull Request öffnen

## 📄 Lizenz

Dieses Projekt ist unter der MIT-Lizenz lizenziert - siehe die [LICENSE](LICENSE)-Datei für Details.

## 🆘 Support

- **Dokumentation**: [Wiki](../../wiki)
- **Issues**: [GitHub Issues](../../issues)
- **Diskussionen**: [GitHub Discussions](../../discussions)

## 🏢 Enterprise-Support

Für Enterprise-Bereitstellungen, benutzerdefinierte Integrationen oder professionellen Support:

- **E-Mail**: support@your-domain.com
- **Dokumentation**: Vollständige Bereitstellungshandbücher verfügbar
- **Schulungen**: PowerShell- und SnipeIT-Integrations-Workshops

---

**Gemacht mit ❤️ für professionelles IT-Asset-Management**

![Professional IT](https://img.shields.io/badge/Professional-IT%20Asset%20Management-blue?style=for-the-badge)