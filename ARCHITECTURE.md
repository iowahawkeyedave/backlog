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
│  │              Zustand UI Store                      │   │
│  │   - searchQuery, activeListId, local filters      │   │
│  └──────────────────────────────────────────────────┘   │
│                         ↓                               │
│  ┌──────────────────────────────────────────────────┐   │
│  │                Convex React Client                │   │
│  │      - useQuery / useMutation / useAction         │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                           ↓
              ┌────────────────────────────────┐
              │            Convex              │
              │   ┌──────────┐ ┌────────────┐  │
              │   │ queries  │ │ mutations  │  │
              │   └──────────┘ └────────────┘  │
              │   ┌──────────┐ ┌────────────┐  │
              │   │ actions  │ │ database   │  │
              │   └──────────┘ └────────────┘  │
              └────────────────────────────────┘
```

## Data Flow

### Adding an Item
1. User pastes URL in `AddItemForm`
2. Form calls Convex action `items.addFromUrl`
3. Action fetches metadata, normalizes fields, and detects content type
4. Action writes item through an internal mutation
5. Matching queries re-run and the React list updates automatically

### Filtering
1. User types in search or selects a list
2. Zustand updates UI filter state
3. Components pass the active filters into `useQuery(api.items.list, filters)`
4. Convex returns filtered items and re-renders subscribers on change

## State Management Split

```typescript
interface WatchLaterStore {
  // UI State
  searchQuery: string
  activeListId: string | null
  activeFilter: 'all' | 'unwatched' | 'watched' | 'archived'
  isAddItemOpen: boolean
  
  // UI Actions
  setSearch: (query: string) => void
  setActiveList: (id: string | null) => void
  setActiveFilter: (filter: Filter) => void
  setAddItemOpen: (isOpen: boolean) => void
}
```

Server data should not be mirrored into Zustand unless there is a very specific offline or optimistic UI need.

## URL Metadata Strategy

### MVP Approach
- Use Convex action `items.addFromUrl`
- First try a metadata API for stable previews
- Fallback to direct fetch + HTML meta tag parsing when allowed
- Persist normalized metadata in Convex so the client never re-scrapes the same URL

### Production Approach (Future)
- Move extraction into a dedicated worker if rate limits or CORS issues appear
- Screenshot generation for visual preview
- Video-specific metadata (YouTube duration, etc.)

## Sync Strategy

**Phase 1 (MVP)**: Convex-hosted single-user or anonymous-first app with realtime queries
**Phase 2**: Add auth, per-user isolation, and optional shared lists
