# Countdown Banner — Design Spec

**Date:** 2026-04-17  
**File to modify:** `index.html`

---

## Context

Election day is 19 April 2026. Starting from 20:00 Sofia time, the site will host live public video feeds from polling stations for citizen monitoring of the count. A banner in the header needs to:

1. Build anticipation before the event with a live countdown
2. Switch to a clickable CTA when the event goes live
3. Persist as a permanent link to the video archive after midnight

---

## Design

### Placement

A green card inside `<header>`, below the `<h1>` title, with `margin-top: 1rem`. Same card shape and size across all three states for visual consistency.

### States

| Window | Content | Link |
|--------|---------|------|
| Now → Apr 19 20:00 EET | Countdown card with day/hour/min/sec digit blocks | None (not clickable) |
| Apr 19 20:00 → Apr 20 00:00 EET | `👁️‍🗨️ Гледай броенето на живо!` + subdomain | `https://video.izborenmonitor.com` |
| Apr 20 00:00 EET onward | `📹 Изборни Камери` + subdomain | `https://video.izborenmonitor.com` |

### Countdown card copy

- Headline (uppercase, small): `⏳ ГЛЕДАЙ БРОЕНЕТО НА ЖИВО — СТАРТ СЛЕД`
- Digit blocks: day / hours / min / sec with Cyrillic labels (ден / часа / мин / сек)
- Sub-line: `19 Апр 2026 · 20:00 ч. (София) · video.izborenmonitor.com`

### Visual style

```css
background: linear-gradient(135deg, #00704f, #00966E);
border: 1px solid rgba(255,255,255,0.15);
border-radius: 10px;
margin: 1rem 1.5rem 1rem;
padding: 0.85rem 1.5rem;
color: #fff;
```

Digit boxes: `background: rgba(0,0,0,0.15); border-radius: 6px`

---

## Implementation

- Pure inline JS + CSS in `index.html`, no dependencies
- Target datetime: `new Date('2026-04-19T20:00:00+03:00')` (EEST = UTC+3, Sofia summer time)
- Live end: `new Date('2026-04-20T00:00:00+03:00')`
- `setInterval` ticking every 1000ms, updates digit spans in place
- Banner HTML injected once; JS swaps content/wrapper on state transitions
- Single `<div id="event-banner">` placeholder in the header; JS renders into it on `DOMContentLoaded`

---

## Verification

1. Open `index.html` locally; confirm countdown renders and ticks
2. Temporarily set target to `Date.now() + 3000` to test live-state transition
3. Temporarily set live-end to `Date.now() + 3000` to test after-midnight state
4. Check on mobile viewport that card doesn't overflow
