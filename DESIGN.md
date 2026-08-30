<!-- SEED: re-run /impeccable document once there's code to capture the actual tokens and components. -->

---
name: Backlog
description: A calm, fast queue for videos and articles you mean to finish
---

# Design System: Backlog

## Overview

**Creative North Star: "The Quiet Queue"**

Backlog looks like a tidy shelf in a bright room: a calm, near-neutral surface where colorful thumbnails are the only loud thing on screen. Density is moderate and predictable; the feed is the page. The system rejects the beige-paper nostalgia of heavyweight reader apps, the cramped urgency of extension popups, and dashboard chrome of any kind. It should feel like a best-in-class tool (Linear's restraint, Things 3's calm progress through a list, Notion's content-first quiet), not a consumer app begging for attention.

**Key Characteristics:**
- Light theme: capture and skim happen in lit, variable environments (a desk between meetings, a couch with a phone), and thumbnails carry the visual energy, so the chrome stays quiet.
- Restrained color: tinted neutrals plus one green accent, under 10% of any screen.
- System-native type with zero webfont payload.
- Motion is state feedback only, 150–250 ms, ease-out.
- Flat by default; depth is a hover/focus response, never a resting style.

## Colors

Strategy: **Restrained**. One hue family, "progress green" (a deep, slightly warm forest green), anchors the accent; every neutral is tinted toward that hue instead of pure gray. Exact values: [to be resolved during implementation, in OKLCH, with hex exports for Tailwind].

**The Restrained Rule.** Tinted neutrals carry every surface. The green accent appears only on primary actions (Add, Mark done), current selection, and focus. Its rarity is the point.

### Primary
- **Progress green** [to be resolved during implementation]: the Add button, done states, active list, and focus rings. Green is the brand color because the product's promise is a shrinking backlog; the accent IS completion.

### Secondary
- **Amber, indicators only** [to be resolved during implementation]: a small status dot on open items. A signal, never decoration.
- **Content-type chips** (video, article, mixed): tiny, low-chroma tinted chips in distinct hues. They identify; they don't decorate.

### Neutral
- **Page surface**: warm off-white (never #fff), tinted toward the accent hue.
- **Card surface**: one step lighter than the page, hairline border, no resting shadow.
- **Sidebar/panel**: a second, slightly deeper neutral layer so navigation reads as furniture.
- **Ink**: deep green-tinted near-black for text (never #000); secondary text around 60% of its lightness.

## Typography

**Direction: single sans, system-native.** The stack is the platform default (`-apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif`); no webfonts. This is a speed decision as much as an aesthetic one: zero font payload, native feel on every device. One family, weights 400/500/600, fixed rem scale (no fluid clamping), ratio ~1.125 between steps. Card titles run 0.875–1rem medium with two-line truncation; page headers 1.25–1.5rem semibold; labels and metadata 0.75rem. Prose is rare in this UI; the 65–75ch line-length rule applies only to descriptions.

**The One Family Rule.** No display fonts anywhere in the UI. Weight and size do all hierarchy work.

## Elevation

Flat by default. Depth is tonal (surface steps) plus hairline borders; shadows appear only as state responses (a subtle hover lift on cards, a focus ring on interaction). Audit test: if a resting card looks like it floats, the shadow is wrong; remove it.

## Components

[Omitted tokens: no components exist yet. The seed rules that will govern them: every interactive component ships with default, hover, focus, active, disabled, and loading states; loading feeds use skeletons, never spinners; empty states teach ("Paste a link to start your backlog") instead of just apologizing; the sidebar collapses into a drawer below 768px.]

## Do's and Don'ts

### Do:
- **Do** keep total accent coverage under 10% of any screen (The Restrained Rule).
- **Do** pair every status color with an icon or label; never color-only.
- **Do** use skeletons for loading and teaching copy for empty states.
- **Do** collapse the sidebar into a drawer below 768px and test at 375px.
- **Do** respect `prefers-reduced-motion` (drop transitions to opacity-only or none).

### Don't:
- **Don't** use border-left greater than 1px as a colored stripe on cards or list items.
- **Don't** ship beige-paper reader nostalgia, cramped popup-density UI, or dashboard stat widgets (PRODUCT.md anti-references).
- **Don't** animate layout properties or orchestrate page-load sequences; state changes only, 150–250 ms, ease-out.
- **Don't** introduce a second font family, a second accent hue, or gradient text.
- **Don't** nest cards inside cards; the feed is one level of surface.
