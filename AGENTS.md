# AGENT NOTES

## Deployment

**This project uses Dokploy for deployment.**

- **DO NOT uncomment or modify the `ports:` section in docker-compose.yml** - Dokploy handles port mapping automatically
- The docker-compose.yml is configured for Dokploy's internal networking
- External port access is managed through Dokploy's interface, not through docker-compose

## Architecture

This is a single Docker container that runs:
1. **Chrome browser** with CDP enabled on port 9222 (internal)
2. **Playwright MCP server** on port 3002
3. **KasmVNC** web desktop on ports 3000/3001

Both Chrome and the MCP server run **inside the same container**, so they communicate via `localhost:9222` - no special networking configuration needed.

## Configuration

- **config.json location**: `/config/config.json` (mounted volume)
- **cdpEndpoint**: Must be `http://localhost:9222` (same container)
- **allowExternalConnections**: Must be `true` to accept connections from outside the container
- **host**: Must be `0.0.0.0` to bind to all interfaces inside container

## Common Issues

**"Access is only allowed at localhost:3002"**
- Solution: Ensure `allowExternalConnections: true` in config.json
- The MCP server host is already set to `0.0.0.0`
- After config changes, rebuild the container
