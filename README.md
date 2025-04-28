# DenoKV MCP Server

Usage with Claude Desktop or Cursor

```json
{
  "mcpServers": {
    "denokv": {
      "command": "deno",
      "args": [
        "--unstable-kv",
        "--allow-env",
        "--allow-net",
        "--allow-read",
        "--allow-write",
        "--allow-run",
        "/Users/joshuayoes/Code/mcp-deno-kv/index.ts"
      ],
      "env": {
        "DENO_KV_PATH": "https://api.deno.com/databases/<UUID>/connect",
        "DENO_KV_ACCESS_TOKEN": "<DENO-DEPLOY-API-KEY>"
      }
    }
  }
}
```
