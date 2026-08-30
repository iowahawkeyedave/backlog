# Backlog

A calm, fast place to save any URL (video or article) and actually come back to it. Backlog auto-fetches the title, description, and thumbnail, detects whether it's a video or an article, and keeps everything in one realtime feed with search, lists, and an open/done status.

React + Vite + Tailwind + Zustand on the frontend, Convex on the backend. Anonymous-first: no account needed for the MVP.

> Status: planning phase. There is no code yet; the docs below are the spec.

## Documentation map

| Doc | Answers |
|-----|---------|
| [PRD.md](PRD.md) | What we're building and why: problem, users, MVP scope (P0/P1/P2), success metrics |
| [PRODUCT.md](PRODUCT.md) | Design strategy: register, users, brand personality, anti-references, design principles |
| [ARCHITECTURE.md](ARCHITECTURE.md) | How the pieces fit: system diagram, data flow, state split, URL metadata strategy |
| [TECH-SPEC.md](TECH-SPEC.md) | Stack, Convex schema, function inventory, identity, duplicate-URL handling |
| [DESIGN.md](DESIGN.md) | How it looks: visual direction, color strategy, typography, guardrails (seed, pre-code) |
| [BUILD-PLAN.md](BUILD-PLAN.md) | Execution order: phases and what to test at each step (tracked in GitHub Issues) |
| [DEPENDENCIES.md](DEPENDENCIES.md) | Planned packages (planning snapshot; `package.json` governs once it exists) |
| [ENV.md](ENV.md) | Environment variables: client (Vite) vs server (Convex), and how to set each |
| [DEPLOYMENT.md](DEPLOYMENT.md) | Hosting options comparison and the deploy flow |
| [AGENTS.md](AGENTS.md) | Coding standards for AI agents (and humans) working in this repo |

## Working on this repo

- Task tracking lives in GitHub Issues, one issue per build-plan phase.
- Docs first: when a decision changes, update the relevant doc before the code.
