# Bereitstellungsbeispiele 🚀

Dieses Dokument stellt verschiedene Bereitstellungsszenarien für das SnipeIT Professional Inventory System vor.

## Schnellstart-Bereitstellung

### Einzelcomputer-Test
```powershell
# Test auf aktuellem Computer
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Produktions-Einzellauf
```powershell
# Produktionslauf mit benutzerdefiniertem Firmennamen
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Acme Corporation"
```

## Enterprise-Bereitstellungsszenarien

### 1. Gruppenrichtlinien-Bereitstellung

Erstellen Sie eine Gruppenrichtlinie zur domänenweiten Bereitstellung:

**GPO-Script (PowerShell):**
```powershell
# Deploy-SnipeIT-Inventory.ps1
param(
    [string]$CompanyName = "Enterprise Corporation"
)

# Ausführungsrichtlinie für diese Sitzung setzen
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force

# Script-Pfad definieren
$ScriptPath = "\\server\share\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1"
$ConfigPath = "\\server\share\SnipeIT-Inventory\SnipeConfig.json"

# Prüfen ob Script existiert
if (Test-Path $ScriptPath) {
    try {
        # Inventarisierungs-Script ausführen
        & $ScriptPath -ConfigurationFile $ConfigPath -CustomerName $CompanyName -VerboseLogging
        
        # Erfolg protokollieren
        Write-EventLog -LogName Application -Source "SnipeIT Inventory" -EventId 1001 -Message "Inventarisierung erfolgreich auf $env:COMPUTERNAME abgeschlossen"
    }
    catch {
        # Fehler protokollieren
        Write-EventLog -LogName Application -Source "SnipeIT Inventory" -EventId 1002 -EntryType Error -Message "Inventarisierung fehlgeschlagen auf $env:COMPUTERNAME: $_"
    }
}
```

### 2. SCCM-Bereitstellung

**Paket-Konfiguration:**
- **Programm**: `powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "%%COMPANYNAME%%"`
- **Ausführungsmodus**: Mit Administratorrechten ausführen
- **Nach Ausführung**: Keine Aktion erforderlich

**Erkennungsmethode:**
```powershell
# Prüfen ob Inventarisierung in letzten 7 Tagen ausgeführt wurde
$LogPath = "C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"
if (Test-Path $LogPath) {
    $LastWrite = (Get-Item $LogPath).LastWriteTime
    if ($LastWrite -gt (Get-Date).AddDays(-7)) {
        Write-Output "Inventarisierung aktuell"
    }
}
```

### 3. Geplante Aufgaben-Bereitstellung

**Erstellen über PowerShell:**
```powershell
# Create-InventoryTask.ps1
$TaskName = "SnipeIT Professional Inventory"
$ScriptPath = "C:\Scripts\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1"
$CompanyName = "Ihr Firmenname"

# Aufgaben-Aktion
$Action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -WindowStyle Hidden -File '$ScriptPath' -CustomerName '$CompanyName'"

# Aufgaben-Trigger (täglich um 2:00 Uhr)
$Trigger = New-ScheduledTaskTrigger -Daily -At "02:00"

# Aufgaben-Einstellungen
$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries -StartWhenAvailable

# Aufgaben-Principal (als SYSTEM ausführen)
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

# Aufgabe registrieren
Register-ScheduledTask -TaskName $TaskName -Action $Action -Trigger $Trigger -Settings $Settings -Principal $Principal -Description "Automatisierte SnipeIT Inventarisierung"

Write-Host "Geplante Aufgabe '$TaskName' erfolgreich erstellt" -ForegroundColor Green
```

### 4. Docker Container-Bereitstellung

**Dockerfile:**
```dockerfile
FROM mcr.microsoft.com/powershell:7.3-ubuntu-22.04

# Arbeitsverzeichnis setzen
WORKDIR /app

# Scripts kopieren
COPY . .

# Berechtigungen setzen
RUN chmod +x SnipeIT-Professional-Inventory.ps1

# Standard-Befehl
CMD ["pwsh", "-File", "SnipeIT-Professional-Inventory.ps1", "-TestMode"]
```

**docker-compose.yml:**
```yaml
version: '3.8'
services:
  snipeit-inventory:
    build: .
    environment:
      - SNIPEIT_URL=https://ihr-snipeit.com/api/v1
      - SNIPEIT_TOKEN=ihr-token
      - COMPANY_NAME=Ihre Firma
    volumes:
      - ./logs:/app/logs
      - ./config:/app/config
```

## Konfigurationsvorlagen

### 1. Entwicklungsumgebung
```json
{
  "Snipe": {
    "Url": "http://localhost:8000/api/v1",
    "Token": "dev-token-hier",
    "StandardCompanyName": "Entwicklungsfirma"
  },
  "TestMode": {
    "IsPresent": true
  },
  "VerboseLogging": {
    "IsPresent": true
  }
}
```

### 2. Produktionsumgebung
```json
{
  "Snipe": {
    "Url": "https://assets.firma.com/api/v1",
    "Token": "produktions-token-hier",
    "StandardCompanyName": "Produktionsfirma"
  },
  "Performance": {
    "ApiTimeout": 120,
    "MaxRetries": 5,
    "RetryDelay": 3
  },
  "TestMode": {
    "IsPresent": false
  }
}
```

### 3. Multi-Mandanten-Umgebung
```json
{
  "Snipe": {
    "Url": "https://mandant1.snipeit-cloud.com/api/v1",
    "Token": "mandant1-token",
    "StandardCompanyName": "Mandant 1 Corporation"
  },
  "CustomFieldMapping": {
    "_snipeit_mandant_id_1": "MandantId",
    "_snipeit_standort_code_2": "StandortCode"
  }
}
```

## Automatisierungs-Scripts

### 1. Massen-Bereitstellungs-Script
```powershell
# Deploy-Multiple-Computers.ps1
param(
    [string[]]$ComputerNames,
    [string]$CompanyName = "Enterprise",
    [PSCredential]$Credential
)

foreach ($Computer in $ComputerNames) {
    Write-Host "Bereitstellung auf $Computer..." -ForegroundColor Yellow
    
    try {
        Invoke-Command -ComputerName $Computer -Credential $Credential -ScriptBlock {
            param($Company)
            
            # Script herunterladen und ausführen
            $ScriptUrl = "https://raw.githubusercontent.com/Enrique3482/SnipeIT-Professional-Inventory/main/SnipeIT-Professional-Inventory.ps1"
            $Script = Invoke-WebRequest -Uri $ScriptUrl -UseBasicParsing
            
            # Ausführen
            Invoke-Expression $Script.Content -CustomerName $Company
        } -ArgumentList $CompanyName
        
        Write-Host "✅ $Computer abgeschlossen" -ForegroundColor Green
    }
    catch {
        Write-Host "❌ $Computer fehlgeschlagen: $_" -ForegroundColor Red
    }
}
```

### 2. Gesundheitsprüfungs-Script
```powershell
# Check-Inventory-Health.ps1
$Computers = Get-ADComputer -Filter * | Select-Object -ExpandProperty Name

foreach ($Computer in $Computers) {
    if (Test-Connection -ComputerName $Computer -Count 1 -Quiet) {
        $LogFile = "\\$Computer\C$\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"
        
        if (Test-Path $LogFile) {
            $LastRun = (Get-Item $LogFile).LastWriteTime
            $DaysOld = (Get-Date) - $LastRun
            
            if ($DaysOld.Days -gt 7) {
                Write-Host "$Computer - Inventarisierung veraltet ($($DaysOld.Days) Tage)" -ForegroundColor Yellow
            } else {
                Write-Host "$Computer - Inventarisierung aktuell" -ForegroundColor Green
            }
        } else {
            Write-Host "$Computer - Keine Inventarisierungs-Log gefunden" -ForegroundColor Red
        }
    }
}
```

## Überwachung und Reporting

### 1. Log-Analyse-Script
```powershell
# Analyze-Inventory-Logs.ps1
$LogPath = "C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"

if (Test-Path $LogPath) {
    $Logs = Get-Content $LogPath
    
    $Errors = $Logs | Where-Object { $_ -like "*[ERROR]*" }
    $Warnings = $Logs | Where-Object { $_ -like "*[WARN]*" }
    $Success = $Logs | Where-Object { $_ -like "*[SUCCESS]*" }
    
    Write-Host "=== Inventarisierungs-Log-Analyse ===" -ForegroundColor Cyan
    Write-Host "Erfolgreiche Ereignisse: $($Success.Count)" -ForegroundColor Green
    Write-Host "Warnungen: $($Warnings.Count)" -ForegroundColor Yellow
    Write-Host "Fehler: $($Errors.Count)" -ForegroundColor Red
    
    if ($Errors.Count -gt 0) {
        Write-Host "`nNeueste Fehler:" -ForegroundColor Red
        $Errors | Select-Object -Last 5 | ForEach-Object { Write-Host "  $_" }
    }
}
```

### 2. Performance-Überwachung
```powershell
# Monitor-Inventory-Performance.ps1
$StartTime = Get-Date

# Inventarisierung ausführen
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Performance Test"

$EndTime = Get-Date
$Duration = $EndTime - $StartTime

Write-Host "Inventarisierungs-Performance-Bericht" -ForegroundColor Cyan
Write-Host "Startzeit: $StartTime"
Write-Host "Endzeit: $EndTime"
Write-Host "Dauer: $($Duration.TotalMinutes) Minuten"

# In Performance-Datenbank protokollieren
$PerfData = @{
    Computer = $env:COMPUTERNAME
    StartTime = $StartTime
    EndTime = $EndTime
    Duration = $Duration.TotalMinutes
    Date = (Get-Date).Date
}

# Nach CSV exportieren zur Verfolgung
$PerfData | Export-Csv -Path "C:\Logs\InventoryPerformance.csv" -Append -NoTypeInformation
```

## Fehlerbehebung bei Bereitstellungen

### Häufige Probleme und Lösungen

#### 1. Ausführungsrichtlinien-Einschränkungen
```powershell
# Lösung: Ausführungsrichtlinie im Bereitstellungs-Script setzen
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

#### 2. Netzwerkverbindungsprobleme
```powershell
# Verbindung vor Bereitstellung testen
if (Test-NetConnection snipeit.firma.com -Port 443 -WarningAction SilentlyContinue) {
    # Mit Inventarisierung fortfahren
} else {
    Write-Error "Verbindung zum SnipeIT-Server nicht möglich"
    exit 1
}
```

#### 3. Berechtigungsprobleme
```powershell
# Prüfen ob als Administrator ausgeführt wird
if (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Warning "Script benötigt Administrator-Rechte für vollständige Hardware-Erkennung"
    # Mit begrenzter Erkennung fortfahren oder beenden
}
```

---

## Best Practices

1. **Immer zuerst in Nicht-Produktionsumgebung testen**
2. **Konfigurationsmanagement für API-Token verwenden**
3. **Log-Dateien auf Probleme überwachen**
4. **Regelmäßige Inventarisierungs-Updates planen**
5. **Gesundheitsprüfungen und Alarme implementieren**
6. **Bereitstellungs-Scripts versionskontrolliert verwalten**

Für zusätzliche Bereitstellungsunterstützung siehe die [Installationsanleitung](INSTALLATION-DE.md) oder öffnen Sie ein Issue auf GitHub.