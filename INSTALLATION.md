# Installation Guide 📋

This guide will walk you through setting up the SnipeIT Professional Inventory System.

## Prerequisites ✅

### System Requirements

| Platform | Status | PowerShell | Hardware Detection | Support Level |
|----------|--------|------------|-------------------|---------------|
| **Windows 10/11** | ✅ **Fully Supported** | Windows PowerShell 5.1+ | ✅ Complete | **Recommended** |
| **Windows Server** | ✅ **Fully Supported** | Windows PowerShell 5.1+ | ✅ Complete | Production Ready |
| **Linux (Ubuntu/Debian)** | ⚠️ **Experimental** | PowerShell Core 7+ | ❌ **Windows Commands Required** | Limited |
| **macOS** | ⚠️ **Experimental** | PowerShell Core 7+ | ❌ **Windows Commands Required** | Limited |

### Windows Requirements (Recommended)
- **PowerShell 5.1+** (Windows PowerShell or PowerShell Core)
- **Windows 10** (Build 1809+) or **Windows Server 2016+**
- **SnipeIT instance** with API access
- **Administrator privileges** for complete hardware detection
- **Network connectivity** to your SnipeIT server

### Linux/macOS Requirements (Experimental)
- **PowerShell Core 7.0+** - [Installation Guide](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)
- **Ubuntu 20.04+**, **Debian 11+**, or **macOS 10.15+**
- **SnipeIT instance** with API access
- **Sudo privileges** for PowerShell Core installation
- **⚠️ Limited functionality** - Hardware detection requires Windows commands

### SnipeIT Requirements
- SnipeIT v5.0+ recommended
- API token with the following permissions:
  - Read/Write access to Assets
  - Read/Write access to Categories
  - Read/Write access to Custom Fields
  - Read/Write access to Models
  - Read/Write access to Manufacturers

## Platform-Specific Installation 🖥️

### Windows Installation (Recommended) 🪟

#### Step 1: Download the Script
```powershell
# Clone the repository
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

#### Step 2: Configure SnipeIT Connection
1. Open `SnipeConfig.json` in a text editor
2. Update with your SnipeIT details:

```json
{
  "Snipe": {
    "Token": "YOUR-API-TOKEN-HERE",
    "Url": "https://your-snipeit-instance.com/api/v1",
    "StandardCompanyName": "Your Company Name"
  }
}
```

#### Step 3: Test Configuration
```powershell
# Test mode with full hardware detection
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Linux Installation (Experimental) 🐧

#### Step 1: Install PowerShell Core
```bash
# Ubuntu/Debian
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y powershell

# Verify installation
pwsh --version
```

#### Step 2: Download Script
```bash
git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
cd SnipeIT-Professional-Inventory
```

#### Step 3: Configure SnipeIT
```bash
# Edit configuration
nano SnipeConfig.json
```

#### Step 4: Test with Simulation (Required)
```bash
# Hardware detection will fail on Linux - use simulation
pwsh -File SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging
```

#### ⚠️ **Critical Linux Limitations**
The following Windows commands will **FAIL** on Linux:
```powershell
Get-CimInstance Win32_ComputerSystem     # ❌ Windows WMI only
Get-CimInstance Win32_BIOS               # ❌ Windows WMI only
Get-NetAdapter                           # ❌ Windows networking
$env:COMPUTERNAME                        # ❌ Windows environment
HKLM:\SOFTWARE\Microsoft                 # ❌ Windows Registry
```

**Alternative Linux commands needed for hardware detection:**
```bash
lshw -short                  # Hardware listing
dmidecode -s system-serial   # Serial number
lscpu                        # CPU information
free -h                      # Memory information
lsblk                        # Storage devices
ip addr show                 # Network interfaces
hostnamectl                  # System information
```

### macOS Installation (Experimental) 🍎

#### Step 1: Install PowerShell Core
```bash
# Using Homebrew
brew install --cask powershell

# Verify installation
pwsh --version
```

#### Step 2: Follow Linux-style configuration
- Use simulated hardware (`-SimulateHardware`)
- Limited hardware detection capabilities
- Windows commands will not work

## Step 2: Get Your API Token 🔑

1. Log in to your SnipeIT instance
2. Go to **Account Settings** → **API Keys**
3. Click **Create New Token**
4. Copy the generated token

## Step 3: Test the Configuration 🧪

### Windows Testing
```powershell
# Full hardware detection test
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging

# Test with simulated hardware
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging
```

### Linux/macOS Testing
```bash
# Only simulation works on non-Windows platforms
pwsh -File SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging
```

You should see:
- ✅ Configuration loaded successfully
- ✅ API connection test passed
- ✅ Hardware detection working (Windows) / Simulation working (Linux/macOS)
- ✅ Field mapping validated

## Step 4: First Production Run 🚀

### Windows Production
```powershell
# Standard production run
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Your Company"

# With verbose logging for troubleshooting
.\SnipeIT-Professional-Inventory.ps1 -VerboseLogging
```

### Linux/macOS Production (Limited)
```bash
# Production with simulation (no real hardware detection)
pwsh -File SnipeIT-Professional-Inventory.ps1 -SimulateHardware -CustomerName "Your Company"
```

## Step 5: Verify Results ✔️

### Check SnipeIT
1. Log in to your SnipeIT instance
2. Navigate to **Assets** → **All Assets**
3. Look for your computer asset
4. Verify custom fields are populated
5. Check for external monitors (Windows only)

### Check Logs
Review logs for any issues:
- **Windows**: `C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log`
- **Linux/macOS**: `./logs/SnipeIT-Inventory.log`

## Advanced Configuration 🔬

### Custom Field Setup
The system automatically creates and maps custom fields. To customize:

1. Review the `CustomFieldMapping` section in `SnipeConfig.json`
2. Modify field names if needed
3. Test changes with `-TestMode`

### Windows Automatic Deployment
For enterprise deployment via Group Policy or SCCM:

```powershell
# Deploy command
powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "Enterprise"
```

### Linux Automatic Deployment
```bash
# Cron job example (simulation only)
0 2 * * * /usr/bin/pwsh -File /opt/scripts/SnipeIT-Professional-Inventory.ps1 -SimulateHardware
```

### Windows Scheduled Execution
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

#### PowerShell Core Installation (Linux/macOS)
```bash
# Ubuntu/Debian - if installation fails
sudo apt-get update
sudo apt-get install -y wget apt-transport-https software-properties-common

# Verify PowerShell version
pwsh --version
```

#### Hardware Detection Failures (Linux/macOS)
```
Error: Get-CimInstance command not found
```
**Solution**: This is expected on non-Windows platforms. Use `-SimulateHardware`:
```bash
pwsh -File SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware
```

#### API Authentication Errors
```
Error: 401 Unauthorized
```
**Solution**: Verify your API token and permissions

#### PowerShell Execution Policy (Windows)
```
Error: Execution policy restriction
```
**Solution**: Run as Administrator and set execution policy:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

#### Hardware Detection Issues (Windows)
```
Warning: Limited hardware information detected
```
**Solution**: Ensure running with Administrator privileges

#### SnipeIT Connection Issues
```
Error: Unable to connect to SnipeIT
```
**Solution**: Check network connectivity:
```powershell
# Windows
Test-NetConnection your-snipeit-server.com -Port 443

# Linux/macOS
curl -I https://your-snipeit-server.com
```

### Platform-Specific Troubleshooting

#### Windows
- Check Windows Event Logs
- Verify WMI service is running
- Ensure all Windows updates are installed

#### Linux
- Check system logs: `journalctl -u powershell`
- Verify PowerShell Core installation: `pwsh --version`
- Use simulation mode for testing: `-SimulateHardware`

#### macOS
- Check Console app for PowerShell errors
- Verify Homebrew installation
- Use simulation mode exclusively

### Getting Help

1. **Check the logs** in platform-appropriate location
2. **Run with verbose logging** using `-VerboseLogging`
3. **Test in safe mode** using `-TestMode`
4. **Use simulation** on non-Windows platforms
5. **Submit an issue** on GitHub with log excerpts

## Security Considerations 🔒

### API Token Security
- Store `SnipeConfig.json` securely
- Use least-privilege API tokens
- Consider using environment variables for sensitive data
- Regularly rotate API tokens

### Platform-Specific Security

#### Windows
- Run with appropriate service accounts
- Use Group Policy for deployment
- Enable PowerShell logging

#### Linux/macOS
- Secure script files with proper permissions: `chmod 750`
- Use sudo for installation only
- Consider containerization for isolation

### Network Security
- Use HTTPS for SnipeIT connections
- Consider VPN for remote deployments
- Validate SSL certificates

## Next Steps 📈

After successful installation:

### For Windows Deployments
1. **Schedule regular runs** for ongoing inventory
2. **Deploy via Group Policy/SCCM** for enterprise scale
3. **Monitor Windows Event Logs** for issues
4. **Set up maintenance schedules** in SnipeIT

### For Linux/macOS Deployments
1. **Use simulation mode** for testing SnipeIT integration
2. **Plan for Windows VMs** or **Windows-based agents** for actual hardware detection
3. **Consider hybrid deployment** strategies
4. **Monitor system logs** for PowerShell Core issues

### Universal Next Steps
1. **Customize field mappings** as needed
2. **Train staff** on asset management workflows
3. **Document deployment procedures**
4. **Plan for future PowerShell updates**

---

## Platform Summary 📊

| Feature | Windows | Linux | macOS |
|---------|---------|-------|-------|
| **PowerShell Support** | ✅ Native | ✅ Core 7+ | ✅ Core 7+ |
| **Hardware Detection** | ✅ Full WMI | ❌ Simulation Only | ❌ Simulation Only |
| **API Integration** | ✅ Complete | ✅ Complete | ✅ Complete |
| **Production Ready** | ✅ Yes | ⚠️ Limited | ⚠️ Limited |
| **Enterprise Deployment** | ✅ GP/SCCM | ⚠️ Manual | ⚠️ Manual |

**Need Help?** 
- 📖 [Documentation](../../wiki)
- 🐛 [Report Issues](../../issues)
- 💬 [Discussions](../../discussions)