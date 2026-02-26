# Backlog — Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────┐
│                     React Frontend                       │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │  Sidebar │  │ ItemList │  │    AddItemForm        │  │
│  └──────────┘  └──────────┘  └──────────────────────┘  │
│         ↓              ↓                 ↓               │
│  ┌──────────────────────────────────────────────────┐   │
│  │              Zustand State Store                   │   │
│  │   - items[], lists[], searchQuery, filters       │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │    SpacetimeDB (Cloud)  │
              │   Real-time sync        │
              │  ┌────────────┐  ┌──────┴───────┐         │
              │  │ saved_items│  │ user_prefs   │         │
              │  └────────────┘  └──────────────┘         │
              └────────────────────────┘
```

## Data Flow

### Adding an Item
1. User pastes URL in `AddItemForm`
2. Form triggers `scraper.fetchMetadata(url)` + detect content type
3. Metadata returned → Zustand `addItem(item)`
4. Store calls SpacetimeDB reducer `add_item(...)`
5. SpacetimeDB persists → pushes update to all connected clients

### Filtering
1. User types in search or selects a list
2. Zustand updates filter state
3. Derived selector computes `filteredItems`
4. Component re-renders with filtered list

## State Management (Zustand)

```typescript
interface WatchLaterStore {
  // Data
  items: SavedItem[]
  lists: List[]
  
  // UI State
  searchQuery: string
  activeListId: string | null
  activeFilter: 'all' | 'unwatched' | 'watched' | 'archived'
  
  // Actions
  addItem: (url: string) => Promise<void>
  updateStatus: (id: string, status: string) => void
  deleteItem: (id: string) => void
  createList: (name: string, color: string) => void
  setSearch: (query: string) => void
}
```

## URL Metadata Strategy

### MVP Approach
- Use simple `fetch(url)` + `DOMParser` to extract Open Graph tags
- Fallback: Use a free metadata API (like linkpreview.net free tier)
- Cache results in SpacetimeDB to avoid re-scraping

### Production Approach (Future)
- Dedicated microservice or Cloudflare Worker
- Screenshot generation for visual preview
- Video-specific metadata (YouTube duration, etc.)

## Sync Strategy

**Phase 1 (MVP)**: Local-only via SpacetimeDB embedded
**Phase 2**: Enable SpacetimeDB sync for cross-device
