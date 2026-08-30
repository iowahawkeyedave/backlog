# Product

## Register

product

## Users

Knowledge workers and learners who hit interesting videos and articles all day but can't consume them in the moment. They encounter a link while doing something else (working, scrolling, debugging); the job is to capture it in two seconds, then actually come back and finish it. For the MVP the primary user is the builder themself: one person using the app anonymously across laptop and phone. They are technical enough to paste a URL and learn keyboard shortcuts, and impatient enough to abandon anything clunky.

## Product Purpose

Backlog is a cross-platform, account-free place to save any URL (video or article) and consume it later. It auto-fetches title, description, and thumbnail, detects content type, and keeps everything in one realtime feed with search, lists, and an open/done status. Success per the PRD: 10+ items saved per user in the first week, 30%+ of items marked done within 7 days, 50%+ of users returning at 30 days. The market window is the read-later shakeout: Pocket shut down in July 2025 and Omnivore died in November 2024, and no incumbent owns the video+article hybrid.

## Brand Personality

Quick, calm, clear. Saving feels instant and frictionless; the feed feels like a tidy shelf, not a to-do nag. UI copy is sentence case, minimal, and encouraging ("Paste a link to start your backlog"), never guilt-inducing about unread piles. The app's edge, per the PRD: no-account onboarding, video+article hybrid, fast and lightweight.

## Anti-references

- Clunky browser extension popups: cramped save dialogs that ask five questions before saving.
- Heavyweight reader apps (Matter, Instapaper): beige-paper skeuomorphism, feature sprawl, article-only focus.
- Buried bookmark managers: folder trees, table sprawl, star-and-forget interfaces where saved things go to die.
- Pocket clones: red-branded read-later nostalgia. This is a fresh start, not a revival.
- Dashboard chrome: stats widgets, streaks, gamification. The queue is the interface.

## Design Principles

1. **Two seconds to saved.** Capture is the core promise; adding a URL is always one action away and never asks a question first.
2. **The content is the interface.** Thumbnails and titles carry the design; chrome recedes.
3. **Progress you can see.** The product's job is shrinking the backlog; open/done state is the primary visual vocabulary.
4. **Familiar, not novel.** Standard patterns (side nav, search field, filter tabs) behave as expected; no invented affordances.
5. **Quiet by default.** One accent, used sparingly; color means something or it isn't there.

## Accessibility & Inclusion

Target WCAG 2.1 AA as the default bar (no specific user requirements gathered yet). Visible focus rings on every interactive element; status is never color-only (icon + label); respect `prefers-reduced-motion`; thumbnails are always paired with a text title so nothing depends on imagery alone; 44px minimum touch targets on mobile.
