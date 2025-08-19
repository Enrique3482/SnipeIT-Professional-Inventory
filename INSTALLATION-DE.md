# Installationsanleitung 📋

Diese Anleitung führt Sie durch die Einrichtung des SnipeIT Professional Inventory Systems.

## Voraussetzungen ✅

### Systemanforderungen
- **PowerShell 5.1+** (Windows PowerShell oder PowerShell Core)
- **Windows 10** (Build 1809+) oder **Windows Server 2016+**
- **SnipeIT-Instanz** mit API-Zugang
- **Administrator-Rechte** für vollständige Hardware-Erkennung
- **Netzwerkverbindung** zu Ihrem SnipeIT-Server

### SnipeIT-Anforderungen
- SnipeIT v5.0+ empfohlen
- API-Token mit folgenden Berechtigungen:
  - Lese-/Schreibzugriff auf Assets
  - Lese-/Schreibzugriff auf Kategorien
  - Lese-/Schreibzugriff auf Custom Fields
  - Lese-/Schreibzugriff auf Modelle
  - Lese-/Schreibzugriff auf Hersteller

## Schritt 1: Script herunterladen 📥

### Option A: Git Clone (Empfohlen)
```powershell
# Repository klonen
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

### Option B: Direkter Download
1. ZIP-Datei von GitHub herunterladen
2. In einen Ordner extrahieren (z.B. `C:\Scripts\SnipeIT-Inventory\`)
3. PowerShell als Administrator öffnen
4. Zum extrahierten Ordner navigieren

## Schritt 2: SnipeIT-Verbindung konfigurieren 🔧

### API-Token beschaffen
1. Bei Ihrer SnipeIT-Instanz anmelden
2. Zu **Konto-Einstellungen** → **API-Schlüssel** gehen
3. **Neuen Token erstellen** klicken
4. Den generierten Token kopieren

### Konfigurationsdatei aktualisieren
1. `SnipeConfig.json` in einem Texteditor öffnen
2. Folgende Werte aktualisieren:

```json
{
  "Snipe": {
    "Token": "IHR-API-TOKEN-HIER",
    "Url": "https://ihre-snipeit-instanz.com/api/v1",
    "StandardCompanyName": "Ihr Firmenname"
  }
}
```

**Wichtig**: Ersetzen Sie:
- `IHR-API-TOKEN-HIER` mit Ihrem tatsächlichen API-Token
- `https://ihre-snipeit-instanz.com/api/v1` mit Ihrer SnipeIT-URL
- `Ihr Firmenname` mit dem Namen Ihrer Organisation

## Schritt 3: Konfiguration testen 🧪

Bevor Sie in der Produktion ausführen, testen Sie Ihre Einrichtung:

```powershell
# Zuerst im Testmodus ausführen
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging

# Mit simulierter Hardware testen
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging
```

Sie sollten sehen:
- ✅ Konfiguration erfolgreich geladen
- ✅ API-Verbindungstest bestanden
- ✅ Hardware-Erkennung funktioniert
- ✅ Feld-Mapping validiert

## Schritt 4: Erster Produktionslauf 🚀

Sobald der Test erfolgreich ist:

```powershell
# Standard-Produktionslauf
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Ihre Firma"

# Mit ausführlicher Protokollierung zur Fehlersuche
.\SnipeIT-Professional-Inventory.ps1 -VerboseLogging
```

## Schritt 5: Ergebnisse überprüfen ✔️

### SnipeIT überprüfen
1. Bei Ihrer SnipeIT-Instanz anmelden
2. Zu **Assets** → **Alle Assets** navigieren
3. Nach Ihrem Computer-Asset suchen
4. Überprüfen, ob Custom Fields ausgefüllt sind
5. Nach externen Monitoren suchen (falls erkannt)

### Logs überprüfen
Logs auf Probleme überprüfen:
- **Haupt-Log**: `C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log`
- **Fehler-Log**: `C:\ProgramData\SnipeIT\Errorlog\SnipeIT-Errors.log`

## Erweiterte Konfiguration 🔬

### Custom Field-Einrichtung
Das System erstellt und mappt Custom Fields automatisch. Zur Anpassung:

1. Den Abschnitt `CustomFieldMapping` in `SnipeConfig.json` überprüfen
2. Feldnamen bei Bedarf ändern
3. Änderungen mit `-TestMode` testen

### Automatische Bereitstellung
Für Enterprise-Bereitstellung über Gruppenrichtlinien oder SCCM:

```powershell
# Bereitstellungsbefehl
powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "Enterprise"
```

### Geplante Ausführung
Geplante Aufgabe erstellen, um Inventarisierung regelmäßig auszuführen:

```powershell
# Geplante Aufgabe erstellen (als Administrator ausführen)
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File 'C:\Scripts\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1'"
$trigger = New-ScheduledTaskTrigger -Daily -At "2:00"
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries
Register-ScheduledTask -TaskName "SnipeIT Inventur" -Action $action -Trigger $trigger -Settings $settings -User "SYSTEM"
```

## Fehlerbehebung 🔧

### Häufige Probleme

#### API-Authentifizierungsfehler
```
Fehler: 401 Unauthorized
```
**Lösung**: API-Token und Berechtigungen überprüfen

#### PowerShell-Ausführungsrichtlinie
```
Fehler: Ausführungsrichtlinien-Einschränkung
```
**Lösung**: Als Administrator ausführen und Ausführungsrichtlinie setzen:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

#### Hardware-Erkennungsprobleme
```
Warnung: Begrenzte Hardware-Informationen erkannt
```
**Lösung**: Sicherstellen, dass mit Administrator-Rechten ausgeführt wird

#### SnipeIT-Verbindungsprobleme
```
Fehler: Verbindung zu SnipeIT nicht möglich
```
**Lösung**: Netzwerkverbindung und URL überprüfen:
```powershell
Test-NetConnection ihr-snipeit-server.com -Port 443
```

### Hilfe erhalten

1. **Logs überprüfen** in `C:\ProgramData\SnipeIT\`
2. **Mit ausführlicher Protokollierung ausführen** mit `-VerboseLogging`
3. **Im sicheren Modus testen** mit `-TestMode`
4. **Problem auf GitHub melden** mit Log-Auszügen

## Sicherheitsaspekte 🔒

### API-Token-Sicherheit
- `SnipeConfig.json` sicher speichern
- API-Token mit minimalen Rechten verwenden
- Erwägen Sie Umgebungsvariablen für sensible Daten
- API-Token regelmäßig rotieren

### Netzwerksicherheit
- HTTPS für SnipeIT-Verbindungen verwenden
- VPN für Remote-Bereitstellungen erwägen
- SSL-Zertifikate validieren

## Nächste Schritte 📈

Nach erfolgreicher Installation:

1. **Regelmäßige Läufe planen** für laufende Inventarisierung
2. **Logs überwachen** auf Probleme
3. **Feld-Mappings anpassen** nach Bedarf
4. **Personal schulen** zu Asset-Management-Workflows
5. **Wartungspläne einrichten** in SnipeIT

---

**Hilfe benötigt?** 
- 📖 [Dokumentation](../../wiki)
- 🐛 [Probleme melden](../../issues)
- 💬 [Diskussionen](../../discussions)