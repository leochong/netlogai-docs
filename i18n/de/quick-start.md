# NetLogAI Schnellstartanleitung

Diese Anleitung hilft Ihnen dabei, schnell mit NetLogAI zu beginnen und seine Kernfunktionen zu verstehen.

## 📋 Voraussetzungen

Stellen Sie vor dem Start sicher, dass Sie Folgendes installiert haben:

- **Windows 10/11** (64-Bit)
- **Visual Studio 2022** oder **Visual Studio Code mit CMake Tools**
- **Git für Windows**
- **Administratorrechte** (für einige Installationsschritte erforderlich)

## 🚀 Installationsschritte

### Schritt 1: Repository klonen

```bash
# NetLogAI-Repository klonen
git clone https://github.com/leochong/netlogai-core.git
cd netlogai-core

# Submodule initialisieren (enthält Open-Source-Komponenten)
git submodule update --init --recursive
```

### Schritt 2: Build-Umgebung einrichten

```bash
# vcpkg-Abhängigkeiten installieren
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# CMake-Build konfigurieren
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### Schritt 3: Projekt erstellen

```bash
# Im Release-Modus erstellen
cmake --build build --config Release

# Erfolgreichen Build überprüfen
./build/Release/netlogai.exe --version
```

## 🎯 Erste Nutzung

### Grundlegende Befehle testen

```bash
# NetLogAI-Status überprüfen
./build/Release/netlogai.exe status

# Verfügbare Parser auflisten
./build/Release/netlogai.exe parser list

# AI-Status überprüfen
./build/Release/netlogai.exe ai-status
```

### Interaktive Shell starten

```bash
# NetLogAI im interaktiven Modus ausführen
./build/Release/netlogai.exe

# Hilfe in der Shell anzeigen
help

# Verfügbare Befehle erkunden
browse
```

## 🤖 AI-Einrichtung (Optional)

Um AI-gestützte Analyse zu verwenden:

```bash
# Interaktiver AI-Setup-Assistent
./build/Release/netlogai.exe config ai

# Oder Umgebungsvariablen setzen
export NETLOGAI_AI_PROVIDER=anthropic
export NETLOGAI_ANTHROPIC_KEY=your-api-key
```

## 📊 Beispiel-Anwendungsfälle

### 1. Log-Datei analysieren

```bash
# Beispiel-Log-Datei analysieren
./build/Release/netlogai.exe analyze sample-logs/cisco-router.log

# Nach spezifischen Mustern suchen
./build/Release/netlogai.exe analyze --pattern "BGP" sample-logs/
```

### 2. AI-gestützte Analyse

```bash
# Natürlichsprachige Fragen stellen
./build/Release/netlogai.exe ask "Gibt es Probleme im Netzwerk?"

# Spezifische Fehler erklären
./build/Release/netlogai.exe ask error "%LINEPROTO-5-UPDOWN" --device-type cisco-ios
```

### 3. Geräteverwaltung

```bash
# Netzwerkgerät hinzufügen
./build/Release/netlogai.exe device add Router1 --ip 192.168.1.1 --type cisco-ios

# Geräteliste überprüfen
./build/Release/netlogai.exe device list

# Geräteverbindung testen
./build/Release/netlogai.exe device show Router1
```

## 🎮 Interaktive Funktionen

### Log-Viewer-Steuerung

- **Navigation**: `j/k` (hoch/runter), `Space/b` (Seite runter/hoch)
- **Suche**: `/` (suchen), `n/N` (nächste/vorherige Übereinstimmung)
- **Funktionen**: `l` (Zeilennummern), `t` (Zeitstempel)
- **Hilfe**: `h` oder `?`

### Dateibrowser

- **Navigation**: Pfeiltasten, `Enter` (öffnen)
- **Auswahl**: `Space` (umschalten), `a` (alle auswählen)
- **Aktionen**: `r` (aktualisieren), `/` (suchen)

## ⚠️ Häufige Problemlösungen

### Build-Fehler

```bash
# vcpkg-Abhängigkeiten neu installieren
./vcpkg/vcpkg remove --outdated
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# Build-Cache löschen
rm -rf build/
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### Berechtigungsprobleme

```bash
# Eingabeaufforderung als Administrator ausführen
# Oder in Benutzerverzeichnis installieren
```

### AI-Verbindungsprobleme

```bash
# AI-Status überprüfen
./build/Release/netlogai.exe ai-status

# AI-Konfiguration testen
./build/Release/netlogai.exe ai-test
```

## 📚 Nächste Schritte

1. **[Benutzerhandbuch](../user-guide.md)** - Erweiterte Funktionen erlernen
2. **[Befehlsreferenz](../command-reference.md)** - Alle Befehle erkunden
3. **[Konfigurationshandbuch](../configuration.md)** - Erweiterte Einstellungen
4. **[Plugin-Entwicklung](../plugin-development.md)** - Benutzerdefinierte Funktionen erstellen

## 🆘 Benötigen Sie Hilfe?

- 📧 **E-Mail**: support@netlogai.com
- 🐛 **Issues**: [GitHub Issues](https://github.com/netlogai/netlogai-core/issues)
- 📖 **Dokumentation**: [Projekt-Wiki](https://github.com/netlogai/netlogai-core/wiki)

---

Glückwunsch! Sie sind jetzt bereit, NetLogAI zu verwenden. 🎉