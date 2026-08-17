# Backlog — Coding Standards

## File Organization

```
src/
├── components/{feature}/   # Feature-scoped components
│   ├── items/ItemCard.tsx
│   ├── add/AddItemForm.tsx
│   └── search/SearchBar.tsx
├── components/ui/           # Shared UI primitives
├── lib/                     # Utilities and external integrations
├── stores/                  # Zustand stores
├── types/                   # TypeScript definitions
└── App.tsx                  # Root component
```

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `ItemCard.tsx` |
| Hooks | camelCase with `use` | `useItems.ts` |
| Utils | camelCase | `scraper.ts` |
| Types | PascalCase | `SavedItem.ts` |
| Files match default export | - | `ItemCard.tsx` → `export function ItemCard` |

## Component Structure

```tsx
// 1. Imports (React → libs → local)
import { useState } from 'react'
import { useStore } from '@/stores/useStore'
import { cn } from '@/lib/utils'
import { ItemCard } from './ItemCard'

// 2. Types
interface Props {
  item: SavedItem
  onToggle: (id: string) => void
  onDelete: (id: string) => void
}

// 3. Component
export function ItemCard({ item, onToggle, onDelete }: Props) {
  // State
  const [isHovered, setIsHovered] = useState(false)
  
  // Computed
  const statusColor = item.status === 'watched' ? 'text-green-500' : 'text-yellow-500'
  
  // Handlers
  const handleToggle = () => onToggle(item.id)
  
  // Render
  return (
    <div 
      className="relative rounded-lg overflow-hidden border"
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {item.thumbnail && (
        <img src={item.thumbnail} alt="" className="w-full h-32 object-cover" />
      )}
      <div className="p-3">
        <h3 className="font-medium truncate">{item.title || item.url}</h3>
        <p className={statusColor}>{item.status}</p>
      </div>
    </div>
  )
}
```

## State Management Rules

- **UI state**: Zustand store in `stores/useStore.ts` (search, active list, filters, dialogs)
- **Server data**: Convex `useQuery` / `useMutation` / `useAction` — do not copy items into Zustand
- **Component state**: `useState` for UI-only concerns (modals, hover, etc.)
- **No prop drilling**: Access store and Convex hooks directly, don't pass everything down
- **Derived data**: Use Zustand selectors for UI state, Convex queries for server data

## Convex Patterns (Cloud with Real-time Sync)

```typescript
import { useQuery, useMutation, useAction } from 'convex/react'
import { api } from '../../convex/_generated/api'

// Live query — re-renders when matching data changes
const items = useQuery(api.items.list, {
  status: activeFilter,
  listId: activeListId ?? undefined,
  search: debouncedQuery || undefined,
})

const updateStatus = useMutation(api.items.updateStatus)
const addFromUrl = useAction(api.items.addFromUrl)

// External HTTP belongs in Convex actions, not the client
try {
  await addFromUrl({ url })
} catch (err) {
  console.error('Failed to add item:', err)
  throw err
}

await updateStatus({ id: item._id, status: 'watched' })
```

Backend layout: `convex/schema.ts`, `convex/items.ts`, `convex/lists.ts`, `convex/prefs.ts`. Use mutations for writes, queries for reads, and actions only when calling outbound APIs (URL metadata).

## Tailwind Guidelines

- Use `cn()` utility for conditional classes
- Prefer semantic colors: `bg-primary`, `text-secondary`
- Mobile-first: `class="w-full md:w-1/2"`
- Consistent spacing: use scale (1 = 4px, 2 = 8px, 4 = 16px)

## Testing Checklist

Before marking complete, verify:
- [ ] Component renders without errors
- [ ] Props are typed correctly (no `any`)
- [ ] Loading states shown during async operations
- [ ] Error states handled gracefully
- [ ] Mobile responsive (test at 375px width)
- [ ] No console errors or warnings
- [ ] Works with empty data (empty arrays, nulls)
