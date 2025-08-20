# SnipeIT Professional Inventory System - README (DE)

![Version](https://img.shields.io/badge/Version-2.2.0-blue.svg)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1+-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Platform](https://img.shields.io/badge/Platform-Windows-lightgrey.svg)

## 🎯 Überblick

Das **SnipeIT Professional Inventory System** ist eine enterprise-grade Lösung für die automatisierte Hardware-Inventarisierung mit vollständiger Snipe-IT Integration. Version 2.2.0 führt eine komplette VS Code Workspace-Integration und benutzerfreundliche One-Click-Deployment-Optionen ein.

### 🌟 Hauptmerkmale

- **🔄 Automatische Hardware-Erkennung**: Vollständige Systemanalyse mit intelligenter Komponenten-Identifikation
- **🔗 Nahtlose Snipe-IT Integration**: Direkte API-Synchronisation mit erweiterten Custom Fields
- **🖥️ VS Code Workspace**: Professionelle Entwicklungsumgebung mit integrierten Tasks und Debugging
- **🚀 One-Click Deployment**: Batch-Dateien für sofortige Ausführung ohne Kommandozeilen-Kenntnisse
- **🛡️ Test-/Produktionsmodus**: Sichere Testumgebung mit vollständiger API-Simulation
- **📊 Erweiterte Berichterstattung**: Detaillierte Logs und Ausführungsberichte
- **🔧 Automatische Wartungsverfolgung**: Intelligent Asset-Lifecycle-Management

## 📦 Schnellstart

### Option 1: One-Click für Endbenutzer
```bash
# 1. Doppelklick auf eine der Batch-Dateien:
Test-Modus.bat           # Für sichere Tests ohne API-Aufrufe
Produktions-Modus.bat    # Für Live-Umgebung mit echten API-Aufrufen
Interaktiv-Starten.bat   # Für geführte Ausführung mit Menü
```

### Option 2: PowerShell für Power-User
```powershell
# Intelligenter Starter mit interaktivem Menü
.\Start-SnipeInventory.ps1

# Direkte Ausführung mit Parametern
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Option 3: VS Code für Entwickler
```bash
# Workspace öffnen
code workspace.code-workspace

# Integrierte Tasks verwenden:
# Strg+Shift+P → "Tasks: Run Task" → Gewünschten Modus auswählen
```

---

**Entwickelt mit ❤️ für professionelle IT-Asset-Verwaltung**

*Letzte Aktualisierung: 20. August 2025 | Version 2.2.0*