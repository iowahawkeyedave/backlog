# Backlog — Deployment

## Decision

**Recommended: Vercel (frontend) + Convex Cloud (backend).** The deciding factor is preview deployments: every PR gets a live URL to test against the dev Convex deployment, which fits how this repo works (docs → PRs → issues). The migration cost if you switch later is one config file.

## Options considered

| Option | Pros | Cons | Verdict |
|--------|------|------|---------|
| **Vercel + Convex Cloud** | Per-PR preview deploys; deploy on git push; custom domains trivial; free tier fits an MVP | Second service to manage | **Recommended** |
| **Netlify + Convex Cloud** | Same shape as Vercel | No advantage over Vercel here | Equivalent |
| **Cloudflare Pages + Convex Cloud** | Fast edge, generous free tier | Same setup work as Vercel, no preview-deploy advantage | Fine alternative |
| **Convex built-in hosting** (`npx convex deploy`) | One service, fewest moving parts, zero extra config | No per-PR previews; convex.cloud URL unless you wire a domain | Pick if you want zero extra services |

## Environments

| | Dev | Prod |
|---|-----|------|
| Frontend | Local `npm run dev` + Vercel preview URLs | Vercel production |
| Convex | `npx convex dev` (dev deployment) | `npx convex deploy` (prod deployment) |
| `VITE_CONVEX_URL` | Dev deployment URL | Prod deployment URL |

Preview deploys must always point at the **dev** Convex deployment, never prod. Set `VITE_CONVEX_URL` per environment in the Vercel project settings; don't commit real values to git.

## Deploy flow (once code exists)

```bash
# Backend: push schema and functions to the prod deployment
npx convex deploy

# Frontend: connected via git integration (push to main → prod, PRs → previews)
# or manually:
vercel --prod
```

Server-side secrets (e.g. `METADATA_API_KEY`) live in Convex, never Vercel or the client bundle; see ENV.md.
