# Backlog — Technical Specification

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite |
| Language | TypeScript |
| Styling | Tailwind CSS |
| State | Zustand |
| Backend | SpacetimeDB (cloud-hosted, real-time sync) |
| URL Metadata | Custom scraper or link preview API |

## SpacetimeDB Schema

SpacetimeDB is a cloud-hosted database with automatic real-time sync to all connected clients. No backend server needed—we deploy modules directly to SpacetimeDB.

```sql
-- Main table: Saved items
TABLE SavedItem {
    id: string,           -- Unique ID (UUID)
    url: string,          -- The saved URL
    title: string,        -- Auto-fetched or user-edited
    description: string,  -- Auto-fetched description
    thumbnail: string,    -- URL to thumbnail image
    domain: string,       -- For filtering (youtube.com, etc.)
    content_type: string, -- "video" | "article" | "mixed"
    status: string,       -- "unwatched" | "watched" | "archived"
    created_at: u64,      -- Unix timestamp
    updated_at: u64,      -- Unix timestamp
    
    -- User-assigned
    tags: string[],       -- Array of tag strings
    list_id: string,       -- FK to List table (optional)
}

-- Table: Custom lists
TABLE List {
    id: string,           -- UUID
    name: string,         -- "YouTube", "Articles", etc.
    color: string,        -- Hex color for UI
    created_at: u64,
}

-- Table: User preferences
TABLE UserPrefs {
    key: string,          -- "theme", "sortOrder", etc.
    value: string,        -- JSON-encoded value
}
```

## Content Type Detection

When adding a URL, detect the type:

| Signal | Type |
|--------|------|
| YouTube, Vimeo, TikTok, Twitch links | Video |
| Medium, Substack, news sites | Article |
| Pages with both video embeds + text | Mixed |

Detection logic:
1. Check domain against known video platforms
2. Check for `og:type` meta tag (video, article)
3. Default to "article" if uncertain

## Data Flow

```
User Action → React Component → Zustand Store → SpacetimeDB (Cloud)
                                    ↓
                            All connected clients get real-time update
```

## API Routes (SpacetimeDB Reducers)

| Reducer | Parameters | Returns |
|---------|------------|---------|
| `add_item(url, title?, description?, thumbnail?, content_type?)` | URL, optional metadata | `SavedItem` |
| `update_status(id, status)` | id, status | `SavedItem` |
| `update_item(id, updates)` | id, partial SavedItem | `SavedItem` |
| `delete_item(id)` | id | - |
| `create_list(name, color)` | name, color | `List` |
| `delete_list(id)` | id | - |

## Subscriptions (Real-time Sync)

```typescript
// Client subscribes to all items
db.saved_items.subscribe("*")

// Or filter by list
db.saved_items.subscribe($"list_id = {activeListId}")
```

SpacetimeDB pushes updates automatically when data changes.

## Component Inventory

| Component | Location | Props | State |
|-----------|----------|-------|-------|
| `AppLayout` | layout/ | children | - |
| `Sidebar` | components/ | lists, activeList, onSelectList | - |
| `ItemCard` | components/items/ | item, onToggleStatus, onDelete | - |
| `ItemList` | components/items/ | items[], onToggle, onDelete | - |
| `AddItemForm` | components/add/ | onSubmit | isLoading |
| `SearchBar` | components/search/ | onSearch | query |
| `TagPill` | components/tags/ | tag, onRemove | - |
| `ContentTypeBadge` | components/items/ | type: video \| article \| mixed | - |
| `EmptyState` | components/ | message | - |

## File Structure

```
src/
├── components/
│   ├── items/
│   │   ├── ItemCard.tsx
│   │   ├── ItemList.tsx
│   │   └── ContentTypeBadge.tsx
│   ├── add/
│   │   └── AddItemForm.tsx
│   ├── search/
│   │   └── SearchBar.tsx
│   ├── tags/
│   │   └── TagPill.tsx
│   └── ui/              # Reusable UI (Button, Input, etc.)
├── lib/
│   ├── db.ts            # SpacetimeDB client setup
│   ├── scraper.ts       # URL metadata + content type detection
│   └── utils.ts
├── stores/
│   └── useStore.ts      # Zustand store
├── types/
│   └── index.ts
└── App.tsx
```

## Key Implementation Notes

1. **SpacetimeDB is cloud-hosted** — Data lives on their servers, not locally
2. **Real-time sync built-in** — Clients auto-update when data changes
3. **No traditional backend** — We write "reducers" (server functions) that deploy to SpacetimeDB
4. **Metadata scraping** — Use a simple fetch + DOMParser for MVP, or a link preview API
