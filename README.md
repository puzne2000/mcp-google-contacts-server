# MCP Google Contacts Server

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server that gives AI assistants access to Google Contacts. Supports listing, searching, creating, updating, and deleting contacts, as well as searching Google Workspace directories.

Forked from [RayanZaki/mcp-google-contacts-server](https://github.com/RayanZaki/mcp-google-contacts-server) with the following bug fixes:
- `search_contacts` now uses the Google People API's native `searchContacts` endpoint instead of fetching the first 100 contacts and filtering locally — searches across all contacts regardless of list size
- `list_contacts` now paginates through all results instead of stopping at the first page
- `get_contact` by email address now uses the server-side search instead of scanning a partial contact list
- Removed a stray debug print statement in `list_directory_people`

## Prerequisites

- Python 3.12 or higher
- A Google Cloud project with the [People API](https://console.cloud.google.com/apis/library/people.googleapis.com) enabled
- OAuth 2.0 credentials (Desktop app type) downloaded from Google Cloud Console

## Installation

```bash
git clone https://github.com/puzne2000/mcp-google-contacts-server.git
cd mcp-google-contacts-server
pip install .
```

This installs the `mcp-google-contacts` command into your PATH.

## Authentication

### Step 1 — Place your credentials file

Download your OAuth client credentials from Google Cloud Console and place them at:

```
~/.config/google/credentials.json
```

Alternatively, pass the path explicitly with `--credentials-file`, or use environment variables:

```
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
GOOGLE_REFRESH_TOKEN=...
```

### Step 2 — Run the initial auth flow

Run this once in your terminal to open a browser and authorize access:

```bash
mcp-google-contacts
```

Sign in with the Google account whose contacts you want to manage. The token is saved to `~/.config/google-contacts-mcp/token.json` and auto-refreshes — you won't need to repeat this unless you revoke access.

Once you see `Running with stdio transport`, press Ctrl+C. Setup is complete.

## MCP Configuration

Add this to your `.mcp.json` (or `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "google-contacts": {
      "command": "mcp-google-contacts",
      "args": []
    }
  }
}
```

## Available Tools

| Tool | Description |
|---|---|
| `search_contacts` | Search contacts by name, email, or phone (server-side, searches all contacts) |
| `list_contacts` | List all contacts, optionally filtered by name |
| `get_contact` | Get a contact by resource name (`people/...`) or email address |
| `create_contact` | Create a new contact |
| `update_contact` | Update an existing contact |
| `delete_contact` | Delete a contact |
| `list_workspace_users` | List users in your Google Workspace directory |
| `search_directory` | Search your Google Workspace directory |
| `get_other_contacts` | List contacts from the "Other contacts" section |

## License

MIT — see [LICENSE](LICENSE).
