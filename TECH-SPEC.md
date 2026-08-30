# Backlog — Technical Specification

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite |
| Language | TypeScript |
| Styling | Tailwind CSS |
| State | Zustand |
| Backend | Convex |
| Auth | Convex Auth or anonymous-first MVP |
| URL Metadata | Convex action + link preview API or direct fetch parser |

## Convex Data Model

Convex provides the database, backend functions, and realtime subscriptions. The client uses generated API bindings and live queries from React.

```ts
// convex/schema.ts
defineSchema({
  items: defineTable({
    userId: v.optional(v.string()),
    url: v.string(),
    title: v.string(),
    description: v.optional(v.string()),
    thumbnail: v.optional(v.string()),
    domain: v.string(),
    contentType: v.union(
      v.literal("video"),
      v.literal("article"),
      v.literal("mixed")
    ),
    status: v.union(
      v.literal("open"),
      v.literal("done"),
      v.literal("archived")
    ),
    tags: v.array(v.string()),
    listId: v.optional(v.id("lists")),
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_user", ["userId"])
    .index("by_user_status", ["userId", "status"])
    .index("by_user_domain", ["userId", "domain"])
    .index("by_user_url", ["userId", "url"])
    .searchIndex("search_title", {
      searchField: "title",
      filterFields: ["userId", "status", "domain"],
    }),

  lists: defineTable({
    userId: v.optional(v.string()),
    name: v.string(),
    color: v.string(),
    createdAt: v.number(),
  }).index("by_user", ["userId"]),

  userPrefs: defineTable({
    userId: v.optional(v.string()),
    sortOrder: v.optional(v.string()),
    theme: v.optional(v.string()),
  }).index("by_user", ["userId"]),
})
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

## Identity (MVP)

Anonymous-first, no auth in the MVP:

1. On first load the client generates an anonymous id with `crypto.randomUUID()` (no `uuid` package) and persists it in localStorage under the key `backlog:anonymous-id`.
2. Every query, mutation, and action receives that id as a `userId` argument; it is stored in the schema's `userId` fields. The client sends nothing else identifying.
3. When real accounts arrive later, add a mapping from account to anonymous id (a users table or one-time claim mutation) so existing items migrate without reshaping the schema.

## Duplicate URL Handling

Adding a URL that already exists for the user never creates a second item. Rationale: a re-add signals the item matters.

1. Normalize the URL first (force https, strip trailing slash and tracking params), then query the `by_user_url` index.
2. On match, the action updates `updatedAt` on the existing item, which surfaces it at the top of the newest-first feed, and returns it so the UI can highlight it instead of showing a generic success.
3. No match: proceed with metadata fetch and creation.

## Data Flow

```
User Action → React Component → Zustand UI Store → Convex mutation/action
                                    ↓
                              Convex database update
                                    ↓
                        useQuery/live subscription re-renders UI
```

## Backend Functions (Convex)

| Function | Type | Purpose |
|----------|------|---------|
| `items.addFromUrl` | `action` | Dedupe by URL, fetch metadata (with fetch guards, see ARCHITECTURE.md), detect content type, then create or bump |
| `items.create` | `mutation` | Insert item when metadata already exists |
| `items.updateStatus` | `mutation` | Toggle `open` / `done` / `archived` |
| `items.update` | `mutation` | Edit title, tags, list, or description |
| `items.remove` | `mutation` | Delete item |
| `items.list` | `query` | Return items for current filters |
| `items.search` | `query` | Search title/URL scoped to current user |
| `lists.create` | `mutation` | Create list |
| `lists.remove` | `mutation` | Delete list |
| `lists.list` | `query` | Return user lists |
| `prefs.get` | `query` | Read user preferences |
| `prefs.update` | `mutation` | Save theme/sort/filter defaults |

## Realtime Pattern

```typescript
// Query drives UI directly
const items = useQuery(api.items.list, {
  status: activeFilter,
  listId: activeListId ?? undefined,
  search: debouncedQuery || undefined,
})

// Mutations update server state
const updateStatus = useMutation(api.items.updateStatus)
```

Convex re-runs subscribed queries automatically when matching data changes.

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
│   ├── convex.ts        # Convex client setup
│   ├── metadata.ts      # Shared metadata parsing helpers
│   └── utils.ts
├── stores/
│   └── useStore.ts      # Zustand UI state only
├── types/
│   └── index.ts
└── App.tsx

convex/
├── schema.ts
├── items.ts
├── lists.ts
├── prefs.ts
└── tsconfig.json
```

## Key Implementation Notes

1. **Convex owns backend state** — Zustand should only keep ephemeral UI state like search text, dialogs, and local filter controls.
2. **Use actions for metadata fetches** — External HTTP calls belong in Convex actions, not client components.
3. **Queries are the source of truth** — Prefer `useQuery` over copying server data into Zustand.
4. **Auth can stay optional in MVP** — Start anonymous or single-user, but keep `userId` fields in the schema so multi-user auth can be added without reshaping the app.
5. **Search can start simple** — Use Convex search for title and client-side fallback on URL if exact URL search is needed early.
