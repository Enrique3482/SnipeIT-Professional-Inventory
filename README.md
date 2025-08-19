# SnipeIT Professional Inventory System 🖥️

[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/Enrique3482/SnipeIT-Professional-Inventory)
[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue.svg)](https://docs.microsoft.com/en-us/powershell/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## 📋 Overview

Professional-grade PowerShell inventory system for **SnipeIT** asset management with comprehensive hardware detection, intelligent status management, and automated maintenance tracking.

> **🚀 Quick Start**: [English Guide](QUICKSTART.md) | [Deutsche Anleitung](SCHNELLSTART.md)

### ✨ Key Features

- 🔍 **Automatic Hardware Detection** - Complete system inventory with detailed component recognition
- 🖥️ **Monitor Management** - External monitor detection and separate asset tracking
- 🔌 **Docking Station Recognition** - Automatic detection and component management
- 📊 **Custom Field Management** - Intelligent field mapping with collision detection
- 👤 **Smart Checkout System** - Automatic user-computer matching and asset assignment
- 🔄 **Real-time Synchronization** - Live asset data updates to SnipeIT
- 📈 **Maintenance Tracking** - Automated scheduling and status monitoring
- 🎨 **Enhanced Color Coding** - Improved console output with professional styling
- 🛡️ **Enterprise Security** - Comprehensive error handling and rollback system

## 🚀 Quick Start

### Prerequisites

- **PowerShell 5.1+** (Windows PowerShell or PowerShell Core)
- **Windows 10/11** or **Windows Server 2016+**
- **SnipeIT** instance with API access
- **Administrator privileges** for complete hardware detection

### 5-Minute Installation

1. **Download the script:**
   ```powershell
   # Clone the repository
   git clone https://github.com/Enrique3482/SnipeIT-Professional-Inventory.git
   cd SnipeIT-Professional-Inventory
   ```

2. **Get your SnipeIT API token and configure:**
   ```powershell
   # Edit the configuration file
   notepad SnipeConfig.json
   ```

3. **Update these settings:**
   ```json
   {
     "Snipe": {
       "Url": "https://your-snipeit-instance.com/api/v1",
       "Token": "your-api-token-here",
       "StandardCompanyName": "Your Company Name"
     }
   }
   ```

4. **Test first, then run:**
   ```powershell
   # Test mode (recommended first run)
   .\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
   
   # Production run
   .\SnipeIT-Professional-Inventory.ps1 -CustomerName "Your Company"
   ```

**📖 Need detailed instructions?** See [QUICKSTART.md](QUICKSTART.md) or [SCHNELLSTART.md](SCHNELLSTART.md)

## 📖 Usage Examples

### Basic Usage
```powershell
# Standard inventory scan
.\SnipeIT-Professional-Inventory.ps1 -CustomerName "Enterprise Corp"
```

### Advanced Options
```powershell
# Test mode with simulated hardware
.\SnipeIT-Professional-Inventory.ps1 -TestMode -SimulateHardware -VerboseLogging

# Custom configuration file
.\SnipeIT-Professional-Inventory.ps1 -ConfigurationFile "C:\Config\CustomConfig.json"

# Specify log location
.\SnipeIT-Professional-Inventory.ps1 -LogPath "C:\Logs\inventory.log"
```

### Automated Deployment
```powershell
# Deploy via Group Policy or SCCM
powershell.exe -ExecutionPolicy Bypass -File "SnipeIT-Professional-Inventory.ps1" -CustomerName "Enterprise"
```

## 🔧 Configuration

### Essential Settings

| Parameter | Description | Example |
|-----------|-------------|---------|
| `Url` | SnipeIT API endpoint | `https://assets.company.com/api/v1` |
| `Token` | API authentication token | `Bearer eyJ0eXAiOiJKV1Q...` |
| `StandardCompanyName` | Default company for assets | `"Enterprise Organization"` |
| `StandardModelFieldsetId` | Custom fieldset ID | `2` |

### Field Mapping

The system automatically maps hardware data to SnipeIT custom fields:

```json
{
  "_snipeit_mac_address_1": "MacAddress",
  "_snipeit_cpu_4": "Processor", 
  "_snipeit_ram_gb_5": "Memory",
  "_snipeit_os_version_2": "OperatingSystem",
  "_snipeit_uuid_9": "UUID"
}
```

### Advanced Features

- **Automatic Checkout**: Assets are automatically checked out to users based on computer naming conventions
- **Monitor Detection**: External monitors are created as separate trackable assets
- **Docking Stations**: Detected as components and associated with primary computer
- **Maintenance Scheduling**: Automatic calculation of next maintenance dates

## 📊 System Requirements

### Minimum Requirements
- PowerShell 5.1
- Windows 10 (Build 1809+)
- 2GB RAM
- 100MB disk space
- Network connectivity to SnipeIT

### Recommended Specifications
- PowerShell 7.x
- Windows 11 or Server 2022
- 4GB RAM
- Administrator privileges
- Domain-joined computer

## 🛠️ Troubleshooting

### Common Issues

1. **API Authentication Errors**
   ```powershell
   # Verify your token has sufficient permissions
   Test-Connection your-snipeit-url.com
   ```

2. **Hardware Detection Issues**
   ```powershell
   # Run with elevated privileges
   Start-Process PowerShell -Verb RunAs
   ```

3. **Custom Field Conflicts**
   ```powershell
   # Reset field mappings
   .\SnipeIT-Professional-Inventory.ps1 -TestMode -VerboseLogging
   ```

### Log Locations

- **Main Log**: `C:\ProgramData\SnipeIT\Inventory\SnipeIT-Inventory.log`
- **Error Log**: `C:\ProgramData\SnipeIT\Errorlog\SnipeIT-Errors.log`
- **Backup Directory**: `C:\ProgramData\SnipeIT\Backups\`

## 📚 Documentation

- 📖 **Installation Guide**: [INSTALLATION.md](INSTALLATION.md)
- 🚀 **Quick Start (English)**: [QUICKSTART.md](QUICKSTART.md)
- 🚀 **Schnellstart (Deutsch)**: [SCHNELLSTART.md](SCHNELLSTART.md)
- 🚢 **Deployment Guide**: [DEPLOYMENT.md](DEPLOYMENT.md)
- 📝 **Changelog**: [CHANGELOG.md](CHANGELOG.md)

## 🔄 Version History

### v2.0.0 (Latest)
- ✅ Enhanced color-coded console output
- ✅ Improved RAM detection with fallback methods
- ✅ Eliminated duplicate storage information
- ✅ Advanced external monitor detection
- ✅ Comprehensive docking station recognition
- ✅ Automatic user-computer matching
- ✅ Enterprise-grade error handling

### v1.x
- Basic hardware detection
- Simple SnipeIT integration
- Manual configuration

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Quick Start**: [QUICKSTART.md](QUICKSTART.md) | [SCHNELLSTART.md](SCHNELLSTART.md)
- **Documentation**: [Installation Guide](INSTALLATION.md) | [Deployment Guide](DEPLOYMENT.md)
- **Issues**: [GitHub Issues](../../issues)
- **Discussions**: [GitHub Discussions](../../discussions)

## 🏢 Enterprise Support

For enterprise deployments, custom integrations, or professional support:

- **Email**: support@your-domain.com
- **Documentation**: Full deployment guides available
- **Training**: PowerShell and SnipeIT integration workshops

---

**Made with ❤️ for professional IT asset management**

![Professional IT](https://img.shields.io/badge/Professional-IT%20Asset%20Management-blue?style=for-the-badge)