# Installation Guide 📋

This guide will walk you through setting up the SnipeIT Professional Inventory System.

## Prerequisites ✅

### System Requirements
- **PowerShell 5.1+** (Windows PowerShell or PowerShell Core)
- **Windows 10** (Build 1809+) or **Windows Server 2016+**
- **SnipeIT instance** with API access
- **Administrator privileges** for complete hardware detection
- **Network connectivity** to your SnipeIT server

### SnipeIT Requirements
- SnipeIT v5.0+ recommended
- API token with the following permissions:
  - Read/Write access to Assets
  - Read/Write access to Categories
  - Read/Write access to Custom Fields
  - Read/Write access to Models
  - Read/Write access to Manufacturers

## Step 1: Download the Script 📥

### Option A: Git Clone (Recommended)
```powershell
# Clone the repository
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

### Option B: Direct Download
1. Download the ZIP file from GitHub
2. Extract to a folder (e.g., `C:\Scripts\SnipeIT-Inventory\`)
3. Open PowerShell as Administrator
4. Navigate to the extracted folder

## Step 2: Configure SnipeIT Connection 🔧

### Get Your API Token
1. Log in to your SnipeIT instance
2. Go to **Account Settings** → **API Keys**
3. Click **Create New Token**
4. Copy the generated token

### Update Configuration File
1. Open `SnipeConfig.json` in a text editor
2. Update the following values:

```json
{
  "Snipe": {
    "Token": "YOUR-API-TOKEN-HERE",
    "Url": "https://your-snipeit-instance.com/api/v1",
    "StandardCompanyName": "Your Company Name"
  }
}
```

**Important**: Replace:
- `YOUR-API-TOKEN-HERE` with your actual API token
- `https://your-snipeit-instance.com/api/v1` with your SnipeIT URL
- `Your Company Name` with your organization's name

## Step 3: Test the Configuration 🧪

Before running in production, test your setup:

```powershell
# Run in test mode first
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging

# Test with simulated hardware
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging
```

You should see:
- ✅ Configuration loaded successfully
- ✅ API connection test passed
- ✅ Hardware detection working
- ✅ Field mapping validated

## Step 4: First Production Run 🚀

Once testing is successful:

```powershell
# Standard production run
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Your Company"

# With verbose logging for troubleshooting
.\SnipeIT-Professional-Inventory.ps1 -VerboseLogging
```

## Step 5: Verify Results ✔️

### Check SnipeIT
1. Log in to your SnipeIT instance
2. Navigate to **Assets** → **All Assets**
3. Look for your computer asset
4. Verify custom fields are populated
5. Check for external monitors (if detected)

### Check Logs
Review logs for any issues:
- **Main Log**: `C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log`
- **Error Log**: `C:\ProgramData\SnipeIT\Errorlog\SnipeIT-Errors.log`

## Advanced Configuration 🔬

### Custom Field Setup
The system automatically creates and maps custom fields. To customize:

1. Review the `CustomFieldMapping` section in `SnipeConfig.json`
2. Modify field names if needed
3. Test changes with `-TestMode`

### Automatic Deployment
For enterprise deployment via Group Policy or SCCM:

```powershell
# Deploy command
powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "Enterprise"
```

### Scheduled Execution
Create a scheduled task to run inventory regularly:

```powershell
# Create scheduled task (run as Administrator)
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File 'C:\Scripts\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1'"
$trigger = New-ScheduledTaskTrigger -Daily -At "2:00AM"
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries
Register-ScheduledTask -TaskName "SnipeIT Inventory" -Action $action -Trigger $trigger -Settings $settings -User "SYSTEM"
```

## Troubleshooting 🔧

### Common Issues

#### API Authentication Errors
```
Error: 401 Unauthorized
```
**Solution**: Verify your API token and permissions

#### PowerShell Execution Policy
```
Error: Execution policy restriction
```
**Solution**: Run as Administrator and set execution policy:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

#### Hardware Detection Issues
```
Warning: Limited hardware information detected
```
**Solution**: Ensure running with Administrator privileges

#### SnipeIT Connection Issues
```
Error: Unable to connect to SnipeIT
```
**Solution**: Check network connectivity and URL:
```powershell
Test-NetConnection your-snipeit-server.com -Port 443
```

### Getting Help

1. **Check the logs** in `C:\ProgramData\SnipeIT\`
2. **Run with verbose logging** using `-VerboseLogging`
3. **Test in safe mode** using `-TestMode`
4. **Submit an issue** on GitHub with log excerpts

## Security Considerations 🔒

### API Token Security
- Store `SnipeConfig.json` securely
- Use least-privilege API tokens
- Consider using environment variables for sensitive data
- Regularly rotate API tokens

### Network Security
- Use HTTPS for SnipeIT connections
- Consider VPN for remote deployments
- Validate SSL certificates

## Next Steps 📈

After successful installation:

1. **Schedule regular runs** for ongoing inventory
2. **Monitor logs** for any issues
3. **Customize field mappings** as needed
4. **Train staff** on asset management workflows
5. **Set up maintenance schedules** in SnipeIT

---

**Need Help?** 
- 📖 [Documentation](../../wiki)
- 🐛 [Report Issues](../../issues)
- 💬 [Discussions](../../discussions)