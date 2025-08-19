# Deployment Examples 🚀

This document provides various deployment scenarios for the SnipeIT Professional Inventory System.

## Quick Start Deployment

### Single Computer Test
```powershell
# Test on current computer
.\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
```

### Production Single Run
```powershell
# Production run with custom company name
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Acme Corporation"
```

## Enterprise Deployment Scenarios

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
        # Execute inventory script
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

### 3. Scheduled Task Deployment

**Create via PowerShell:**
```powershell
# Create-InventoryTask.ps1
$TaskName = "SnipeIT Professional Inventory"
$ScriptPath = "C:\Scripts\SnipeIT-Inventory\SnipeIT-Professional-Inventory.ps1"
$CompanyName = "Your Company Name"

# Task action
$Action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -WindowStyle Hidden -File '$ScriptPath' -CustomerName '$CompanyName'"

# Task trigger (daily at 2 AM)
$Trigger = New-ScheduledTaskTrigger -Daily -At "02:00"

# Task settings
$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries -StartWhenAvailable

# Task principal (run as SYSTEM)
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

# Register the task
Register-ScheduledTask -TaskName $TaskName -Action $Action -Trigger $Trigger -Settings $Settings -Principal $Principal -Description "Automated SnipeIT inventory scan"

Write-Host "Scheduled task '$TaskName' created successfully" -ForegroundColor Green
```

### 4. Docker Container Deployment

**Dockerfile:**
```dockerfile
FROM mcr.microsoft.com/powershell:7.3-ubuntu-22.04

# Set working directory
WORKDIR /app

# Copy scripts
COPY . .

# Set permissions
RUN chmod +x SnipeIT-Professional-Inventory.ps1

# Default command
CMD ["pwsh", "-File", "SnipeIT-Professional-Inventory.ps1", "-TestMode"]
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
```

## Configuration Templates

### 1. Development Environment
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
  }
}
```

### 2. Production Environment
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
  }
}
```

### 3. Multi-Tenant Environment
```json
{
  "Snipe": {
    "Url": "https://tenant1.snipeit-cloud.com/api/v1",
    "Token": "tenant1-token",
    "StandardCompanyName": "Tenant 1 Corporation"
  },
  "CustomFieldMapping": {
    "_snipeit_tenant_id_1": "TenantId",
    "_snipeit_location_code_2": "LocationCode"
  }
}
```

## Automation Scripts

### 1. Bulk Deployment Script
```powershell
# Deploy-Multiple-Computers.ps1
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
            
            # Download and run script
            $ScriptUrl = "https://raw.githubusercontent.com/Enrique3482/SnipeIT-Professional-Inventory/main/SnipeIT-Professional-Inventory.ps1"
            $Script = Invoke-WebRequest -Uri $ScriptUrl -UseBasicParsing
            
            # Execute
            Invoke-Expression $Script.Content -CustomerName $Company
        } -ArgumentList $CompanyName
        
        Write-Host "✅ $Computer completed" -ForegroundColor Green
    }
    catch {
        Write-Host "❌ $Computer failed: $_" -ForegroundColor Red
    }
}
```

### 2. Health Check Script
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
                Write-Host "$Computer - Inventory outdated ($($DaysOld.Days) days)" -ForegroundColor Yellow
            } else {
                Write-Host "$Computer - Inventory current" -ForegroundColor Green
            }
        } else {
            Write-Host "$Computer - No inventory log found" -ForegroundColor Red
        }
    }
}
```

## Monitoring and Reporting

### 1. Log Analysis Script
```powershell
# Analyze-Inventory-Logs.ps1
$LogPath = "C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log"

if (Test-Path $LogPath) {
    $Logs = Get-Content $LogPath
    
    $Errors = $Logs | Where-Object { $_ -like "*[ERROR]*" }
    $Warnings = $Logs | Where-Object { $_ -like "*[WARN]*" }
    $Success = $Logs | Where-Object { $_ -like "*[SUCCESS]*" }
    
    Write-Host "=== Inventory Log Analysis ===" -ForegroundColor Cyan
    Write-Host "Success Events: $($Success.Count)" -ForegroundColor Green
    Write-Host "Warnings: $($Warnings.Count)" -ForegroundColor Yellow
    Write-Host "Errors: $($Errors.Count)" -ForegroundColor Red
    
    if ($Errors.Count -gt 0) {
        Write-Host "`nRecent Errors:" -ForegroundColor Red
        $Errors | Select-Object -Last 5 | ForEach-Object { Write-Host "  $_" }
    }
}
```

### 2. Performance Monitoring
```powershell
# Monitor-Inventory-Performance.ps1
$StartTime = Get-Date

# Run inventory
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Performance Test"

$EndTime = Get-Date
$Duration = $EndTime - $StartTime

Write-Host "Inventory Performance Report" -ForegroundColor Cyan
Write-Host "Start Time: $StartTime"
Write-Host "End Time: $EndTime"
Write-Host "Duration: $($Duration.TotalMinutes) minutes"

# Log to performance database
$PerfData = @{
    Computer = $env:COMPUTERNAME
    StartTime = $StartTime
    EndTime = $EndTime
    Duration = $Duration.TotalMinutes
    Date = (Get-Date).Date
}

# Export to CSV for tracking
$PerfData | Export-Csv -Path "C:\Logs\InventoryPerformance.csv" -Append -NoTypeInformation
```

## Troubleshooting Deployments

### Common Issues and Solutions

#### 1. Execution Policy Restrictions
```powershell
# Solution: Set execution policy in deployment script
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

#### 2. Network Connectivity Issues
```powershell
# Test connectivity before deployment
if (Test-NetConnection snipeit.company.com -Port 443 -WarningAction SilentlyContinue) {
    # Proceed with inventory
} else {
    Write-Error "Cannot connect to SnipeIT server"
    exit 1
}
```

#### 3. Permission Issues
```powershell
# Check if running as administrator
if (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Warning "Script requires Administrator privileges for full hardware detection"
    # Continue with limited detection or exit
}
```

---

## Best Practices

1. **Always test in non-production first**
2. **Use configuration management for API tokens**
3. **Monitor log files for issues**
4. **Schedule regular inventory updates**
5. **Implement health checks and alerts**
6. **Keep deployment scripts version-controlled**

For additional deployment assistance, please refer to the [Installation Guide](INSTALLATION.md) or open an issue on GitHub.