# Backlog — Environment Variables

```bash
# Convex
VITE_CONVEX_URL=""

# Optional metadata provider
METADATA_API_URL="https://api.linkpreview.net"
METADATA_API_KEY=""

# Development
VITE_APP_TITLE="Backlog"
```

## Convex Setup

1. Create a Convex project
2. Run `npx convex dev` to provision the local project and generate the client bindings
3. Copy the deployed Convex URL into `VITE_CONVEX_URL` if your frontend setup needs it
4. Add metadata API credentials if using a third-party preview service
