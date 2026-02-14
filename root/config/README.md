# MCP Server Configuration

This directory contains configuration files for the Playwright MCP (Model Context Protocol) server.

## Files

### `config.json`
Main configuration file for the Playwright MCP server.

**Current settings:**
```json
{
    "browser": {
        "isolated": true,
        "cdpEndpoint": "http://localhost:9222",
        "launchOptions": {
            "headless": false
        },
        "contextOptions": {}
    },
    "server": {
        "port": 3002,
        "host": "0.0.0.0",
        "allowedHosts": ["*"]
    },
    "capabilities": ["pdf", "vision"],
    "outputDir": "/config/output",
    "imageResponses": "allow",
    "network": {
        "allowedOrigins": ["*"],
        "blockedOrigins": []
    }
}
```

### `output/`
Directory where automation results, screenshots, PDFs, and other outputs are saved.

## Configuration Options

### Browser Settings
- **`isolated: true`**: Each browser session runs in isolation
- **`headless: false`**: Browser runs with UI visible (important for desktop viewing)
- **`cdpEndpoint`**: Chrome DevTools Protocol endpoint URL (e.g., `http://localhost:9222`)
- **`contextOptions`**: Additional Playwright browser context options

### CDP Configuration
```json
{
    "browser": {
        "cdpEndpoint": "http://localhost:9222"
    }
}
```

The MCP server can connect to an existing Chrome instance via CDP (Chrome DevTools Protocol). This is useful when you want the browser to persist across MCP server restarts or when integrating with external tools.

### Server Settings
- **`port: 3002`**: MCP server listens on this port
- **`host: "0.0.0.0"`**: Accepts connections from any IP (required for container networking)
- **`allowedHosts: ["*"]`**: Allows connections from any host (required for external access)
- **Note**: Command-line arguments `--host 0.0.0.0 --port 3002 --allowed-hosts *` should also be passed when starting the MCP server

### Capabilities
- **`pdf`**: Enables PDF generation from web pages
- **`vision`**: Enables visual/screenshot capabilities

### Output Configuration
- **`outputDir`**: Directory for saving automation artifacts
- **`imageResponses: "allow"`**: Permits image-based responses

### Network Securit: ["*"]`**: Allows all origins (use specific domains for production
- **`allowedOrigins`**: Whitelist of allowed domains (empty = all allowed)
- **`blockedOrigins`**: Blacklist of blocked domains

## Customization

### Changing Browser Behavior
```json
{
    "browser": {
        "isolated": true,
        "launchOptions": {
            "headless": false,
            "args": ["--no-sandbox", "--disable-dev-shm-usage"]
        }
    }
}
```

### Network Restrictions
```json
{
    "network": {
        "allowedOrigins": ["https://example.com", "https://api.service.com"],
        "blockedOrigins": ["https://blocked-site.com"]
    }
}
```

### Additional Capabilities
```json
{
    "capabilities": ["pdf", "vision", "recording"]
}
```

## Volume Mounting

When running the container, mount this directory to persist configurations and outputs:
```bash
-v $(pwd)/config:/config
```

This allows you to:
- Modify `config.json` from the host
- Access outputs in the `output/` directory
- Persist configuration changes across container restarts