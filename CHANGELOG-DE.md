# Änderungsprotokoll

Alle wichtigen Änderungen am SnipeIT Professional Inventory System werden in dieser Datei dokumentiert.

Das Format basiert auf [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
und dieses Projekt folgt [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2025-08-19

### 🎉 Hauptversion - Professional Edition

### Hinzugefügt
- ✅ **Erweiterte farbkodierte Konsolenausgabe** - Professionelles Styling mit verbesserter Lesbarkeit
- ✅ **Erweiterte RAM-Erkennung** - Mehrere Fallback-Methoden für zuverlässige Speichererkennung
- ✅ **Externe Monitor-Erkennung** - Umfassende Erkennung externer Displays als separate Assets
- ✅ **Docking-Station-Management** - Automatische Erkennung und Komponentenverfolgung
- ✅ **Intelligente Benutzer-Computer-Zuordnung** - Automatisches Asset-Checkout basierend auf Namenskonventionen
- ✅ **Enterprise-Fehlerbehandlung** - Umfassendes Rollback-System und Backup-Management
- ✅ **Custom Field Manager** - Intelligentes Feld-Mapping mit Kollisionserkennung
- ✅ **Wartungsverfolgung** - Automatisierte Planung und Statusüberwachung
- ✅ **Echtzeit-API-Synchronisation** - Erweiterte SnipeIT-Integration
- ✅ **Professionelles Protokollierungssystem** - Detaillierte Logs mit farbkodierter Ausgabe

### Verbessert
- 🔧 **Hardware-Erkennungs-Engine** - Zuverlässigere Komponentenidentifikation
- 🔧 **Konfigurationsverwaltung** - JSON-basierte Einstellungen mit Validierung
- 🔧 **Asset Manager** - Intelligente Kategorisierung und Status-Zuweisung
- 🔧 **API Client** - Robuste Fehlerbehandlung und Wiederholungsmechanismen

### Behoben
- 🐛 **Doppelte Speicherinformationen** - Redundante Speicherdaten eliminiert
- 🐛 **RAM-Erkennungsprobleme** - Verbesserte Kompatibilität verschiedener Systeme
- 🐛 **Monitor-Erkennungsgenauigkeit** - Bessere Erkennung externer vs. interner Displays
- 🐛 **Feld-Mapping-Konflikte** - Automatische Auflösung von ID-Kollisionen

### Sicherheit
- 🔒 **Token-Schutz** - Sicherer Umgang mit API-Anmeldedaten
- 🔒 **Eingabevalidierung** - Erweiterte Parameterprüfung
- 🔒 **Fehlerbereinigung** - Saubere Fehlermeldungen ohne sensible Daten

## [1.x] - Vorherige Versionen

### Features
- Grundlegende Hardware-Erkennung
- Einfache SnipeIT-Integration
- Manuelle Konfiguration
- Grundlegende Protokollierung

---

## Upgrade-Hinweise

### Von v1.x zu v2.0.0
1. **Konfiguration sichern** - Bestehende Einstellungen speichern
2. **Konfigurationsformat aktualisieren** - Neue JSON-Struktur verwenden
3. **Mit -TestMode testen** - Funktionalität vor Produktion überprüfen
4. **Custom Fields überprüfen** - Feld-Mappings in SnipeIT prüfen

### Breaking Changes
- Konfigurationsdatei-Format von einfach zu JSON geändert
- Neue Custom Field-Struktur
- Erweiterte API-Anforderungen

### Empfohlene Aktionen
- Ersten Scan mit `-TestMode -VerboseLogging` ausführen
- Externe Monitor-Erkennung überprüfen
- Docking-Station-Erkennung validieren
- Benutzer-Computer-Zuordnung testen

---

## Support & Dokumentation

- **Issues**: [GitHub Issues](https://github.com/Enrique3482/SnipeIT-Professional-Inventory/issues)
- **Dokumentation**: [Wiki](https://github.com/Enrique3482/SnipeIT-Professional-Inventory/wiki)
- **Diskussionen**: [GitHub Discussions](https://github.com/Enrique3482/SnipeIT-Professional-Inventory/discussions)