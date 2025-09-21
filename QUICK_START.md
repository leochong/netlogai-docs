# NetLogAI Quick Start Guide

Welcome to NetLogAI - the AI-powered network log analysis tool that transforms your network troubleshooting experience! This guide will get you up and running in minutes.

## 🚀 Installation

### Windows Installation (Recommended)

1. **Download the MSI Installer**
   ```
   Download: NetLogAI-v1.0.0-x64.msi
   ```

2. **Run Dependency Check (Optional)**
   ```cmd
   dependency-check.bat
   ```

3. **Install NetLogAI**
   ```cmd
   # Interactive installation
   NetLogAI-v1.0.0-x64.msi

   # Silent installation
   msiexec /i NetLogAI-v1.0.0-x64.msi /quiet
   ```

4. **Verify Installation**
   ```cmd
   netlogai --version
   verify-installation.bat
   ```

### Alternative Installation (Build from Source)

```cmd
# Clone repository
git clone https://github.com/netlogai/netlogai-core.git
cd netlogai-core

# Build with vcpkg dependencies
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build build --config Release

# Run NetLogAI
./build/Release/netlogai.exe --version
```

## ⚡ First Steps

### 1. Initialize Configuration

```cmd
# Initialize NetLogAI configuration
netlogai config init

# Check status
netlogai status
```

### 2. Configure AI Integration (Optional)

NetLogAI supports multiple AI providers for advanced analysis:

```cmd
# Configure AI provider (interactive wizard)
netlogai config ai

# Or set manually
netlogai config set ai_provider anthropic
netlogai config set anthropic_api_key your-api-key-here
```

### 3. Add Your First Device

```cmd
# Add a network device
netlogai device add Router1 --ip 192.168.1.1 --type cisco-ios --username admin

# List configured devices
netlogai device list

# Test connectivity
netlogai device show Router1
```

## 📊 Basic Usage

### Fetch and Analyze Logs

```cmd
# Fetch logs from a device
netlogai fetch Router1 --output router1_logs.txt

# Analyze logs for patterns
netlogai analyze router1_logs.txt --pattern "interface_down"

# Interactive timeline view
netlogai timeline router1_logs.txt --interactive
```

### AI-Powered Analysis

```cmd
# Ask AI about specific issues
netlogai ask "Why is BGP flapping on Router1?"

# Analyze error messages
netlogai ask error "%LINEPROTO-5-UPDOWN: gi0/1 down" --device-type cisco-ios

# Get troubleshooting suggestions
netlogai ask fix "High CPU usage on switches"
```

### Git-Style Log Management

```cmd
# View condensed log timeline
netlogai log --online

# Show device-specific logs with graph
netlogai log --graph --device Router1

# Search for specific events
netlogai log --grep "BGP"

# Compare logs between timeframes
netlogai diff --since "1 hour ago"
```

## 🔧 Configuration

### Basic Configuration File

NetLogAI uses JSON configuration files located in:
- Windows: `%PROGRAMFILES%\NetLogAI\config\`
- Source build: `./config/`

Example `config/netlogai.json`:
```json
{
  "version": "1.0.0",
  "ai_provider": "anthropic",
  "log_level": "info",
  "device_timeout": 30,
  "max_log_entries": 10000,
  "plugins": {
    "security": true,
    "performance": true,
    "topology": true
  }
}
```

### Device Profiles

Example `config/devices.json`:
```json
{
  "devices": [
    {
      "name": "Router1",
      "ip": "192.168.1.1",
      "type": "cisco-ios",
      "username": "admin",
      "ssh_port": 22,
      "timeout": 30
    },
    {
      "name": "Switch1",
      "ip": "192.168.1.10",
      "type": "cisco-nxos",
      "username": "admin",
      "ssh_port": 22
    }
  ]
}
```

## 🎯 Common Use Cases

### 1. Interface Troubleshooting

```cmd
# Find interface issues
netlogai analyze logs.txt --pattern "interface"

# Get AI analysis of interface problems
netlogai ask "Interface gi0/1 keeps going down, what could be the cause?"

# Timeline view of interface events
netlogai timeline logs.txt --filter "interface" --interactive
```

### 2. BGP Analysis

```cmd
# Search for BGP events
netlogai log --grep "BGP"

# AI-powered BGP troubleshooting
netlogai ask "BGP neighbor 10.1.1.1 is flapping, help me troubleshoot"

# Correlate BGP events across devices
netlogai correlate --pattern "BGP" --timespan 1h
```

### 3. Security Monitoring

```cmd
# Run security analysis
netlogai security scan logs.txt

# Check for authentication failures
netlogai analyze logs.txt --pattern "auth_fail"

# AI security assessment
netlogai ask "Are there any security concerns in these logs?" --file logs.txt
```

### 4. Performance Monitoring

```cmd
# Establish performance baseline
netlogai perf baseline --device Router1

# Compare current performance
netlogai perf compare --period 7d

# Get performance insights
netlogai ask "Why is network performance degraded?"
```

## 🔌 Plugin System

### Available Plugins

- **Security Plugin**: Advanced threat detection and security analysis
- **Performance Plugin**: Network performance monitoring and baseline analysis
- **Topology Plugin**: Network topology discovery and visualization

### Plugin Management

```cmd
# List available plugins
netlogai plugin list

# Install a plugin
netlogai plugin install security_plugin.dll

# Test plugin functionality
netlogai plugin test security_plugin.dll

# Plugin status
netlogai plugin status
```

## 🆘 Troubleshooting

### Common Issues

1. **AI Features Not Working**
   ```cmd
   # Check AI configuration
   netlogai ai-status

   # Test AI connectivity
   netlogai ai-test
   ```

2. **Device Connection Failures**
   ```cmd
   # Test device connectivity
   netlogai device show DeviceName

   # Check SSH connectivity manually
   ssh admin@192.168.1.1
   ```

3. **Plugin Loading Issues**
   ```cmd
   # Validate plugin
   netlogai plugin validate plugin.dll

   # Check plugin dependencies
   netlogai plugin test plugin.dll
   ```

### Getting Help

```cmd
# General help
netlogai --help

# Command-specific help
netlogai device --help
netlogai ask --help

# Show configuration
netlogai config show

# System status
netlogai status
```

## 📚 Next Steps

- **Advanced Configuration**: See [Configuration Guide](CONFIGURATION.md)
- **Plugin Development**: See [Plugin Development Guide](PLUGIN_DEVELOPMENT.md)
- **AI Integration**: See [AI Integration Guide](AI_INTEGRATION.md)
- **Troubleshooting**: See [Troubleshooting Guide](TROUBLESHOOTING.md)
- **API Reference**: See [API Documentation](API_REFERENCE.md)

## 🤝 Support

- **Documentation**: https://docs.netlogai.com
- **GitHub Issues**: https://github.com/netlogai/netlogai-core/issues
- **Community Forum**: https://community.netlogai.com
- **Email Support**: support@netlogai.com

---

**Welcome to the future of network troubleshooting! 🎉**

NetLogAI combines the power of traditional network analysis with cutting-edge AI to help you solve problems faster than ever before.