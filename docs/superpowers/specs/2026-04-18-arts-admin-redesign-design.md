---
title: Arts Administration page redesign
date: 2026-04-18
status: approved
---

# Arts Administration Page Redesign

Redesign `Arts-Admin-Experience2.html` into a more classy, professional, better-structured editorial-style portfolio page. Keep consistency with the site's existing typography (Playfair Display + Inter) and cool-slate palette already used in `index.html` and `Musical-experience3.html`.

## Goals

- Replace the dense bio paragraph with scannable, hierarchical structure
- Surface key metrics (audience counts, students served, cities, productions) that are currently buried in prose
- Make the **upcoming Santa Fe Chamber Music Festival** role visible and celebrated rather than hidden in paragraph two
- Add explicit **Skills & Capabilities** showcase grouped by competency
- Give the **Institute for Arts Public Value (Angela Meleca)** internship its own card
- Preserve existing nav, footer, dropdown behavior, scroll animations, and carousels

## Non-Goals

- Changing the navigation structure or other pages
- Replacing the color palette or fonts
- Adding interactivity beyond what already exists (dropdown, carousels, fade-ins)
- Building a CMS or data-driven content — content stays hardcoded in HTML

## Page Structure (top → bottom)

### 1. Navigation *(unchanged)*
Same nav with Experience dropdown. Arts Administration bold in dropdown.

### 2. Hero
- Eyebrow label: `PORTFOLIO` (uppercase, tracked, muted)
- `h1`: "Arts Administration" (Playfair, large)
- One-sentence positioning statement beneath title, ~18–22 words:
  > "Orchestra operations, arts marketing, and program development across international festivals, chamber orchestras, and music education initiatives."
- Horizontal divider rule or generous whitespace

### 3. Currently & Upcoming — paired callout
A two-column card row that tells the narrative "what I'm doing now / what's next."

**Left card — CURRENT**
- Small badge: `CURRENT` (muted-blue accent)
- Role: Arts Administration Intern
- Organization: Summermusik (Cincinnati Chamber Orchestra)
- Date: 2025 – Present
- One-line summary (marketing + social + festival production + development)

**Right card — UPCOMING**
- Small badge: `UPCOMING · SUMMER 2026` (accent-blue, slightly brighter)
- Role: Arts Administration Team
- Organization: Santa Fe Chamber Music Festival
- Date: Summer 2026
- One-line summary

Style: both cards share the same footprint; upcoming card has a subtle highlight treatment (accent left-border or slightly lifted shadow) so it reads as the headline.

### 4. Impact at a Glance — metrics strip
Horizontal row of 4 stat tiles, each with a large Playfair number and Inter label beneath:

| Number | Label |
|---|---|
| 800+ | Audience Members (Prague Festival) |
| 150+ | Students Served (Try It Day) |
| 3 | Czech Cities Stage-Managed |
| 4 | Opera Productions (Vienna) |

Each tile: centered, minimal divider between tiles on desktop, stacks 2×2 on mobile.

### 5. Selected Experience
Heading: `Selected Experience` (h2 Playfair).

Experience cards listed top-to-bottom in reverse-chronological order. Each card has the existing magazine split-layout but with added meta chips (date range, location, role-category tag).

**Card 5a — Festival Prague Summer Nights** (Orchestra Operations Administrator, Summer 2025)
- Meta chips: `SUMMER 2025` · `CZECHIA + AUSTRIA` · `OPERATIONS`
- **Text-forward left panel** (no stock photo). Large Playfair "FPSN" monogram or venue name treatment on a textured/gradient slate background.
- Body copy: existing Prague description.

**Card 5b — Summermusik (Cincinnati Chamber Orchestra)** (Arts Administration Intern, 2025 – Present)
- Meta chips: `2025–PRESENT` · `CINCINNATI, OH` · `MARKETING · DEVELOPMENT`
- **Text-forward left panel** (no stock photo). Typography-led treatment.
- Body copy: existing Summermusik description.
- Keep existing **Featured Social Media Content** video carousel inside the card.

**Card 5c — Instrument Try It Day Event** (Project Manager)
- Meta chips: `MARSHALL ELEMENTARY` · `PROJECT MANAGEMENT` (date chip omitted — not in source; add if user provides)
- **Keep existing real photo** (`Marshall Elementary Instrument Try It Day Photo 1.jpg`) — these are authentic and work.
- Body copy: existing description.
- Keep existing **Event Gallery** image carousel.

**Card 5d — Institute for Arts Public Value** *(new)*
- Meta chips: `INTERNSHIP · UNDER ANGELA MELECA` · `RESEARCH · DATA` (date chip omitted — not in source; add if user provides)
- **Text-forward left panel** (no photo).
- Body copy drawn from current bio text: pilot project internship focused on survey development and data analysis.

### 6. Skills & Capabilities
Heading: `Skills & Capabilities` (h2).

4-column grid on desktop, 2×2 on tablet, stacked on mobile. Each column is a skill-group block:

**Operations & Production**
- Stage management
- Scheduling & logistics
- Music librarianship
- Personnel management

**Marketing & Social Media**
- Social media management (multi-platform)
- Content production (video/reels)
- Marketing strategy

**Development**
- Grant writing
- Grant research
- Donor relations
- Artist liaison

**Research & Data**
- Survey development
- Data analysis
- Data management

Each block: small accent icon (emoji OR simple unicode glyph, no external icon font) + heading + short bulleted list. Clean borders, subtle hover lift.

### 7. Profile
Heading: `Profile` (h2).

Condensed bio (the current wall-of-text bio, cut into ~3 shorter paragraphs) presented in a narrower reading column (max-width ~650px) with generous line-height. Existing content stays — just broken up and tightened. The "Santa Fe" mention is removed here (already elevated in section 3).

### 8. Footer *(unchanged)*

## Visual / Styling Notes

- **Typography:** Playfair Display for h1–h4, stat numbers, monograms; Inter for body, labels, chips.
- **Palette:** Use existing CSS variables (`--bg-color`, `--bg-light-blue`, `--text-main`, `--text-muted`, `--accent-blue`, `--border-color`, `--divider-light`, `--white`). No new colors.
- **Chips/badges:** Uppercase, small, letter-spaced, bordered pills. Two variants: neutral (border + muted text) and accent (filled light blue).
- **Text-forward card panels:** Replace stock photo panels with a styled left panel — light slate/blue gradient background, large-scale Playfair typography as decoration (organization initials or abbreviated name). No external image.
- **Spacing:** Preserve the existing 3rem gaps between cards. Add section-level vertical rhythm (~5rem between sections).
- **Responsive breakpoints:** Keep the 900px magazine-split breakpoint. Stats strip collapses to 2×2 below ~700px. Skills grid collapses similarly.

## Animations
Reuse existing `fade-in` / IntersectionObserver pattern. Apply to:
- Each Currently/Upcoming card (staggered delays)
- Each stat tile (staggered)
- Each experience card (already in place)
- Each skills group block (staggered)
- Profile section

No new animation libraries.

## Out of Scope / Future

- Real photography for Prague and Summermusik (could replace text-forward panels later when photos are available)
- Separate dedicated case-study pages per engagement
- Testimonials section
- Downloadable resume/CV link (could be added to hero later)

## Acceptance

- Page renders correctly on desktop (≥900px) and mobile
- All existing carousels function
- All existing image paths resolve (case-sensitive — no `.jpg` vs `.JPG` mismatches)
- Scroll fade-in animations trigger for new sections
- Nav dropdown works unchanged
- No broken internal links
- Santa Fe role is visible without scrolling past three cards
- Marcus's key metrics are visible in the first screen-height on desktop
