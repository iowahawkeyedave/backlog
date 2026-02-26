# Watch Later App — Environment Variables

```bash
# SpacetimeDB (cloud-hosted)
# Get these from your SpacetimeDB dashboard after creating a module
SPACETIMEDB_DATABASE_NAME="watch-later"
SPACETIMEDB_MODULE_NAME="watch_later"
SPACETIMEDB_TOKEN=""  # Auth token for your database

# Optional: Metadata API (for better URL previews)
# If not set, uses simple DOM parsing
METADATA_API_URL="https://api.linkpreview.net"
METADATA_API_KEY=""  # Get free key at linkpreview.net

# Development
VITE_APP_TITLE="Backlog"
```

## SpacetimeDB Setup

1. Go to spacetimedb.com and create an account
2. Create a new database (e.g., "watch-later")
3. Deploy your module with the schema
4. Copy the database name and token to these variables
