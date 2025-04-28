# DenoKV MCP Server

## MCP Configuration Example

Below is an example configuration for integrating this server with an MCP client like Claude Desktop or Cursor:

```json
{
  "mcpServers": {
    "denokv": {
      "command": "deno",
      "args": [
        "run",
        "--unstable-kv",
        "--allow-env",
        "--allow-net",
        "--allow-read",
        "--allow-write",
        "--allow-run",
        "/path/to/your/mcp-deno-kv/index.ts" # Replace with the actual path
      ],
      "env": {
        "DENO_KV_PATH": "path/to/your/kv.db OR https://api.deno.com/databases/<UUID>/connect",
        "DENO_KV_ACCESS_TOKEN": "<YOUR_DENO_DEPLOY_ACCESS_TOKEN>" # Only needed for remote DB
      }
    }
  }
}
```

Make sure to replace `/path/to/your/mcp-deno-kv/index.ts` and the `env` values with your specific details.

## Environment Variables

This server requires the following environment variables to be set:

- `DENO_KV_PATH`: Specifies the path to the Deno KV database.
  - For a **local database**, this should be the file path (e.g., `./my-kv.db`).
  - For a **remote Deno Deploy database**, this should be the connection URL (e.g., `https://api.deno.com/databases/<UUID>/connect`).
- `DENO_KV_ACCESS_TOKEN`: Required **only if** `DENO_KV_PATH` points to a remote Deno Deploy database. This should be your Deno Deploy personal access token.

## Running the Server

To run the Deno KV MCP server, you need Deno installed. Execute the following command in your terminal:

```bash
deno run --unstable-kv --allow-env --allow-net --allow-read --allow-write --allow-run index.ts
```

Ensure the required environment variables are set before running the command.
