# Larapex Charts Documentation MCP Server

A Model Context Protocol (MCP) server that provides access to Larapex Charts documentation. This server allows AI assistants to search and retrieve documentation for Larapex Charts directly from [https://larapex-charts.netlify.app/](https://larapex-charts.netlify.app/).

## Features

- **Search Documentation**: Search for chart types and topics across Larapex Charts docs
- **Get Chart Type Info**: Retrieve detailed documentation for specific chart types
- **List Components**: Browse all available chart types and documentation pages organized by category
- **Get Page Content**: Fetch any documentation page by path
- **Local Caching**: Automatically caches documentation pages locally for faster access and offline availability of previously fetched pages

This server provides access to documentation covering Larapex Charts installation, configuration, chart types, customization, and advanced features.

## Installation

### From Source

```bash
git clone https://github.com/jagocki/larapex-charts-docs-mcp.git
cd larapex-charts-docs-mcp
npm install
npm run build
```

## Usage

### Configuration

The server can be configured using the following environment variables:

- `LARAPEX_DOCS_URL` (optional): Base URL for Larapex Charts documentation. Defaults to `https://larapex-charts.netlify.app`
- `MAX_CONTENT_SIZE` (optional): Maximum content size in characters for documentation pages. Defaults to `15000`
- `CACHE_DIR` (optional): Directory for caching documentation pages. Defaults to `.cache`
- `CACHE_TTL` (optional): Cache time-to-live in seconds. Defaults to `3600` (1 hour)

#### Caching

The server automatically caches fetched documentation pages locally to improve performance and enable offline access to previously fetched pages. The cache:

- Stores pages in the `.cache` directory (configurable via `CACHE_DIR`)
- Expires after 1 hour by default (configurable via `CACHE_TTL`)
- Reduces load on the documentation website
- Provides faster response times after the first fetch
- Enables offline access to pages that have been previously cached

To clear the cache, simply delete the cache directory:

```bash
rm -rf .cache
```

### Requirements

- Node.js >= 20.18.1

### With Claude Desktop

Add this to your Claude Desktop configuration file (see `claude_desktop_config.example.json` for a complete example):

**MacOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "larapex-charts-docs": {
      "command": "node",
      "args": ["/path/to/larapex-charts-docs-mcp/dist/index.js"]
    }
  }
}
```

Or if installed globally via npm:

```json
{
  "mcpServers": {
    "larapex-charts-docs": {
      "command": "larapex-charts-docs-mcp"
    }
  }
}
```

### With VSCode

To use this MCP server with VSCode, you'll need to install an MCP-compatible extension.

#### Using Cline Extension (Recommended)

1. **Install the Cline extension** from the VSCode marketplace:
   - Open VSCode
   - Go to Extensions (Ctrl+Shift+X / Cmd+Shift+X)
   - Search for "Cline"
   - Click Install

2. **Configure the MCP server**:
   - Open VSCode settings (File > Preferences > Settings or Ctrl+,)
   - Search for "Cline MCP"
   - Click "Edit in settings.json"
   - Add the following configuration:

```json
{
  "cline.mcpServers": {
    "larapex-charts-docs": {
      "command": "node",
      "args": ["/absolute/path/to/larapex-charts-docs-mcp/dist/index.js"],
      "env": {
        "CACHE_DIR": "/absolute/path/to/larapex-charts-docs-mcp/.cache"
      }
    }
  }
}
```

**Important:** Use absolute paths for both the server executable and the CACHE_DIR to ensure the cache is created in the correct location. See `vscode_settings.example.json` for a complete example.

3. **Using the tools**:
   - Open Cline in VSCode (click the Cline icon in the sidebar)
   - The Larapex Charts documentation tools will be available to the AI assistant
   - Ask questions like "Show me the Larapex Charts line chart documentation"

#### Alternative: Using Continue Extension

1. **Install Continue extension** from the VSCode marketplace

2. **Configure MCP server** in Continue's config file (`~/.continue/config.json`):

```json
{
  "mcpServers": [
    {
      "name": "larapex-charts-docs",
      "command": "node",
      "args": ["/absolute/path/to/larapex-charts-docs-mcp/dist/index.js"]
    }
  ]
}
```

#### Tips for VSCode Usage

- Make sure to use absolute paths in your configuration
- Restart VSCode after adding the MCP server configuration
- Check the extension's output panel if the server doesn't connect
- The cache will be created in the server's directory, making subsequent queries faster

### With Other MCP Clients

The server uses stdio for communication. Run it with:

```bash
npm start
# or
node dist/index.js
```

## Available Tools

### `search_docs`

Search Larapex Charts documentation for chart types or topics.

**Parameters:**

- `query` (string, required): Search query (chart type, topic, or keyword)

**Example:**

```json
{
  "query": "installation"
}
```

### `get_component`

Get documentation for a specific chart type or topic.

**Parameters:**

- `component` (string, required): Page name (e.g., 'installation', 'simple-example', 'customization')

**Example:**

```json
{
  "component": "installation"
}
```

### `list_components`

List all available Larapex Charts documentation pages organized by category.

**No parameters required.**

### `get_page`

Get the content of a specific documentation page.

**Parameters:**

- `path` (string, required): Documentation page name (e.g., 'installation', 'simple-example', 'customization')

**Example:**

```json
{
  "path": "installation"
}
```

## Documentation Categories

Larapex Charts documentation is organized into the following categories:

- **Examples** (4 pages): Installation, simple-example, more-charts, customization
- **Advance** (5 pages): Charts with Eloquent, Charts with InertiaJS, Charts Stubs, Host Library, Support

## Development

```bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Watch mode for development
npm run watch

# Run the server
npm start
```

## About Larapex Charts

Larapex Charts is a Laravel wrapper for ApexCharts library, making it easy to create beautiful, interactive charts in Laravel applications without writing JavaScript. For more information, visit [larapex-charts.netlify.app](https://larapex-charts.netlify.app/).

## License

MIT