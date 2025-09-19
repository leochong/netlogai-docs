# NetLogAI Architecture Overview

NetLogAI implements a modern, modular architecture designed for scalability, maintainability, and extensibility. The system is built around a multi-repository "open core" model that balances commercial viability with community collaboration.

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                          NetLogAI Core                          │
│                     (Commercial License)                       │
├─────────────────────────────────────────────────────────────────┤
│  CLI Interface  │  AI Integration  │  Advanced Analytics      │
│  Git-style CMD  │  Claude/OpenAI   │  Pattern Recognition     │
├─────────────────────────────────────────────────────────────────┤
│                      Plugin System                              │
│                      (MIT License)                              │
├─────────────────────────────────────────────────────────────────┤
│                       libnetlog                                 │
│                Core Parsing Library (MIT License)               │
└─────────────────────────────────────────────────────────────────┘
```

## Repository Structure

### 1. netlogai-core (Private Repository)
**License:** Dual License (Non-commercial Free / Commercial Paid)
**Purpose:** Commercial core with advanced features

```
netlogai-core/
├── src/
│   ├── commands/           # CLI command implementations
│   ├── ai/                 # AI integration layer
│   ├── analytics/          # Advanced analysis features
│   └── networking/         # Device communication
├── include/
│   ├── plugins/            # Plugin interface definitions
│   └── analytics/          # Analysis frameworks
└── third-party/            # Submodules to public repos
    ├── libnetlog/          # Core parsing library
    ├── plugin-sdk/         # Plugin development kit
    ├── parsers/            # Community parsers
    └── docs/               # Documentation
```

### 2. libnetlog (Public Repository)
**License:** MIT
**Purpose:** Core log parsing library

```
libnetlog/
├── include/libnetlog/
│   ├── parsers/            # Device-specific parsers
│   ├── log_entry.hpp       # Log entry structures
│   └── parser_factory.hpp  # Parser instantiation
├── src/
│   ├── parsers/            # Parser implementations
│   ├── lua_engine.cpp      # Lua scripting support
│   └── utils/              # Utility functions
└── CMakeLists.txt          # Build configuration
```

### 3. netlogai-plugin-sdk (Public Repository)
**License:** MIT
**Purpose:** Plugin development framework

```
netlogai-plugin-sdk/
├── plugin_interface.hpp    # Core plugin interface
├── plugin_manager.hpp      # Plugin lifecycle management
└── examples/
    ├── security/           # Security analysis plugin
    ├── performance/        # Performance monitoring plugin
    └── topology/           # Network topology plugin
```

### 4. netlogai-parsers (Public Repository)
**License:** MIT
**Purpose:** Community-contributed parsers

```
netlogai-parsers/
└── examples/
    ├── cisco-ios-basic.nlp     # Cisco IOS parser
    ├── palo-alto-firewall.nlp  # Palo Alto parser
    └── README.md               # Parser development guide
```

### 5. netlogai-docs (Public Repository)
**License:** MIT
**Purpose:** Comprehensive documentation

```
netlogai-docs/
├── architecture-overview.md    # System architecture
├── netlog-parser-dsl.md       # Parser DSL reference
├── api-reference.md           # API documentation
└── user-guide.md              # End-user documentation
```

## Core Components

### 1. Command Line Interface (CLI)

The CLI implements a git-style command structure for intuitive operation:

```bash
# Log management
netlogai log --online                    # Condensed log view
netlogai log --graph --device R1         # Device timeline
netlogai show HEAD~5                     # Historical view

# AI-powered analysis
netlogai ask "Why is BGP flapping?"      # Natural language queries
netlogai analyze --pattern "interface down" # Pattern analysis

# Device management
netlogai device add 192.168.1.1          # Add network device
netlogai fetch --device R1               # Fetch logs

# Parser management
netlogai parser install custom.nlp       # Install custom parser
netlogai parser list                     # List available parsers
```

### 2. AI Integration Layer

The AI system provides an abstracted interface supporting multiple providers:

```cpp
namespace NetLogAI::AI {
    enum class AIProvider { Anthropic, OpenAI, None };

    class AIConfigurationManager {
        void configure_provider(AIProvider provider, const std::string& api_key);
        std::string query(const std::string& question, const LogContext& context);
        bool is_configured() const;
    };
}
```

**Supported Providers:**
- **Anthropic Claude** - Primary recommendation for network analysis
- **OpenAI GPT** - Alternative provider support
- **Extensible** - Plugin architecture for additional providers

### 3. Log Processing Pipeline

```
Raw Logs → Device Parsers → libnetlog → Structured Data → Analysis Engine → Results
    ↓            ↓             ↓            ↓              ↓
  SSH/API    Lua Scripts   C++ Library   JSON/DB      AI/Analytics
```

**Processing Flow:**
1. **Log Collection** - SSH, API, or file-based log retrieval
2. **Device Detection** - Automatic device type identification
3. **Parser Selection** - Route to appropriate device parser
4. **Parsing** - Extract structured data using Lua DSL
5. **Storage** - Store in queryable format
6. **Analysis** - Apply analytics and AI processing

### 4. Plugin Architecture

```cpp
// Plugin Interface
class INetLogPlugin {
public:
    virtual std::string getName() const = 0;
    virtual std::string getVersion() const = 0;
    virtual bool initialize(const PluginConfig& config) = 0;
    virtual PluginResult process(const LogEntry& entry) = 0;
    virtual void shutdown() = 0;
};

// Plugin Manager
class PluginManager {
    void loadPlugin(const std::string& plugin_path);
    void unloadPlugin(const std::string& plugin_name);
    std::vector<PluginInfo> listPlugins() const;
    bool executePlugin(const std::string& plugin_name, const LogEntry& entry);
};
```

**Plugin Types:**
- **Security Plugins** - Threat detection, anomaly analysis
- **Performance Plugins** - Resource monitoring, baseline comparison
- **Topology Plugins** - Network mapping, relationship analysis
- **Custom Plugins** - User-defined analysis logic

## Data Flow Architecture

### 1. Log Ingestion

```
Network Devices → Collection Layer → Parsing Engine → Storage Layer
     ↓                   ↓               ↓              ↓
   SSH/SNMP         Device Drivers   Lua Parsers    Database/Files
```

### 2. Analysis Pipeline

```
Stored Logs → Query Engine → Analysis Modules → AI Processing → Results
     ↓             ↓             ↓               ↓            ↓
   Database    SQL/NoSQL     Pattern/Stats    Claude API   JSON/Reports
```

### 3. Plugin Processing

```
Log Entry → Plugin Manager → Security Plugin → Results
    ↓            ↓              ↓               ↓
  Structured   Load/Execute   Threat Detect   Alerts/Actions
```

## Technology Stack

### Core Technologies
- **Language:** C++20 with MSVC 2022
- **Build System:** CMake with vcpkg package management
- **Scripting:** Lua 5.4 for parser DSL
- **JSON Processing:** nlohmann/json library
- **Networking:** Native Windows sockets + WinAPI

### Development Environment
- **Primary IDE:** Visual Studio Code with CMake Tools
- **Compiler:** Microsoft Visual C++ 2022
- **Platform:** Windows-first with cross-platform potential
- **Version Control:** Git with submodule architecture

### Dependencies
```cmake
# Core dependencies (managed via vcpkg)
find_package(nlohmann_json CONFIG REQUIRED)
find_package(Lua REQUIRED)

# Optional dependencies
find_package(OpenSSL REQUIRED)     # For secure connections
find_package(CURL REQUIRED)        # For HTTP API calls
```

## Security Architecture

### 1. Input Validation
- **Log Sanitization** - All log input validated and sanitized
- **Parser Sandboxing** - Lua parsers run in controlled environment
- **API Key Protection** - Encrypted storage of AI provider keys

### 2. Access Control
- **Role-based Access** - Different permission levels for users
- **Plugin Security** - Plugins run with restricted permissions
- **Network Security** - Secure device communication protocols

### 3. Data Protection
- **Encryption at Rest** - Sensitive data encrypted in storage
- **Secure Transmission** - TLS for all network communications
- **Key Management** - Proper handling of API keys and credentials

## Performance Considerations

### 1. Parsing Optimization
- **Compiled Patterns** - Regex patterns pre-compiled for speed
- **Memory Management** - Efficient memory allocation strategies
- **Parallel Processing** - Multi-threaded log processing where applicable

### 2. Storage Efficiency
- **Indexed Storage** - Optimized indexing for fast queries
- **Compression** - Log data compression for space efficiency
- **Caching** - Intelligent caching of frequently accessed data

### 3. Scalability
- **Modular Design** - Components can be scaled independently
- **Plugin Architecture** - Extensible without core modifications
- **Resource Management** - Configurable resource limits

## Deployment Architecture

### 1. Single Node Deployment
```
┌─────────────────────────────────────────┐
│              Local Machine              │
├─────────────────────────────────────────┤
│  NetLogAI Core  │  Database  │  Plugins │
│     CLI         │   SQLite   │   DLLs   │
└─────────────────────────────────────────┘
```

### 2. Enterprise Deployment
```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│   Collection    │  │   Processing    │  │    Analysis     │
│     Nodes       │  │      Node       │  │      Node       │
│   (NetLogAI)    │  │  (NetLogAI Core)│  │   (AI/Analytics)│
└─────────────────┘  └─────────────────┘  └─────────────────┘
         │                      │                      │
         └──────────────────────┼──────────────────────┘
                                │
                    ┌─────────────────┐
                    │   Shared        │
                    │   Database      │
                    └─────────────────┘
```

## Integration Points

### 1. External Systems
- **SIEM Integration** - Export to security information systems
- **Monitoring Tools** - Integration with existing monitoring
- **Ticketing Systems** - Automatic ticket creation for issues
- **Reporting Systems** - Export data for business intelligence

### 2. APIs and Interfaces
- **REST API** - Web service interface for integration
- **CLI Interface** - Command-line automation
- **Plugin API** - Third-party plugin development
- **Export Formats** - JSON, CSV, XML data export

## Future Architecture Considerations

### 1. Cloud Integration
- **Cloud Parsers** - Cloud-hosted parser execution
- **Distributed Storage** - Cloud storage for large deployments
- **Auto-scaling** - Dynamic resource allocation

### 2. Machine Learning
- **Pattern Learning** - Automatic pattern discovery
- **Anomaly Detection** - ML-based anomaly identification
- **Predictive Analytics** - Forecasting network issues

### 3. Real-time Processing
- **Stream Processing** - Real-time log analysis
- **Event Correlation** - Real-time event correlation
- **Alerting** - Immediate notification systems

This architecture provides a solid foundation for both current functionality and future enhancements while maintaining the flexibility needed for diverse deployment scenarios.