# NetLog Parser DSL Reference

The NetLog Parser DSL (Domain-Specific Language) is a Lua-based scripting language designed specifically for parsing network device logs. It provides a standardized framework for creating efficient, maintainable parsers for various network devices.

## Overview

The DSL abstracts common log parsing patterns while allowing device-specific customizations. It's designed to be:

- **Easy to learn** - Simple Lua syntax with network-focused functions
- **Performant** - Optimized for high-throughput log processing
- **Extensible** - Support for custom parsing logic and patterns
- **Maintainable** - Clear structure and validation frameworks

## Parser Structure

Every parser must follow this basic structure:

```lua
-- Parser metadata (required)
parser_info = {
    name = "parser-name",
    version = "1.0.0",
    description = "Parser description",
    vendor = "vendor-name",
    device_type = "device-type",
    supported_formats = {"syslog", "csv", "json"}
}

-- Main parsing function (required)
function parse_line(line)
    local entry = {}
    -- Parsing logic here
    return entry
end

-- Validation function (optional but recommended)
function validate_entry(entry)
    -- Validation logic here
    return true
end
```

## Parser Metadata

The `parser_info` table contains essential metadata:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Unique parser identifier |
| `version` | string | Yes | Semantic version (e.g., "1.2.3") |
| `description` | string | Yes | Human-readable description |
| `vendor` | string | Yes | Device vendor (cisco, juniper, etc.) |
| `device_type` | string | Yes | Device category (router, switch, firewall) |
| `supported_formats` | array | Yes | Log formats supported |

## Core Functions

### parse_line(line)

The main parsing function that processes individual log lines.

**Parameters:**
- `line` (string): Raw log line to parse

**Returns:**
- `entry` (table): Parsed log entry or `nil` if parsing fails

**Standard Entry Fields:**
```lua
entry = {
    timestamp = "2023-03-01T10:15:30.123Z",
    facility = "LINEPROTO",
    severity = 5,
    message = "Interface GigabitEthernet0/1 up",
    device_type = "cisco_ios",
    -- Additional device-specific fields
}
```

### validate_entry(entry)

Optional validation function to ensure data quality.

**Parameters:**
- `entry` (table): Parsed log entry

**Returns:**
- `boolean`: `true` if valid, `false` otherwise

## Built-in Utility Functions

### String Processing

```lua
-- Split string by delimiter
local parts = split_string(text, delimiter)

-- Trim whitespace
local clean = trim(text)

-- Check if string matches pattern
local matches = string_matches(text, pattern)
```

### Timestamp Parsing

```lua
-- Parse common timestamp formats
local timestamp = parse_timestamp(timestamp_string, format)

-- Supported formats:
-- "ios" - Cisco IOS format (*Mar  1 10:15:30.123)
-- "iso" - ISO 8601 format (2023-03-01T10:15:30.123Z)
-- "syslog" - RFC3164 format (Mar  1 10:15:30)
-- "custom" - User-defined format
```

### IP Address Utilities

```lua
-- Validate IP address
local is_valid = is_valid_ip(ip_string)

-- Extract IP addresses from text
local ips = extract_ips(text)

-- Check if IP is in subnet
local in_subnet = ip_in_subnet(ip, subnet_cidr)
```

### Network Interface Utilities

```lua
-- Normalize interface names
local normalized = normalize_interface(interface_name)
-- "Gi0/1" -> "GigabitEthernet0/1"

-- Parse interface type and number
local type, number = parse_interface(interface_name)
```

## Common Parsing Patterns

### Cisco Syslog Format

```lua
function parse_line(line)
    local entry = {}

    -- Parse timestamp: *Mar  1 10:15:30.123:
    local timestamp, rest = line:match("^(%*%w+%s+%d+%s+%d+:%d+:%d+%.%d+):%s*(.*)")
    if timestamp then
        entry.timestamp = parse_timestamp(timestamp, "ios")
    end

    -- Parse facility-severity-mnemonic: %LINEPROTO-5-UPDOWN:
    local facility, severity, mnemonic, message = rest:match("%%([%w_]+)%-(%d)%-([%w_]+):%s*(.*)")
    if facility then
        entry.facility = facility
        entry.severity = tonumber(severity)
        entry.mnemonic = mnemonic
        entry.message = message
    end

    return entry
end
```

### CSV Format Parsing

```lua
function parse_line(line)
    local entry = {}
    local fields = split_csv(line)

    if #fields >= 10 then
        entry.timestamp = fields[1]
        entry.src_ip = fields[2]
        entry.dst_ip = fields[3]
        entry.protocol = fields[4]
        -- Map additional fields as needed
    end

    return entry
end

function split_csv(line)
    local fields = {}
    local field = ""
    local in_quotes = false

    for i = 1, #line do
        local char = line:sub(i, i)
        if char == '"' then
            in_quotes = not in_quotes
        elseif char == ',' and not in_quotes then
            table.insert(fields, field)
            field = ""
        else
            field = field .. char
        end
    end

    table.insert(fields, field)
    return fields
end
```

### JSON Log Parsing

```lua
function parse_line(line)
    local entry = {}

    -- Parse JSON (requires json library)
    local json_data = json.decode(line)
    if json_data then
        entry.timestamp = json_data.timestamp
        entry.level = json_data.level
        entry.message = json_data.message
        entry.source = json_data.source

        -- Extract nested fields
        if json_data.event then
            entry.event_type = json_data.event.type
            entry.event_details = json_data.event.details
        end
    end

    return entry
end
```

## Advanced Features

### Multiline Log Processing

```lua
-- For logs that span multiple lines
parser_info.multiline = true

local multiline_buffer = ""

function parse_line(line)
    -- Check if this starts a new log entry
    if line:match("^%*%w+%s+%d+%s+%d+:%d+:%d+") then
        -- Process previous complete entry
        if multiline_buffer ~= "" then
            local entry = process_complete_entry(multiline_buffer)
            multiline_buffer = line
            return entry
        else
            multiline_buffer = line
        end
    else
        -- Continuation line
        multiline_buffer = multiline_buffer .. "\n" .. line
    end

    return nil -- No complete entry yet
end
```

### State Management

```lua
-- Maintain state across log entries
local parser_state = {
    current_session = nil,
    interface_states = {},
    bgp_neighbors = {}
}

function parse_line(line)
    local entry = {}

    -- Update state based on log content
    if line:match("BGP neighbor .* up") then
        local neighbor = line:match("BGP neighbor ([%d%.]+)")
        parser_state.bgp_neighbors[neighbor] = "up"
        entry.bgp_state_change = {
            neighbor = neighbor,
            state = "up"
        }
    end

    return entry
end
```

### Performance Optimization

```lua
-- Pre-compile regex patterns for better performance
local patterns = {
    timestamp = "^(%*%w+%s+%d+%s+%d+:%d+:%d+%.%d+):",
    syslog = "%%([%w_]+)%-(%d)%-([%w_]+):%s*(.*)",
    interface = "[Ii]nterface%s+([%w%d/%.%-]+)"
}

function parse_line(line)
    local entry = {}

    -- Use pre-compiled patterns
    local timestamp = line:match(patterns.timestamp)
    if timestamp then
        entry.timestamp = timestamp
    end

    return entry
end
```

## Error Handling

```lua
function parse_line(line)
    local entry = {}

    -- Always validate input
    if not line or line == "" then
        return nil
    end

    -- Use pcall for error handling
    local success, result = pcall(function()
        -- Parsing logic that might fail
        local timestamp = parse_timestamp(line)
        return timestamp
    end)

    if success then
        entry.timestamp = result
    else
        -- Log parsing error (optional)
        entry.parse_error = "Invalid timestamp format"
    end

    return entry
end

function validate_entry(entry)
    -- Ensure required fields are present
    if not entry.timestamp or not entry.message then
        return false
    end

    -- Validate field formats
    if entry.severity and (entry.severity < 0 or entry.severity > 7) then
        return false
    end

    return true
end
```

## Testing Your Parser

```lua
-- Include test cases in your parser
local test_cases = {
    {
        input = "*Mar  1 10:15:30.123: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up",
        expected = {
            facility = "LINEPROTO",
            severity = 5,
            mnemonic = "UPDOWN",
            interface = "GigabitEthernet0/1"
        }
    }
}

function run_tests()
    for i, test in ipairs(test_cases) do
        local result = parse_line(test.input)
        assert(result.facility == test.expected.facility, "Test " .. i .. " failed: facility mismatch")
        assert(result.severity == test.expected.severity, "Test " .. i .. " failed: severity mismatch")
        -- Add more assertions as needed
    end
    print("All tests passed!")
end
```

## Best Practices

### 1. Input Validation
- Always check for nil or empty input
- Validate field formats and ranges
- Handle malformed log entries gracefully

### 2. Performance
- Pre-compile regex patterns when possible
- Avoid expensive operations in tight loops
- Use local variables for better performance

### 3. Maintainability
- Use descriptive variable names
- Comment complex parsing logic
- Follow consistent coding style

### 4. Extensibility
- Design patterns to be reusable
- Support configuration options where appropriate
- Plan for future device firmware changes

### 5. Testing
- Include comprehensive test cases
- Test with real-world log samples
- Validate performance with large log files

## Common Pitfalls

### 1. Regex Performance
```lua
-- Bad: Compiling pattern every time
function parse_line(line)
    local result = line:match("^(%*%w+%s+%d+%s+%d+:%d+:%d+%.%d+):")
end

-- Good: Pre-compile patterns
local timestamp_pattern = "^(%*%w+%s+%d+%s+%d+:%d+:%d+%.%d+):"
function parse_line(line)
    local result = line:match(timestamp_pattern)
end
```

### 2. Memory Leaks
```lua
-- Bad: Accumulating data without cleanup
local all_entries = {}
function parse_line(line)
    local entry = parse_entry(line)
    table.insert(all_entries, entry) -- Memory grows indefinitely
    return entry
end

-- Good: Process entries individually
function parse_line(line)
    local entry = parse_entry(line)
    return entry -- Let NetLogAI handle storage
end
```

### 3. Error Propagation
```lua
-- Bad: Uncaught errors crash parser
function parse_line(line)
    local timestamp = parse_timestamp(line) -- May throw error
    return {timestamp = timestamp}
end

-- Good: Handle errors gracefully
function parse_line(line)
    local success, timestamp = pcall(parse_timestamp, line)
    if success then
        return {timestamp = timestamp}
    else
        return {parse_error = "Invalid timestamp"}
    end
end
```

## Integration with NetLogAI

Your parser integrates with NetLogAI Core through a standardized interface:

```bash
# Install parser
netlogai parser install my-parser.nlp

# Test parser
netlogai parser test my-parser.nlp sample.log

# Use in analysis
netlogai analyze --parser my-parser logs/
```

The parsed entries are automatically stored and indexed for analysis, AI queries, and reporting.

## Reference Implementation

See the [NetLogAI Parsers Repository](https://github.com/leochong/netlogai-parsers) for complete parser examples and the latest DSL features.