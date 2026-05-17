# Design: WhatsApp Pre-filled Message & Hamburger Menu

**Date:** 2026-05-17  
**File:** `studio-zarq-novo.html`  
**Status:** Approved

---

## Overview

Two features from the backlog for the Studio ZARQ & Construções landing page:

1. All "orçamento" WhatsApp buttons open with a pre-filled message
2. The site becomes fully responsive on mobile with a hamburger menu

---

## Feature 1 — WhatsApp Pre-filled Message

### What

Update both WhatsApp `href` links to include a URL-encoded `text` query parameter so that WhatsApp opens with the message pre-filled.

### Where

| Location | Line | Current href |
|---|---|---|
| Nav CTA button | ~504 | `https://wa.me/5511984493780` |
| Hero "Solicitar orçamento" button | ~531 | `https://wa.me/5511984493780` |

### New href

```
https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊
```

### How it works

WhatsApp's `wa.me` API reads the `text` query parameter and pre-fills the message input box when the user opens the chat. No JavaScript required.

---

## Feature 2 — Mobile Hamburger Menu

### Breakpoint

Applies at `max-width: 900px` (matches existing mobile breakpoint in the file).

### Desktop behavior (unchanged)

- Header shows logo left, nav links + Orçamento CTA button right — no changes.

### Mobile behavior

**Header:**
- Nav links hidden
- Orçamento CTA button hidden
- Hamburger icon (3 horizontal lines) appears top-right, styled in gold (`var(--gold)` / `#C9B96A`)

**Overlay (triggered on hamburger tap):**
- Full-screen, `position: fixed`, `z-index: 1000`
- Background: `var(--navy3)` (#1a2535) with slight opacity/blur for depth
- Fade-in animation (`opacity 0 → 1`, ~250ms)
- Content centered vertically and horizontally:
  - Nav links stacked vertically, large font (~28px), cream color (`var(--cream)`)
  - Spacing between links: ~32px
  - "Orçamento" CTA button below the links, styled same as desktop `.nav-cta`
- X close button top-right (same position as hamburger icon)

**JS behavior (~15 lines):**
- `toggleMenu()` function adds/removes `.is-open` class on the overlay element
- Hamburger click → `toggleMenu()`
- X button click → `toggleMenu()`
- Each nav link inside overlay → `toggleMenu()` on click (menu closes before scroll)
- When `.is-open` is active → `body` gets `overflow: hidden` to lock scroll

### Hamburger icon

Pure CSS (3 `<span>` elements stacked), no SVG dependency. Can animate to X on open (optional, low priority).

---

## Out of Scope

- Changing any desktop layout
- Adding new nav links
- Modifying the WhatsApp phone number
- Any other buttons or CTAs not explicitly mentioned

---

## Success Criteria

- [ ] Both WhatsApp buttons open with "Olá! Gostaria de solicitar um orçamento 😊" pre-filled
- [ ] On mobile, hamburger icon appears top-right
- [ ] Tapping hamburger opens full-screen dark overlay with all nav links + Orçamento CTA
- [ ] Tapping any nav link closes the overlay and scrolls to the section
- [ ] Tapping X closes the overlay
- [ ] Body scroll is locked while menu is open
- [ ] Desktop layout is completely unchanged
