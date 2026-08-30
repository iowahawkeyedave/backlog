# Backlog — Dependencies

Planning snapshot. Once `package.json` exists it is the source of truth; update or delete this file in Build Plan Phase 1. Caret ranges resolve to current versions at install time.

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "convex": "^1.17.0",
    "zustand": "^4.5.0",
    "date-fns": "^3.3.0",
    "lucide-react": "^0.400.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@vitejs/plugin-react": "^4.2.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.4.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "eslint": "^9.0.0",
    "typescript-eslint": "^8.0.0",
    "eslint-plugin-react-hooks": "^5.0.0",
    "eslint-plugin-react-refresh": "^0.4.0",
    "prettier": "^3.0.0",
    "prettier-plugin-tailwindcss": "^0.6.0",
    "vitest": "^3.0.0",
    "jsdom": "^25.0.0",
    "@testing-library/react": "^16.0.0",
    "@playwright/test": "^1.50.0"
  }
}
```

## Notes

- No `uuid` package: anonymous ids use the built-in `crypto.randomUUID()` (see TECH-SPEC.md, Identity).
- `lucide-react` is the icon set (per DESIGN.md); it's the only icon dependency allowed.
- Tooling enforced from Phase 1: ESLint + Prettier (AGENTS.md standards), Vitest (unit), Playwright (one smoke path).
