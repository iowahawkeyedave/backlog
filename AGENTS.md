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

- **Global state**: Zustand store in `stores/useStore.ts`
- **Component state**: `useState` for UI-only concerns (modals, hover, etc.)
- **No prop drilling**: Access store directly, don't pass everything down
- **Derived data**: Use Zustand selectors, not derived state in components

## SpacetimeDB Patterns (Cloud with Real-time Sync)

```typescript
// Call a reducer (server function deployed to SpacetimeDB)
try {
  const item = db.add_item({
    url: url,
    title: metadata.title,
    description: metadata.description,
    thumbnail: metadata.image,
    domain: new URL(url).hostname,
    content_type: detectContentType(url, metadata),
    status: 'unwatched',
    tags: [],
    created_at: Date.now(),
    updated_at: Date.now(),
  })
} catch (err) {
  console.error('Failed to add item:', err)
}

// Subscribe to real-time updates from other clients
db.saved_items.subscribe("*")
```

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
