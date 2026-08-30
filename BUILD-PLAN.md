# Backlog — Build Plan

Tracked as GitHub Issues, one issue per phase ([#8](https://github.com/iowahawkeyedave/backlog/issues/8)–[#14](https://github.com/iowahawkeyedave/backlog/issues/14)); this file is the planning snapshot and the issues are the live tracker.

## Phase 1: Setup (30 min)

- [ ] Initialize React + Vite + TypeScript project
- [ ] Install dependencies per DEPENDENCIES.md (convex, zustand, tailwindcss, date-fns, lucide-react)
- [ ] Set up project structure (components/, lib/, stores/, types/)
- [ ] Configure Tailwind CSS with the token set from DESIGN.md
- [ ] Set up ESLint + Prettier (lint/format scripts) and Vitest
- [ ] Configure Convex project and generated API client
- [ ] Verify empty shell builds and runs; lint and tests pass

## Phase 2: Convex Backend Setup (45 min)

- [ ] Create Convex project
- [ ] Define schema (`items`, `lists`, `userPrefs`) with indexes including `by_user_url`
- [ ] Write queries, mutations, and the `addFromUrl` action
- [ ] Set up client connection in `lib/convex.ts`
- [ ] Anonymous id helper in the client (`crypto.randomUUID()` → `backlog:anonymous-id`, see TECH-SPEC.md Identity)
- [ ] Unit tests: content-type detection, URL normalization, dedupe lookup logic
- [ ] Test basic CRUD operations

## Phase 3: Core UI Components (60 min)

- [ ] Build `AppLayout` with sidebar and main content area
- [ ] Build `Sidebar` showing lists and filters
- [ ] Build `ItemCard` component (thumbnail, title, domain, status toggle)
- [ ] Build `ItemList` to render list of cards
- [ ] Build empty state component (teaching copy per DESIGN.md)
- [ ] Skeleton loading states for the feed

## Phase 4: Add Item Flow (45 min)

- [ ] Build `AddItemForm` with URL input
- [ ] Implement Convex `items.addFromUrl` action with fetch guards: http/https only; validate host and reject localhost, loopback, private, and reserved addresses; cap response size and timeout
- [ ] Duplicate URL handling: bump existing item to the top, return it, highlight in the UI
- [ ] Connect form to Convex action
- [ ] Show loading state while fetching metadata
- [ ] Handle errors (invalid URL, fetch failed)

## Phase 5: List Management (30 min)

- [ ] Build list creation UI
- [ ] Allow assigning items to lists
- [ ] Filter items by selected list
- [ ] Add/remove lists

## Phase 6: Search & Filter (30 min)

- [ ] Add search input in header
- [ ] Implement search by title/URL
- [ ] Add filter tabs: All / Open / Done / Archived
- [ ] Wire UI state from Zustand into Convex queries

## Phase 7: Polish & Test (45 min)

- [ ] Add animations for adding/removing items (state feedback only, 150–250 ms ease-out per DESIGN.md)
- [ ] Responsive design for mobile (sidebar drawer below 768px)
- [ ] Playwright smoke test: add URL → item appears → mark done → search finds it
- [ ] Test edge cases (empty state, long titles, missing thumbnails, duplicate URL)
- [ ] Final review and bug hunt

## Post-MVP (from PRD P1/P2)

- Tags and sort options (P1, deliberately deferred)
- Keyboard shortcuts (j/k nav, x to mark done), browser extension, mobile share sheet, bulk actions (P2)
- Analytics instrumentation for the PRD success metrics
