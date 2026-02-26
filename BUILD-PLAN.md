# Backlog — Build Plan

## Phase 1: Setup (30 min)

- [ ] Initialize React + Vite + TypeScript project
- [ ] Install dependencies: `zustand`, `@spacetimeDB/client`, `tailwindcss`, `uuid`
- [ ] Set up project structure (components/, lib/, stores/, types/)
- [ ] Configure Tailwind CSS
- [ ] Verify empty shell builds and runs

## Phase 2: SpacetimeDB Setup (45 min)

- [ ] Create SpacetimeDB account at spacetimedb.com
- [ ] Create new database module for the app
- [ ] Define schema (SavedItem, List, UserPrefs tables)
- [ ] Write reducer functions (add_item, update_status, delete_item, etc.)
- [ ] Deploy module to SpacetimeDB cloud
- [ ] Install `@spacetimeDB/client` SDK
- [ ] Set up client connection in `db.ts`
- [ ] Test basic CRUD operations

## Phase 3: Core UI Components (60 min)

- [ ] Build `AppLayout` with sidebar and main content area
- [ ] Build `Sidebar` showing lists and filters
- [ ] Build `ItemCard` component (thumbnail, title, domain, status toggle)
- [ ] Build `ItemList` to render list of cards
- [ ] Build empty state component

## Phase 4: Add Item Flow (45 min)

- [ ] Build `AddItemForm` with URL input
- [ ] Implement `scraper.ts` for metadata fetching
- [ ] Connect form to SpacetimeDB `add` function
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
- [ ] Add filter tabs: All / Unwatched / Watched / Archived
- [ ] Wire up Zustand selectors

## Phase 7: Polish (30 min)

- [ ] Add keyboard shortcuts (? for help, j/k for nav)
- [ ] Add animations for adding/removing items
- [ ] Responsive design for mobile
- [ ] Test edge cases (empty state, long titles, missing thumbnails)
- [ ] Final review and bug hunt
