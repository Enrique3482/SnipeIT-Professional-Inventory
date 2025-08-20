# SnipeIT Professional Inventory System - README (EN)

![Version](https://img.shields.io/badge/Version-2.2.0-blue.svg)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1+-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Platform](https://img.shields.io/badge/Platform-Windows-lightgrey.svg)

## 🎯 Overview

The **SnipeIT Professional Inventory System** is an enterprise-grade solution for automated hardware inventory with complete Snipe-IT integration. Version 2.2.0 introduces complete VS Code workspace integration and user-friendly one-click deployment options.

### 🌟 Key Features

- **🔄 Automatic Hardware Detection**: Complete system analysis with intelligent component identification
- **🔗 Seamless Snipe-IT Integration**: Direct API synchronization with extended custom fields
- **🖥️ VS Code Workspace**: Professional development environment with integrated tasks and debugging
- **🚀 One-Click Deployment**: Batch files for immediate execution without command line knowledge
- **🛡️ Test/Production Mode**: Safe test environment with complete API simulation
- **📊 Extended Reporting**: Detailed logs and execution reports
- **🔧 Automatic Maintenance Tracking**: Intelligent asset lifecycle management

## 📦 Quick Start

### Option 1: One-Click for End Users
```bash
# 1. Double-click one of the batch files:
Test-Modus.bat           # For safe testing without API calls
Produktions-Modus.bat    # For live environment with real API calls
Interaktiv-Starten.bat   # For guided execution with menu
```

### Option 2: PowerShell for Power Users
```powershell
# Intelligent starter with interactive menu
.\Start-SnipeInventory.ps1

# Direct execution with parameters
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Option 3: VS Code for Developers
```bash
# Open workspace
code workspace.code-workspace

# Use integrated tasks:
# Ctrl+Shift+P → "Tasks: Run Task" → Select desired mode
```

---

**Developed with ❤️ for professional IT asset management**

*Last updated: August 20, 2025 | Version 2.2.0*