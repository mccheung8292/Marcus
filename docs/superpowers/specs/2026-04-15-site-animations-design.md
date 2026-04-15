# Site Animations Design
**Date:** 2026-04-15
**Scope:** All three pages — `index.html`, `Musical-experience3.html`, `Arts-Admin-Experience2.html`

## Approach
Intersection Observer API (vanilla JS) with CSS transitions. No libraries. All code added inline to each HTML file.

## Scroll Animations
- **Pattern:** `.fade-in` class sets `opacity: 0` and `transform: translateY(20px)`. Intersection Observer adds `.visible` class when element enters viewport, triggering a `0.6s ease` transition.
- **Stagger:** Grid children get incremental `transition-delay` (0ms, 100ms, 200ms...) so cards don't all pop in at once.

**Targeted elements per page:**
- `index.html`: about text block, headshot image, each `.booking-card`, contact section content
- `Musical-experience3.html`: biography block, each `.portfolio-card`, each `.gallery-item`
- `Arts-Admin-Experience2.html`: each `.portfolio-card`

## Hover Effects
- **Nav links:** CSS `::after` pseudo-element underline slides in from left on hover (`width: 0 → 100%`, `0.3s ease`).
- **Hero button (`index.html`):** Adds `transform: scale(1.03)` on hover, alongside existing background fill transition.
- **Cards & gallery:** Existing hover transforms kept unchanged.

## Page Load Animations
- `index.html` hero `<h1>` and `<p>` subtitle fade in on load (`opacity: 0 → 1`, `0.8s ease`, staggered).
- Sub-page `<h1>` elements (`.portfolio-header h1`) fade in on load.

## Implementation Notes
- All CSS added to each page's existing `<style>` block.
- Intersection Observer JS added to each page's `<script>` block (or new `<script>` tag before `</body>`).
- The same ~15-line Observer snippet is used across all three pages.
- No new files created.
