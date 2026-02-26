# Backlog — PRD

## Problem Statement

People constantly encounter interesting content online—videos, articles, tutorials—but have no reliable way to save it for later without losing track. Bookmarks get buried, browser extensions are clunky, and platform-specific "save for later" features (YouTube, etc.) only work within their ecosystem. Users need a unified, cross-platform place to save any URL and actually go back to watch/read it.

## Market Opportunity ⚡️

**The timing is perfect:** The read-later market just got wrecked.
- **Pocket** is shutting down July 8, 2025 (Mozilla)
- **Omnivore** died in November 2024 (acquired by ElevenLabs)

Millions of users are looking for alternatives right now. Most existing apps focus on articles only—we can own the "videos + articles" hybrid space.

## Target Audience

- **Primary**: Knowledge workers and learners who consume a mix of videos (YouTube, tutorials) and articles (blogs, docs)
- **Secondary**: Content curators, researchers, and anyone building a personal "consume later" library
- **Tertiary**: Developers saving coding tutorials and documentation

## What We Save (Content Types)

| Type | Examples | Metadata Needed |
|------|----------|-----------------|
| **Videos** | YouTube, Vimeo, TikTok | Title, thumbnail, duration, channel |
| **Articles** | Blog posts, news, docs | Title, description, thumbnail, reading time |
| **Mixed** | Pages with video + text | Best of both |

## User Stories

| As a... | I want to... | So that... |
|---------|--------------|------------|
| User | Save any URL (video or article) with one click | I don't lose content I want to consume later |
| User | See a thumbnail and title auto-fetched | I can quickly recognize what I saved |
| User | See content type indicated (video vs article) | I can pick what I'm in the mood for |
| User | Organize content into lists/tags | I can categorize by topic (e.g., "React Tutorials", "Recipes") |
| User | Mark items as watched/unwatched | I can track my progress |
| User | Archive completed items | They don't clutter my main list |
| User | Quick-add from mobile share or browser | I can save without opening the app |
| User | Search through my saved items | I can find specific content fast |

## Competitor Landscape

| App | Price | Strengths | Weaknesses |
|-----|-------|-----------|------------|
| **Instapaper** | Free/$3mo | Clean reader, simple | No tagging, article-only |
| **Matter** | Free/$10mo | Modern, audio narration | Newsletter-focused |
| **Wallabag** | Self-hosted | Open source, privacy | Need to host yourself |
| **Meco** | Free/$5mo | Newsletters only | Very niche |

**Our Edge:** Local-first (no account needed), video+article hybrid, fast/lightweight, SpacetimeDB for sync-ready architecture.

## MVP Features (Prioritized)

### P0 — Must Have
1. **Add URL** — Paste/save any URL (video or article), auto-fetch title + description + thumbnail
2. **Content Type Detection** — Auto-detect video vs article vs mixed
3. **View List** — See all saved items in a clean, scrollable feed
4. **Mark Watched/Unwatched** — Toggle completion status
5. **Delete Item** — Remove items from the list
6. **Basic Search** — Filter by title/URL

### P1 — Should Have
7. **Tags/Categories** — Add tags to items for organization
8. **Lists** — Group items into named lists (e.g., "YouTube", "Articles", "To Read")
9. **Archive** — Hide completed items from main view
10. **Sort Options** — Sort by date added, title, or status

### P2 — Nice to Have
11. **Browser Extension** — Quick-save button for Chrome/Firefox
12. **Mobile Share** — Add via iOS/Android share sheet
13. **Keyboard Shortcuts** — Quick actions (j/k navigation, 'x' to mark watched)
14. **Bulk Actions** — Select multiple items to delete/archive

## Success Metrics

- **DAU/MAU Ratio**: Target 40%+ (people come back regularly)
- **Items Saved per User**: Average 10+ items in first week
- **Completion Rate**: 30%+ items marked as "watched" within 7 days
- **Retention**: 50%+ users return after 30 days
