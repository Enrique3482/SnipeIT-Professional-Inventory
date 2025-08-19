# Quick Start Guide 🚀

A simple step-by-step guide to install the SnipeIT Professional Inventory System.

## 📋 What You Need

- **Windows 10/11** or **Windows Server**
- **PowerShell 5.1+** (included with Windows)
- **SnipeIT** server with API access
- **Administrator privileges** on the computer

## ⚡ 5-Minute Installation

### Step 1: Download
```powershell
# Open PowerShell as Administrator and run:
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

**Or manually:**
1. Go to: https://github.com/Enrique3482/SnipeIT-Professional-Inventory
2. Click "Code" → "Download ZIP"
3. Extract the file to a folder

### Step 2: Get SnipeIT API Token
1. Log into your SnipeIT instance
2. Go to **Account Settings** → **API Keys**
3. Click **"Create New Token"**
4. Copy the token

### Step 3: Configure Settings
Open `SnipeConfig.json` and update:

```json
{
  "Snipe": {
    "Token": "PASTE-YOUR-API-TOKEN-HERE",
    "Url": "https://your-snipeit-server.com/api/v1",
    "StandardCompanyName": "Your Company Name"
  }
}
```

### Step 4: Test Run
```powershell
# Test run without making real changes
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Step 5: Production Run
```powershell
# Real run - creates assets in SnipeIT
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Your Company"
```

## ✅ Success Check

After running, you should see:
- ✅ Green "SUCCESS" messages
- ✅ Your computer as an asset in SnipeIT
- ✅ Hardware details in custom fields

## 🆘 Common Issues

### "Execution Policy" Error
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### "API 401 Unauthorized"
- Check your API token
- Ensure URL is correct (with `/api/v1` at the end)

### "No Hardware Detected"
- Run PowerShell as Administrator
- Ensure WMI is working

## 📁 What Happens?

The script:
1. 🔍 **Detects** your hardware automatically (CPU, RAM, drives, etc.)
2. 🖥️ **Finds** external monitors and docking stations
3. 📊 **Creates** or **updates** assets in SnipeIT
4. 📝 **Populates** all hardware details in custom fields
5. 👤 **Assigns** the computer to the user automatically

## 🔄 Regular Execution

For automatic weekly scans:
```powershell
# Create task (run as Administrator)
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File 'C:\Path\to\Script\SnipeIT-Professional-Inventory.ps1'"
$trigger = New-ScheduledTaskTrigger -Weekly -At "02:00" -DaysOfWeek Monday
Register-ScheduledTask -TaskName "SnipeIT Inventory" -Action $action -Trigger $trigger -User "SYSTEM"
```

## 📞 Need Help?

- 📖 **Full Documentation**: [README.md](README.md)
- 🐛 **Report Bugs**: [GitHub Issues](../../issues)
- 💬 **Ask Questions**: [GitHub Discussions](../../discussions)
- 📧 **Email Support**: henrique.sebastiao@me.com

---

**🎯 Tip**: Run the script with `-TestMode` first to see what gets detected without making actual changes!