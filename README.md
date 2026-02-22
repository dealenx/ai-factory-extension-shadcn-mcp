# ai-factory-extension-mcp-template

A minimal template for packaging MCP servers as an [ai-factory](https://github.com/lee-to/ai-factory) extension. Use it to quickly distribute one or more MCP servers across all agents managed by ai-factory.

## Quick Start

1. Click **"Use this template"** on GitHub to create your own repository from this template, then clone it. Alternatively, fork or clone this repository directly.
2. Edit `extension.json` — set your extension `name`, `version`, `description`, and list every MCP server in the `mcpServers` array.
3. For each server, create a JSON template in the `mcp/` directory.
4. Install the extension into your project:

```bash
# from a local directory
ai-factory extension add ./ai-factory-extension-mcp-template

# or from a git repo
ai-factory extension add https://github.com/<user>/<repo>.git
```

That's it — ai-factory merges the MCP server configs into every agent that supports MCP (Claude Code, Cline, etc.).

## Project Structure

```
ai-factory-extension-mcp-template/
├── extension.json       # Extension manifest (required)
├── mcp/                 # MCP server templates
│   └── playwright.json  # Example: Playwright MCP server
├── package.json
├── LICENSE
└── README.md
```

## How It Works

### extension.json

The manifest declares which MCP servers the extension provides:

```json
{
  "name": "ai-factory-extension-mcp-template",
  "version": "1.0.0",
  "description": "Template: hello MCP server inside ai-factory extension",
  "mcpServers": [
    {
      "key": "playwright",
      "template": "./mcp/playwright.json",
      "instruction": "Playwright MCP: Install Chrome to use a web browser"
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `key` | Unique identifier for the server entry in the agent's settings file. |
| `template` | Path to a JSON file with the server's command, args, and env. |
| `instruction` | Message shown to the user after install (e.g. required env vars). |

### MCP Template (`mcp/*.json`)

Each template follows the standard MCP server format:

```json
{
  "command": "npx",
  "args": ["@playwright/mcp@latest"]
}
```

You can also pass environment variables:

```json
{
  "command": "npx",
  "args": ["-y", "@my-org/my-mcp-server"],
  "env": {
    "MY_API_KEY": "${MY_API_KEY}"
  }
}
```

## Adding More MCP Servers

1. Create a new template file in `mcp/`, e.g. `mcp/my-server.json`.
2. Add a new entry to the `mcpServers` array in `extension.json`:

```json
{
  "key": "my-server",
  "template": "./mcp/my-server.json",
  "instruction": "Set MY_API_KEY environment variable before use"
}
```

3. Re-install the extension to apply:

```bash
ai-factory extension remove ai-factory-extension-mcp-template
ai-factory extension add ./ai-factory-extension-mcp-template
```

## Managing the Extension

```bash
# List installed extensions
ai-factory extension list

# Remove the extension (cleans up MCP entries from agent configs)
ai-factory extension remove ai-factory-extension-mcp-template
```

## Documentation

- [ai-factory Extensions Guide](https://github.com/lee-to/ai-factory/blob/2.x/docs/extensions.md) — full reference for extension manifest fields, MCP servers, injections, commands, skills, and agents.

## License

[MIT](LICENSE)
