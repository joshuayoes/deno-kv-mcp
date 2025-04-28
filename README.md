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

## Available Tools

This server provides the following tools:

- **`set`**: Set a key-value pair in the Deno KV store.
  - `key` (array of strings): The key to set.
  - `value` (string): The value to set (JSON string).
  - `expireIn` (number, optional): Time-to-live (TTL) for the key in milliseconds.
- **`get`**: Get a value by key from the Deno KV store.
  - `key` (array of strings): The key to get.
  - `consistency` (enum: "strong" | "eventual", optional): Consistency level for the read.
- **`delete`**: Delete a key-value pair from the Deno KV store.
  - `key` (array of strings): The key to delete.
- **`getMany`**: Get multiple values by keys from the Deno KV store.
  - `keys` (array of array of strings): The keys to get.
  - `consistency` (enum: "strong" | "eventual", optional): Consistency level for the read.
- **`list`**: List key-value pairs based on a selector.
  - `prefix` (array of strings, optional): Key prefix to list (e.g., `["users"]`).
  - `start` (array of strings, optional): Start key for range queries (e.g., `["orders", "2023"]`).
  - `end` (array of strings, optional): End key for range queries (e.g., `["orders", "2024"]`).
  - `limit` (integer, positive, optional): Maximum number of entries to return.
  - `consistency` (enum: "strong" | "eventual", optional): Consistency level for the list operation.
  - `batchSize` (integer, positive, optional): Number of entries to fetch per batch internally.
  - `reverse` (boolean, optional): Whether to reverse the order of entries.
  - _Note:_ Must provide `prefix`, `start`, or (`start` and `end`) parameter. `end` requires `start` or `prefix`.
- **`enqueue`**: Enqueue a message into the Deno KV queue.
  - `value` (string): The value to enqueue (JSON string).
  - `delay` (integer, non-negative, optional): Delay in milliseconds before the message is delivered.
  - `keysIfUndelivered` (array of array of strings, optional): Keys to set if the message is not successfully delivered.
  - `backoffSchedule` (array of positive integers, optional): Retry backoff schedule in milliseconds.
- **`reset`**: Delete **all** keys from the Deno KV store (requires confirmation).
  - `confirmation` (literal: "yes"): Must be 'yes' to confirm deletion.
