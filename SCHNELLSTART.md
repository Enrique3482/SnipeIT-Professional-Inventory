# Schnelle Installationsanleitung 🚀

Eine einfache Schritt-für-Schritt-Anleitung zur Installation des SnipeIT Professional Inventory Systems.

## 📋 Was Sie benötigen

- **Windows 10/11** oder **Windows Server**
- **PowerShell 5.1+** (bereits in Windows enthalten)
- **SnipeIT** Server mit API-Zugang
- **Administrator-Rechte** auf dem Computer

## ⚡ 5-Minuten Installation

### Schritt 1: Herunterladen
```powershell
# Öffnen Sie PowerShell als Administrator und führen Sie aus:
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

**Oder manuell:**
1. Gehen Sie zu: https://github.com/Enrique3482/SnipeIT-Professional-Inventory
2. Klicken Sie auf "Code" → "Download ZIP"
3. Entpacken Sie die Datei in einen Ordner

### Schritt 2: SnipeIT API-Token holen
1. Melden Sie sich bei Ihrem SnipeIT an
2. Gehen Sie zu **Konto-Einstellungen** → **API-Schlüssel**
3. Klicken Sie auf **"Neuen Token erstellen"**
4. Kopieren Sie den Token

### Schritt 3: Konfiguration anpassen
Öffnen Sie `SnipeConfig.json` und ändern Sie:

```json
{
  "Snipe": {
    "Token": "HIER-IHREN-API-TOKEN-EINFÜGEN",
    "Url": "https://ihr-snipeit-server.com/api/v1",
    "StandardCompanyName": "Ihr Firmenname"
  }
}
```

### Schritt 4: Testen
```powershell
# Testlauf ohne echte Änderungen
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Schritt 5: Produktionslauf
```powershell
# Echter Lauf - erstellt Assets in SnipeIT
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Ihre Firma"
```

## ✅ Erfolgskontrolle

Nach dem Lauf sollten Sie sehen:
- ✅ Grüne "SUCCESS" Meldungen
- ✅ Ihr Computer als Asset in SnipeIT
- ✅ Hardware-Details in den benutzerdefinierten Feldern

## 🆘 Häufige Probleme

### "Ausführungsrichtlinie" Fehler
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### "API 401 Unauthorized"
- Prüfen Sie Ihren API-Token
- Stellen Sie sicher, dass die URL korrekt ist (mit `/api/v1` am Ende)

### "Keine Hardware erkannt"
- Führen Sie PowerShell als Administrator aus
- Stellen Sie sicher, dass WMI funktioniert

## 📁 Was passiert?

Das Script:
1. 🔍 **Erkennt** Ihre Hardware automatisch (CPU, RAM, Festplatten, etc.)
2. 🖥️ **Findet** externe Monitore und Docking-Stationen
3. 📊 **Erstellt** oder **aktualisiert** Assets in SnipeIT
4. 📝 **Füllt** alle Hardware-Details in benutzerdefinierte Felder
5. 👤 **Ordnet** den Computer automatisch dem Benutzer zu

## 🔄 Regelmäßige Ausführung

Für automatische wöchentliche Scans:
```powershell
# Aufgabe erstellen (als Administrator)
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File 'C:\Pfad\zum\Script\SnipeIT-Professional-Inventory.ps1'"
$trigger = New-ScheduledTaskTrigger -Weekly -At "02:00" -DaysOfWeek Monday
Register-ScheduledTask -TaskName "SnipeIT Inventur" -Action $action -Trigger $trigger -User "SYSTEM"
```

## 📞 Hilfe benötigt?

- 📖 **Vollständige Dokumentation**: [README.md](README.md)
- 🐛 **Bug melden**: [GitHub Issues](../../issues)
- 💬 **Fragen stellen**: [GitHub Discussions](../../discussions)
- 📧 **E-Mail Support**: henrique.sebastiao@example.com

---

**🎯 Tipp**: Führen Sie das Script zuerst mit `-TestMode` aus, um zu sehen, was erkannt wird, ohne tatsächliche Änderungen zu machen!