# Deployment Examples 🚀

This document provides various deployment scenarios for the SnipeIT Professional Inventory System across different platforms.

## Platform Compatibility Overview

| Platform | Deployment Type | Hardware Detection | Limitations |
|----------|----------------|-------------------|-------------|
| **Windows 10/11** | ✅ Full Production | ✅ Complete WMI/CIM | None |
| **Windows Server** | ✅ Enterprise Ready | ✅ Complete WMI/CIM | None |
| **Linux (Ubuntu/Debian)** | ⚠️ Limited/Simulation | ❌ Windows Commands Required | Simulation mode only |
| **macOS** | ⚠️ Limited/Simulation | ❌ Windows Commands Required | Simulation mode only |

## Quick Start Deployment

### Windows Quick Start
```powershell
# Test on current Windows computer
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging

# Production run with custom company name
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Acme Corporation"
```

### Linux/macOS Quick Start
```bash
# Test with simulation (no real hardware detection)
pwsh -File SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging

# Production simulation mode
pwsh -File SnipeIT-Professional-Inventory.ps1 -SimulateHardware -CustomerName "Acme Corporation"
```

## Windows Enterprise Deployment Scenarios

### 1. Group Policy Deployment

Create a Group Policy to deploy across domain computers:

**GPO Script (PowerShell):**
```powershell
# Deploy-SnipeIT-Inventory.ps1
param(
    [string]$CompanyName = "Enterprise Corporation"
)

# Set execution policy for this session
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force

# Define script path
$ScriptPath = "\\server\share\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1"
$ConfigPath = "\\server\share\SnipeIT-Inventory\SnipeConfig.json"

# Check if script exists
if (Test-Path $ScriptPath) {
    try {
        # Execute inventory script with full hardware detection
        & $ScriptPath -ConfigurationFile $ConfigPath -CustomerName $CompanyName -VerboseLogging
        
        # Log success
        Write-EventLog -LogName Application -Source "SnipeIT Inventory" -EventId 1001 -Message "Inventory completed successfully on $env:COMPUTERNAME"
    }
    catch {
        # Log error
        Write-EventLog -LogName Application -Source "SnipeIT Inventory" -EventId 1002 -EntryType Error -Message "Inventory failed on $env:COMPUTERNAME: $_"
    }
}
```

### 2. SCCM Deployment

**Package Configuration:**
- **Program**: `powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "%%COMPANYNAME%%"`
- **Run Mode**: Run with administrative rights
- **After running**: No action required
- **Requirements**: Windows 10+ with PowerShell 5.1+

**Detection Method:**
```powershell
# Check if inventory was run in last 7 days
$LogPath = "C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"
if (Test-Path $LogPath) {
    $LastWrite = (Get-Item $LogPath).LastWriteTime
    if ($LastWrite -gt (Get-Date).AddDays(-7)) {
        Write-Output "Inventory current"
    }
}
```

### 3. Windows Scheduled Task Deployment

**Create via PowerShell:**
```powershell
# Create-InventoryTask.ps1
$TaskName = "SnipeIT Professional Inventory"
$ScriptPath = "C:\Scripts\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1"
$CompanyName = "Your Company Name"

# Task action - Full hardware detection on Windows
$Action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -WindowStyle Hidden -File '$ScriptPath' -CustomerName '$CompanyName'"

# Task trigger (daily at 2 AM)
$Trigger = New-ScheduledTaskTrigger -Daily -At "02:00"

# Task settings
$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries -StartWhenAvailable

# Task principal (run as SYSTEM for full hardware access)
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

# Register the task
Register-ScheduledTask -TaskName $TaskName -Action $Action -Trigger $Trigger -Settings $Settings -Principal $Principal -Description "Automated SnipeIT inventory scan with full hardware detection"

Write-Host "Scheduled task '$TaskName' created successfully" -ForegroundColor Green
```

## Cross-Platform Container Deployment

### 4. Docker Container Deployment (Linux with PowerShell Core)

**⚠️ Note**: Container deployment provides API integration but **no real hardware detection**

**Dockerfile:**
```dockerfile
FROM mcr.microsoft.com/powershell:7.3-ubuntu-22.04

# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy scripts
COPY . .

# Set permissions
RUN chmod +x SnipeIT-Professional-Inventory.ps1

# Note: Hardware detection requires Windows - use simulation mode
CMD ["pwsh", "-File", "SnipeIT-Professional-Inventory.ps1", "-SimulateHardware", "-TestMode"]
```

**docker-compose.yml:**
```yaml
version: '3.8'
services:
  snipeit-inventory:
    build: .
    environment:
      - SNIPEIT_URL=https://your-snipeit.com/api/v1
      - SNIPEIT_TOKEN=your-token
      - COMPANY_NAME=Your Company
    volumes:
      - ./logs:/app/logs
      - ./config:/app/config
    # Note: Simulation mode only - no real hardware detection in containers
```

### 5. Linux Cron Job Deployment

**⚠️ Limitation**: Linux deployment uses simulation mode only

```bash
#!/bin/bash
# /opt/scripts/snipeit-inventory.sh

# Set PowerShell Core path
PWSH_PATH="/usr/bin/pwsh"
SCRIPT_PATH="/opt/scripts/SnipeIT-Professional-Inventory.ps1"
COMPANY_NAME="Your Linux Company"

# Check if PowerShell Core is available
if [ ! -f "$PWSH_PATH" ]; then
    echo "PowerShell Core not found. Please install PowerShell Core 7+"
    exit 1
fi

# Run inventory with simulation (Linux cannot detect Windows hardware)
$PWSH_PATH -File "$SCRIPT_PATH" -SimulateHardware -CustomerName "$COMPANY_NAME" -VerboseLogging

# Log completion
echo "$(date): SnipeIT inventory simulation completed" >> /var/log/snipeit-inventory.log
```

**Crontab entry:**
```bash
# Run daily at 2 AM (simulation mode only)
0 2 * * * /opt/scripts/snipeit-inventory.sh >> /var/log/snipeit-inventory.log 2>&1
```

## Configuration Templates

### 1. Windows Development Environment
```json
{
  "Snipe": {
    "Url": "http://localhost:8000/api/v1",
    "Token": "dev-token-here",
    "StandardCompanyName": "Development Company"
  },
  "TestMode": {
    "IsPresent": true
  },
  "VerboseLogging": {
    "IsPresent": true
  },
  "Platform": "Windows",
  "HardwareDetection": "Full"
}
```

### 2. Windows Production Environment
```json
{
  "Snipe": {
    "Url": "https://assets.company.com/api/v1",
    "Token": "production-token-here",
    "StandardCompanyName": "Production Company"
  },
  "Performance": {
    "ApiTimeout": 120,
    "MaxRetries": 5,
    "RetryDelay": 3
  },
  "TestMode": {
    "IsPresent": false
  },
  "Platform": "Windows",
  "HardwareDetection": "Full"
}
```

### 3. Linux/macOS Simulation Environment
```json
{
  "Snipe": {
    "Url": "https://assets.company.com/api/v1",
    "Token": "simulation-token-here",
    "StandardCompanyName": "Simulation Company"
  },
  "SimulateHardware": {
    "IsPresent": true
  },
  "TestMode": {
    "IsPresent": true
  },
  "Platform": "Linux",
  "HardwareDetection": "Simulation",
  "Limitations": "Windows commands not available"
}
```

### 4. Multi-Tenant Environment
```json
{
  "Snipe": {
    "Url": "https://tenant1.snipeit-cloud.com/api/v1",
    "Token": "tenant1-token",
    "StandardCompanyName": "Tenant 1 Corporation"
  },
  "CustomFieldMapping": {
    "_snipeit_tenant_id_1": "TenantId",
    "_snipeit_location_code_2": "LocationCode",
    "_snipeit_platform_3": "Platform"
  }
}
```

## Platform-Specific Automation Scripts

### 1. Windows Bulk Deployment Script
```powershell
# Deploy-Multiple-Windows-Computers.ps1
param(
    [string[]]$ComputerNames,
    [string]$CompanyName = "Enterprise",
    [PSCredential]$Credential
)

foreach ($Computer in $ComputerNames) {
    Write-Host "Deploying to $Computer..." -ForegroundColor Yellow
    
    try {
        Invoke-Command -ComputerName $Computer -Credential $Credential -ScriptBlock {
            param($Company)
            
            # Download and run script with full Windows hardware detection
            $ScriptUrl = "https://raw.githubusercontent.com/Enrique3482/SnipeIT-Professional-Inventory/main/SnipeIT-Professional-Inventory.ps1"
            $Script = Invoke-WebRequest -Uri $ScriptUrl -UseBasicParsing
            
            # Execute with full hardware detection (Windows only)
            Invoke-Expression $Script.Content -CustomerName $Company
        } -ArgumentList $CompanyName
        
        Write-Host "✅ $Computer completed (Full hardware detection)" -ForegroundColor Green
    }
    catch {
        Write-Host "❌ $Computer failed: $_" -ForegroundColor Red
    }
}
```

### 2. Linux Simulation Deployment Script
```bash
#!/bin/bash
# deploy-linux-simulation.sh

COMPUTERS=("server1" "server2" "server3")
COMPANY_NAME="Linux Company"
SSH_KEY="/home/admin/.ssh/id_rsa"

for computer in "${COMPUTERS[@]}"; do
    echo "Deploying simulation to $computer..."
    
    # Copy script to remote server
    scp -i "$SSH_KEY" SnipeIT-Professional-Inventory.ps1 admin@"$computer":/opt/scripts/
    
    # Execute with simulation mode (Linux limitation)
    ssh -i "$SSH_KEY" admin@"$computer" "pwsh -File /opt/scripts/SnipeIT-Professional-Inventory.ps1 -SimulateHardware -CustomerName '$COMPANY_NAME'"
    
    if [ $? -eq 0 ]; then
        echo "✅ $computer completed (Simulation mode only)"
    else
        echo "❌ $computer failed"
    fi
done
```

### 3. Cross-Platform Health Check Script
```powershell
# Check-Inventory-Health-CrossPlatform.ps1
param(
    [string[]]$WindowsComputers,
    [string[]]$LinuxServers
)

Write-Host "=== Cross-Platform Inventory Health Check ===" -ForegroundColor Cyan

# Check Windows computers (full inventory expected)
foreach ($Computer in $WindowsComputers) {
    if (Test-Connection -ComputerName $Computer -Count 1 -Quiet) {
        $LogFile = "\\$Computer\C$\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"
        
        if (Test-Path $LogFile) {
            $LastRun = (Get-Item $LogFile).LastWriteTime
            $DaysOld = (Get-Date) - $LastRun
            
            if ($DaysOld.Days -gt 7) {
                Write-Host "$Computer (Windows) - Inventory outdated ($($DaysOld.Days) days)" -ForegroundColor Yellow
            } else {
                Write-Host "$Computer (Windows) - Full inventory current" -ForegroundColor Green
            }
        } else {
            Write-Host "$Computer (Windows) - No inventory log found" -ForegroundColor Red
        }
    }
}

# Check Linux servers (simulation mode expected)
foreach ($Server in $LinuxServers) {
    if (Test-Connection -ComputerName $Server -Count 1 -Quiet) {
        try {
            $LogCheck = Invoke-Command -ComputerName $Server -ScriptBlock {
                $LogFile = "/opt/scripts/logs/snipeit-inventory.log"
                if (Test-Path $LogFile) {
                    $LastRun = (Get-Item $LogFile).LastWriteTime
                    return $LastRun
                }
                return $null
            }
            
            if ($LogCheck) {
                $DaysOld = (Get-Date) - $LogCheck
                if ($DaysOld.Days -gt 7) {
                    Write-Host "$Server (Linux) - Simulation outdated ($($DaysOld.Days) days)" -ForegroundColor Yellow
                } else {
                    Write-Host "$Server (Linux) - Simulation current (No hardware detection)" -ForegroundColor Cyan
                }
            } else {
                Write-Host "$Server (Linux) - No simulation log found" -ForegroundColor Red
            }
        }
        catch {
            Write-Host "$Server (Linux) - Unable to check status: $_" -ForegroundColor Red
        }
    }
}
```

## Monitoring and Reporting

### 1. Platform-Aware Log Analysis Script
```powershell
# Analyze-Inventory-Logs-CrossPlatform.ps1
param(
    [string]$Platform = "Windows"
)

$LogPath = switch ($Platform) {
    "Windows" { "C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log" }
    "Linux" { "/var/log/snipeit-inventory.log" }
    "macOS" { "/usr/local/var/log/snipeit-inventory.log" }
}

if (Test-Path $LogPath) {
    $Logs = Get-Content $LogPath
    
    $Errors = $Logs | Where-Object { $_ -like "*[ERROR]*" }
    $Warnings = $Logs | Where-Object { $_ -like "*[WARN]*" }
    $Success = $Logs | Where-Object { $_ -like "*[SUCCESS]*" }
    $SimulationMode = $Logs | Where-Object { $_ -like "*SimulateHardware*" }
    
    Write-Host "=== $Platform Inventory Log Analysis ===" -ForegroundColor Cyan
    Write-Host "Success Events: $($Success.Count)" -ForegroundColor Green
    Write-Host "Warnings: $($Warnings.Count)" -ForegroundColor Yellow
    Write-Host "Errors: $($Errors.Count)" -ForegroundColor Red
    
    if ($SimulationMode.Count -gt 0) {
        Write-Host "Simulation Mode: $($SimulationMode.Count) entries" -ForegroundColor Cyan
        Write-Host "Note: $Platform using simulation - no real hardware detection" -ForegroundColor Yellow
    }
    
    if ($Errors.Count -gt 0) {
        Write-Host "`nRecent Errors:" -ForegroundColor Red
        $Errors | Select-Object -Last 5 | ForEach-Object { Write-Host "  $_" }
    }
} else {
    Write-Host "Log file not found: $LogPath" -ForegroundColor Red
}
```

## Platform-Specific Troubleshooting

### Windows Issues and Solutions

#### 1. WMI Service Issues
```powershell
# Fix WMI issues
Stop-Service -Name "Winmgmt" -Force
Start-Service -Name "Winmgmt"

# Rebuild WMI repository if needed
winmgmt /resetrepository
```

#### 2. Execution Policy Restrictions
```powershell
# Solution: Set execution policy in deployment script
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

### Linux/macOS Issues and Solutions

#### 1. PowerShell Core Not Found
```bash
# Ubuntu/Debian installation
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y powershell

# macOS installation
brew install --cask powershell
```

#### 2. Hardware Detection Failures (Expected)
```bash
# This is expected on Linux/macOS - use simulation mode
pwsh -File SnipeIT-Professional-Inventory.ps1 -SimulateHardware -TestMode
```

#### 3. Permission Issues
```bash
# Set proper permissions
chmod +x SnipeIT-Professional-Inventory.ps1
chmod 750 /opt/scripts/snipeit-inventory.sh
```

### Cross-Platform Network Connectivity
```powershell
# Test connectivity before deployment
if (Test-NetConnection snipeit.company.com -Port 443 -WarningAction SilentlyContinue) {
    Write-Host "✅ SnipeIT server reachable" -ForegroundColor Green
} else {
    Write-Error "❌ Cannot connect to SnipeIT server"
    exit 1
}
```

---

## Best Practices by Platform

### Windows Best Practices
1. **Always test in non-production first**
2. **Use Group Policy or SCCM for large deployments**
3. **Run with Administrator privileges for full hardware detection**
4. **Monitor Windows Event Logs**
5. **Schedule during maintenance windows**

### Linux/macOS Best Practices
1. **Accept simulation mode limitations**
2. **Use containerization for consistency**
3. **Monitor system logs for PowerShell Core issues**
4. **Consider hybrid Windows/Linux deployments**
5. **Plan for Windows VMs for actual hardware detection**

### Universal Best Practices
1. **Use configuration management for API tokens**
2. **Monitor log files for issues**
3. **Implement health checks and alerts**
4. **Keep deployment scripts version-controlled**
5. **Document platform-specific limitations**

## Platform Feature Summary

| Feature | Windows | Linux | macOS |
|---------|---------|-------|-------|
| **Full Hardware Detection** | ✅ Yes | ❌ No | ❌ No |
| **API Integration** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Automated Deployment** | ✅ GP/SCCM | ⚠️ Manual/Cron | ⚠️ Manual |
| **Enterprise Ready** | ✅ Yes | ⚠️ Limited | ⚠️ Limited |
| **Simulation Mode** | ✅ Available | ✅ Required | ✅ Required |

For additional deployment assistance, please refer to the [Installation Guide](INSTALLATION.md) or open an issue on GitHub.