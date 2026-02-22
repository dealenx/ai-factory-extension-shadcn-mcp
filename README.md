# ai-factory-extension-shadcn-mcp

An [ai-factory](https://github.com/lee-to/ai-factory) extension that bundles the **shadcn/ui MCP** server for AI coding agents — tools to install and manage [shadcn/ui](https://ui.shadcn.com/) components directly from the agent.

## Quick Start

```bash
# from a local directory
ai-factory extension add ./ai-factory-extension-shadcn-mcp

# or from a git repo
ai-factory extension add https://github.com/dealenx/ai-factory-extension-shadcn-mcp.git
```

ai-factory merges the MCP server configs into every agent that supports MCP (Claude Code, Cline, etc.).

## What Gets Installed

After installation your agent's MCP configuration will include:

```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

## Project Structure

```
ai-factory-extension-shadcn-mcp/
├── extension.json       # Extension manifest (required)
├── mcp/                 # MCP server templates
│   └── shadcn.json      # shadcn/ui MCP server
├── package.json
├── LICENSE
└── README.md
```

## How It Works

### extension.json

The manifest declares which MCP servers the extension provides:

```json
{
  "name": "ai-factory-extension-shadcn-mcp",
  "version": "1.0.0",
  "description": "ai-factory extension: shadcn/ui MCP server",
  "mcpServers": [
    {
      "key": "shadcn",
      "template": "./mcp/shadcn.json",
      "instruction": "shadcn/ui MCP: provides tools to install and manage shadcn/ui components"
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `key` | Unique identifier for the server entry in the agent's settings file. |
| `template` | Path to a JSON file with the server's command, args, and env. |
| `instruction` | Message shown to the user after install (e.g. required env vars). |

### MCP Templates (`mcp/*.json`)

Each template follows the standard MCP server format:

**mcp/shadcn.json**
```json
{
  "command": "npx",
  "args": ["shadcn@latest", "mcp"]
}
```

## Managing the Extension

```bash
# List installed extensions
ai-factory extension list

# Remove the extension (cleans up MCP entries from agent configs)
ai-factory extension remove ai-factory-extension-shadcn-mcp
```

## Documentation

- [ai-factory Extensions Guide](https://github.com/lee-to/ai-factory/blob/2.x/docs/extensions.md) — full reference for extension manifest fields, MCP servers, injections, commands, skills, and agents.

## License

[MIT](LICENSE)
