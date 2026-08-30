# Backlog — Environment Variables

Two separate homes, and the difference matters for secrets: anything `VITE_`-prefixed is bundled into the client JavaScript, so a secret placed there is public.

## Client (Vite, `.env.local`)

```bash
VITE_CONVEX_URL=""        # Convex deployment URL, from npx convex dev
VITE_APP_TITLE="Backlog"
```

## Convex server (`npx convex env set ...`)

Read inside actions via `process.env`. Set with:

```bash
npx convex env set METADATA_API_URL "https://api.linkpreview.net"
npx convex env set METADATA_API_KEY "<key>"
```

Both are optional: without them, `items.addFromUrl` falls back to direct fetch + meta tag parsing.

## Setup order

1. `npx convex dev` to provision the project and generate client bindings
2. Copy the dev deployment URL into `VITE_CONVEX_URL`
3. Add metadata provider credentials via `npx convex env set` if using one
4. For per-environment handling (prod deployment, Vercel settings), see DEPLOYMENT.md
