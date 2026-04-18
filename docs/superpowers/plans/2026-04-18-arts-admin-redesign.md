# Arts Admin Page Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign `Arts-Admin-Experience2.html` into an editorial CV-hybrid page with hero, Currently/Upcoming callout, metrics strip, restructured experience cards (adds IAPV card + text-forward panels), skills grid, and condensed profile.

**Architecture:** Single-file static HTML edit. All CSS stays inline in the existing `<style>` block. No build step, no framework. Content is hardcoded. Existing animations (IntersectionObserver + `.fade-in`) and carousels are reused unchanged.

**Tech Stack:** HTML5, vanilla CSS (CSS variables, Grid, Flexbox), vanilla JS (already in file). Hosted on GitHub Pages (case-sensitive Linux filesystem — file paths must match exactly).

**Testing model:** No unit-test framework exists. "Verification" = open the file in a browser, visually confirm the section renders, confirm no console errors, confirm existing carousels still function. Each task ends with a manual browser-check step and a commit.

---

## File Structure

**Modified:**
- `Arts-Admin-Experience2.html` — the entire redesign. CSS additions go inside the existing `<style>` block; HTML rewrites replace sections inside `<body>`.

**No new files.** No separate CSS/JS extraction — the other site pages (`index.html`, `Musical-experience3.html`) also keep CSS inline, so we follow that established pattern.

---

## Task 1: Add CSS foundation — chips, section rhythm, new component styles

**Files:**
- Modify: `Arts-Admin-Experience2.html` (inside `<style>` block, before the `/* === Animations === */` comment)

**Why first:** Every subsequent section references these styles. Adding them up front avoids repetition and lets later tasks focus on HTML.

- [ ] **Step 1.1: Open the file and locate the style block**

Open `Arts-Admin-Experience2.html`. The `<style>` block runs from ~line 9 to ~line 362. Find the comment `/* === Animations === */` (~line 324) — insert the new CSS immediately **above** that comment.

- [ ] **Step 1.2: Add new CSS variables for accents**

Find the `:root` block at the top of `<style>`. Add two new variables at the end of the block (before the closing `}`):

```css
            --accent-blue-strong: #334155;
            --accent-soft: #e0f2fe;
```

The final `:root` should look like:

```css
        :root {
            --bg-color: #f8fafc;
            --bg-light-blue: #f0f9ff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --accent-blue: #475569;
            --border-color: #e2e8f0;
            --divider-light: #f1f5f9;
            --white: #ffffff;
            --accent-blue-strong: #334155;
            --accent-soft: #e0f2fe;
        }
```

- [ ] **Step 1.3: Append new component styles above `/* === Animations === */`**

Paste this block immediately above the `/* === Animations === */` comment:

```css
        /* === Section rhythm === */
        .section {
            padding: 4rem 0;
        }
        .section-eyebrow {
            font-size: 0.75rem;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--text-muted);
            margin-bottom: 0.75rem;
            display: block;
            text-align: center;
        }
        .section-title {
            font-size: 2rem;
            text-align: center;
            margin: 0 0 2.5rem 0;
        }

        /* === Chips / badges === */
        .chip {
            display: inline-block;
            font-size: 0.7rem;
            letter-spacing: 1.5px;
            text-transform: uppercase;
            padding: 4px 10px;
            border: 1px solid var(--border-color);
            border-radius: 999px;
            color: var(--text-muted);
            margin-right: 0.4rem;
            margin-bottom: 0.4rem;
            background-color: var(--white);
        }
        .chip-accent {
            background-color: var(--accent-soft);
            border-color: var(--accent-soft);
            color: var(--accent-blue-strong);
        }
        .chip-strong {
            background-color: var(--accent-blue-strong);
            border-color: var(--accent-blue-strong);
            color: var(--white);
        }
        .chip-row {
            margin-bottom: 1rem;
        }

        /* === Hero refinements === */
        .hero-tagline {
            font-family: 'Playfair Display', serif;
            font-style: italic;
            font-size: 1.25rem;
            color: var(--text-muted);
            max-width: 720px;
            margin: 0 auto;
            line-height: 1.6;
        }

        /* === Currently & Upcoming callout === */
        .current-upcoming-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
            margin-top: 1rem;
        }
        @media (min-width: 720px) {
            .current-upcoming-grid {
                grid-template-columns: 1fr 1fr;
            }
        }
        .cu-card {
            background-color: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 2rem;
            box-shadow: 0 10px 30px -10px rgba(0,0,0,0.05);
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }
        .cu-card.upcoming {
            border-left: 3px solid var(--accent-blue-strong);
        }
        .cu-card h4 {
            font-size: 1.35rem;
            margin: 0.25rem 0 0 0;
        }
        .cu-card .cu-org {
            font-size: 0.95rem;
            color: var(--text-main);
            font-weight: 600;
        }
        .cu-card .cu-date {
            font-size: 0.8rem;
            color: var(--text-muted);
            letter-spacing: 0.5px;
        }
        .cu-card .cu-body {
            margin-top: 0.75rem;
            color: #334155;
            line-height: 1.7;
            font-size: 0.95rem;
        }

        /* === Impact at a Glance metrics === */
        .stats-strip {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1.5rem;
            padding: 2.5rem 0;
            border-top: 1px solid var(--border-color);
            border-bottom: 1px solid var(--border-color);
        }
        @media (min-width: 720px) {
            .stats-strip {
                grid-template-columns: repeat(4, 1fr);
            }
        }
        .stat {
            text-align: center;
        }
        .stat-number {
            font-family: 'Playfair Display', serif;
            font-size: 2.75rem;
            color: var(--text-main);
            line-height: 1;
            margin-bottom: 0.5rem;
        }
        .stat-label {
            font-size: 0.8rem;
            color: var(--text-muted);
            letter-spacing: 0.5px;
            line-height: 1.4;
        }

        /* === Text-forward card panel (replacement for stock photo) === */
        .card-image.text-panel {
            background-color: var(--bg-light-blue);
            background-image: linear-gradient(135deg, #1e293b 0%, #475569 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 2rem;
            min-height: 250px;
        }
        .card-image.text-panel .monogram {
            font-family: 'Playfair Display', serif;
            font-size: 4rem;
            color: var(--white);
            letter-spacing: 2px;
            line-height: 1.1;
        }
        .card-image.text-panel .monogram small {
            display: block;
            font-size: 0.75rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            color: #cbd5e1;
            margin-top: 0.75rem;
            font-weight: 400;
            font-family: 'Inter', sans-serif;
        }

        /* === Skills & Capabilities grid === */
        .skills-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
        }
        @media (min-width: 720px) {
            .skills-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        @media (min-width: 1000px) {
            .skills-grid {
                grid-template-columns: repeat(4, 1fr);
            }
        }
        .skill-block {
            background-color: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 1.75rem;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .skill-block:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px -10px rgba(0,0,0,0.08);
        }
        .skill-block .skill-icon {
            font-size: 1.4rem;
            margin-bottom: 0.75rem;
            display: block;
        }
        .skill-block h4 {
            font-size: 1.1rem;
            margin: 0 0 0.75rem 0;
        }
        .skill-block ul {
            list-style: none;
            padding: 0;
            margin: 0;
            color: #334155;
            font-size: 0.9rem;
            line-height: 1.9;
        }

        /* === Profile section === */
        .profile-body {
            max-width: 680px;
            margin: 0 auto;
            color: var(--text-muted);
            line-height: 1.85;
            font-size: 1rem;
        }
        .profile-body p {
            margin-bottom: 1.2rem;
        }
        .profile-body p:last-child {
            margin-bottom: 0;
        }
```

- [ ] **Step 1.4: Save and open the file in a browser**

Run:
```bash
open /Users/nimaaref/marcus/Marcus/Arts-Admin-Experience2.html
```

Expected: Page renders identically to before (no HTML changed yet). Open browser devtools console — should be zero errors/warnings.

- [ ] **Step 1.5: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add CSS foundation for redesign — chips, stats, skill blocks, text-forward panels"
```

---

## Task 2: Rewrite the Hero section

**Files:**
- Modify: `Arts-Admin-Experience2.html` — the `<header class="portfolio-header">` block (currently ~lines 387–395)

**What changes:** Replace the wall-of-text bio with an eyebrow label, the h1, and a single italic tagline. The full bio content moves to Task 7 (Profile).

- [ ] **Step 2.1: Locate the current header**

Find this block (around line 387):

```html
    <header class="portfolio-header">
        <div class="container">
            <h1>Arts Administration</h1>
            <div class="bio-text">
                <p>Marcus is an upcoming Arts Administrator...</p>
                <p>Marcus has recently accepted a new role...</p>
            </div>
        </div>
    </header>
```

- [ ] **Step 2.2: Replace with the new hero**

Replace the entire `<header class="portfolio-header">...</header>` block with:

```html
    <header class="portfolio-header">
        <div class="container">
            <span class="section-eyebrow">Portfolio</span>
            <h1>Arts Administration</h1>
            <p class="hero-tagline">Orchestra operations, arts marketing, and program development across international festivals, chamber orchestras, and music education initiatives.</p>
        </div>
    </header>
```

- [ ] **Step 2.3: Browser-verify**

Reload the file in the browser. Expected:
- "PORTFOLIO" eyebrow above the title
- Title unchanged
- Single italic tagline, centered, narrower than the old bio
- No large bio block (the bio will reappear at the bottom in Task 7)

- [ ] **Step 2.4: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): replace bio wall with hero tagline"
```

---

## Task 3: Add Currently & Upcoming callout section

**Files:**
- Modify: `Arts-Admin-Experience2.html` — insert a new `<section>` between the header (ends ~line 395) and the existing `<section class="portfolio-section">` (~line 397).

- [ ] **Step 3.1: Insert the new section**

After the closing `</header>` and before `<section class="portfolio-section">`, add:

```html
    <section class="section">
        <div class="container">
            <span class="section-eyebrow">Currently &amp; Upcoming</span>
            <div class="current-upcoming-grid">
                <div class="cu-card fade-in" data-delay="0">
                    <span class="chip chip-accent">Current</span>
                    <div class="cu-date">2025 – Present · Cincinnati, OH</div>
                    <h4>Arts Administration Intern</h4>
                    <div class="cu-org">Summermusik (Cincinnati Chamber Orchestra)</div>
                    <p class="cu-body">Managing social media across all platforms, developing marketing strategies, and contributing to festival production, grant writing, and donor relations.</p>
                </div>
                <div class="cu-card upcoming fade-in" data-delay="150">
                    <span class="chip chip-strong">Upcoming · Summer 2026</span>
                    <div class="cu-date">Summer 2026 · Santa Fe, NM</div>
                    <h4>Arts Administration Team</h4>
                    <div class="cu-org">Santa Fe Chamber Music Festival</div>
                    <p class="cu-body">Joining the Arts Administration team for the 2026 summer season — a new engagement that builds on experience from Prague Summer Nights and Summermusik.</p>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 3.2: Browser-verify**

Reload. Expected:
- Two cards side-by-side on desktop, stacked on mobile
- Left card: "CURRENT" pill chip, Summermusik role
- Right card: dark "UPCOMING · SUMMER 2026" pill chip, Santa Fe role, left accent border
- Fade-in animation triggers on scroll

- [ ] **Step 3.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add Currently & Upcoming callout with Santa Fe"
```

---

## Task 4: Add Impact at a Glance metrics strip

**Files:**
- Modify: `Arts-Admin-Experience2.html` — insert after the Currently/Upcoming section from Task 3, before `<section class="portfolio-section">`.

- [ ] **Step 4.1: Insert the stats strip**

After the closing `</section>` of the Currently & Upcoming block and before `<section class="portfolio-section">`, add:

```html
    <section class="section" style="padding-top: 0;">
        <div class="container">
            <div class="stats-strip">
                <div class="stat fade-in" data-delay="0">
                    <div class="stat-number">800+</div>
                    <div class="stat-label">Audience Members<br>Prague Festival</div>
                </div>
                <div class="stat fade-in" data-delay="100">
                    <div class="stat-number">150+</div>
                    <div class="stat-label">Students Served<br>Try It Day</div>
                </div>
                <div class="stat fade-in" data-delay="200">
                    <div class="stat-number">3</div>
                    <div class="stat-label">Czech Cities<br>Stage-Managed</div>
                </div>
                <div class="stat fade-in" data-delay="300">
                    <div class="stat-number">4</div>
                    <div class="stat-label">Opera Productions<br>Vienna</div>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 4.2: Browser-verify**

Reload. Expected:
- 4 stat tiles in a horizontal row on desktop, 2×2 on mobile (≤720px)
- Large Playfair numbers, small muted labels beneath
- Horizontal border lines above and below the strip
- Each tile fades in on scroll, staggered

- [ ] **Step 4.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add Impact at a Glance metrics strip"
```

---

## Task 5a: Add "Selected Experience" section heading and restructure Prague card

**Files:**
- Modify: `Arts-Admin-Experience2.html` — the `<section class="portfolio-section">` block (~line 397) and its Prague card (~lines 401–408).

- [ ] **Step 5a.1: Add a section heading above the grid**

Find `<section class="portfolio-section">` and its inner `<div class="container">` and `<div class="grid">`. Insert a heading between the container and grid:

```html
    <section class="portfolio-section">
        <div class="container">
            <h2 class="section-title">Selected Experience</h2>
            <div class="grid">
```

(The existing `</div></div></section>` closers stay unchanged.)

- [ ] **Step 5a.2: Replace the Prague card**

Find the first `<div class="portfolio-card fade-in" data-delay="0">` (Prague). Replace the entire card with:

```html
                <div class="portfolio-card fade-in" data-delay="0">
                    <div class="card-image text-panel">
                        <div class="monogram">
                            FPSN
                            <small>Prague Summer Nights</small>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="chip-row">
                            <span class="chip">Summer 2025</span>
                            <span class="chip">Czechia + Austria</span>
                            <span class="chip">Operations</span>
                        </div>
                        <h4>Festival Prague Summer Nights</h4>
                        <span>Orchestra Operations Administrator</span>
                        <p>As an Orchestra Administrator, his role consisted of several responsibilities ranging from stage management, scheduling, and coordinating concerts in the Czech Republic. Marcus stage managed in three different cities in the Czech Republic: Tabor, Plzen, and Prague. These concerts had distinguished guests such as world renown conductor Marin Alsop, the Dallas Symphony Chorus, and principal clarinetist of the Philadelphia Orchestra Ricardo Morales. The festival's concerts had an attendance of over 800 audience members. In addition, Marcus was responsible for stage managing the orchestra for 4 opera productions of Magic Flute and Figaro in Vienna, Austria.</p>
                    </div>
                </div>
```

- [ ] **Step 5a.3: Browser-verify**

Reload. Expected:
- "Selected Experience" heading above the cards
- Prague card's left panel is now a slate-gradient text panel with "FPSN" monogram and "Prague Summer Nights" subtitle (no more stock Unsplash photo)
- Three chips at the top of the right content: "SUMMER 2025", "CZECHIA + AUSTRIA", "OPERATIONS"
- Role title and body copy unchanged

- [ ] **Step 5a.4: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): restructure Prague card with text-panel and meta chips"
```

---

## Task 5b: Restructure Summermusik card

**Files:**
- Modify: `Arts-Admin-Experience2.html` — the Summermusik `<div class="portfolio-card fade-in" data-delay="150">` (~lines 410–448).

- [ ] **Step 5b.1: Replace the Summermusik card**

Replace the entire Summermusik card with:

```html
                <div class="portfolio-card fade-in" data-delay="150">
                    <div class="card-image text-panel">
                        <div class="monogram">
                            SM
                            <small>Summermusik · Cincinnati</small>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="chip-row">
                            <span class="chip">2025 – Present</span>
                            <span class="chip">Cincinnati, OH</span>
                            <span class="chip">Marketing · Development</span>
                        </div>
                        <h4>Summermusik (Cincinnati Chamber Orchestra)</h4>
                        <span>Arts Administration Intern</span>
                        <p>As an Arts Administration Intern, his role is to manage all social media accounts and develop and execute marketing strategies. In addition to marketing, Marcus also has experience in Festival production and planning for We Are One: Roots and development work in grant writing and donor relations.</p>

                        <div class="social-media-work">
                            <h5>Featured Social Media Content</h5>
                            <div class="video-carousel-container">
                                <button class="carousel-btn prev-btn" onclick="changeSlide(-1)">&#10094;</button>

                                <div class="carousel-slides">
                                    <div class="video-slide active">
                                        <div class="media-item">
                                            <video controls>
                                                <source src="Artsville Venue Highlight Reel(2).mp4" type="video/mp4">
                                                Your browser does not support the video tag.
                                            </video>
                                            <p>Artsville Venue Highlight</p>
                                        </div>
                                    </div>
                                    <div class="video-slide">
                                        <div class="media-item">
                                            <video controls>
                                                <source src="Pass The Phone Reel (1).mp4" type="video/mp4">
                                                Your browser does not support the video tag.
                                            </video>
                                            <p>Pass The Phone Challenge</p>
                                        </div>
                                    </div>
                                </div>

                                <button class="carousel-btn next-btn" onclick="changeSlide(1)">&#10095;</button>
                            </div>
                        </div>

                    </div>
                </div>
```

- [ ] **Step 5b.2: Browser-verify**

Reload. Expected:
- Summermusik card's left panel shows "SM" monogram and "Summermusik · Cincinnati" subtitle (no stock photo)
- Three chips: "2025 – PRESENT", "CINCINNATI, OH", "MARKETING · DEVELOPMENT"
- Video carousel still works (click prev/next, videos play)

- [ ] **Step 5b.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): restructure Summermusik card with text-panel and meta chips"
```

---

## Task 5c: Restructure Try It Day card (keep real photos, add chips)

**Files:**
- Modify: `Arts-Admin-Experience2.html` — the Try It Day `<div class="portfolio-card fade-in" data-delay="300">` (~lines 450–488).

- [ ] **Step 5c.1: Add meta chips to the Try It Day card**

Inside `<div class="card-content">`, immediately after the opening tag and before `<h4>Instrument Try It Day Event</h4>`, insert a chip-row. The full card replacement:

```html
                <div class="portfolio-card fade-in" data-delay="300">
                    <div class="card-image" style="background-image: url('Marshall%20Elementary%20Instrument%20Try%20It%20Day%20Photo%201.jpg');"></div>
                    <div class="card-content">
                        <div class="chip-row">
                            <span class="chip">Marshall Elementary</span>
                            <span class="chip">Project Management</span>
                        </div>
                        <h4>Instrument Try It Day Event</h4>
                        <span>Project Manager</span>
                        <p>Marcus project managed an Instrument Try It Day Fair for over 150 students at Marshall Elementary. He collaborated with Marshall Elementary, Miami Music Department, Willis Music, and Ohio Collegiate Music Educators Association (OCMEA) to create this event. He organized the schedule for the event and the logistics coordinating with all parties involved. More specifically his role was focused on coordinating with Willis Music to get the instruments for this event. Additionally, he was heavily involved in coordinating with the president of OCMEA to get music education volunteers to help run the instrument stations for the event.</p>

                        <div class="social-media-work">
                            <h5>Event Gallery</h5>
                            <div class="image-carousel-container">
                                <button class="carousel-btn prev-btn" onclick="changeImageSlide(-1)">&#10094;</button>

                                <div class="carousel-slides">
                                    <div class="image-slide active">
                                        <div class="media-item">
                                            <img src="Marshall Elementary Instrument Try It Day Photo 1.jpg" alt="String Instruments Demonstration">
                                            <p>String Instruments Demonstration</p>
                                        </div>
                                    </div>
                                    <div class="image-slide">
                                        <div class="media-item">
                                            <img src="Marshall Elementary Instrument Try It Day Photo 2.jpg" alt="Instrument Stations in Action">
                                            <p>Instrument Stations in Action</p>
                                        </div>
                                    </div>
                                    <div class="image-slide">
                                        <div class="media-item">
                                            <img src="Marshall Elementary Instrument Try It Day Photo 3 (1).jpg" alt="Students Exploring New Instruments">
                                            <p>Students Exploring New Instruments</p>
                                        </div>
                                    </div>
                                </div>

                                <button class="carousel-btn next-btn" onclick="changeImageSlide(1)">&#10095;</button>
                            </div>
                        </div>

                    </div>
                </div>
```

- [ ] **Step 5c.2: Browser-verify**

Reload. Expected:
- Try It Day card keeps its real Marshall Elementary photo on the left
- Two chips appear above the title: "MARSHALL ELEMENTARY", "PROJECT MANAGEMENT"
- Image carousel still works (prev/next cycles through 3 photos)

- [ ] **Step 5c.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add meta chips to Try It Day card"
```

---

## Task 5d: Add new IAPV (Institute for Arts Public Value) card

**Files:**
- Modify: `Arts-Admin-Experience2.html` — insert a 4th card inside the `<div class="grid">` after the Try It Day card, before the closing `</div>` of the grid.

- [ ] **Step 5d.1: Add the IAPV card**

After the closing `</div>` of the Try It Day `portfolio-card` and before the closing `</div>` of `grid`, insert:

```html
                <div class="portfolio-card fade-in" data-delay="450">
                    <div class="card-image text-panel">
                        <div class="monogram">
                            IAPV
                            <small>Institute for Arts Public Value</small>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="chip-row">
                            <span class="chip">Internship · Under Angela Meleca</span>
                            <span class="chip">Research · Data</span>
                        </div>
                        <h4>Institute for Arts Public Value</h4>
                        <span>Research &amp; Data Intern</span>
                        <p>Contributed to a pilot project under Angela Meleca's direction, focused on understanding the public value of the arts. Marcus developed skills in survey design and data analysis, supporting research that informs how arts organizations measure and communicate their community impact.</p>
                    </div>
                </div>
```

- [ ] **Step 5d.2: Browser-verify**

Reload. Expected:
- A fourth card appears in Selected Experience with "IAPV" monogram panel
- Two chips: "INTERNSHIP · UNDER ANGELA MELECA", "RESEARCH · DATA"
- Body copy describes the pilot project work

- [ ] **Step 5d.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add Institute for Arts Public Value card"
```

---

## Task 6: Add Skills & Capabilities section

**Files:**
- Modify: `Arts-Admin-Experience2.html` — insert a new `<section>` immediately after the closing `</section>` of `portfolio-section`, before `<footer>`.

- [ ] **Step 6.1: Insert the Skills section**

After the closing `</section>` of Selected Experience and before `<footer>`, add:

```html
    <section class="section" style="background-color: var(--bg-light-blue);">
        <div class="container">
            <h2 class="section-title">Skills &amp; Capabilities</h2>
            <div class="skills-grid">
                <div class="skill-block fade-in" data-delay="0">
                    <span class="skill-icon">◆</span>
                    <h4>Operations &amp; Production</h4>
                    <ul>
                        <li>Stage management</li>
                        <li>Scheduling &amp; logistics</li>
                        <li>Music librarianship</li>
                        <li>Personnel management</li>
                    </ul>
                </div>
                <div class="skill-block fade-in" data-delay="100">
                    <span class="skill-icon">◆</span>
                    <h4>Marketing &amp; Social</h4>
                    <ul>
                        <li>Multi-platform social media</li>
                        <li>Video &amp; reel production</li>
                        <li>Marketing strategy</li>
                        <li>Content planning</li>
                    </ul>
                </div>
                <div class="skill-block fade-in" data-delay="200">
                    <span class="skill-icon">◆</span>
                    <h4>Development</h4>
                    <ul>
                        <li>Grant writing</li>
                        <li>Grant research</li>
                        <li>Donor relations</li>
                        <li>Artist liaison</li>
                    </ul>
                </div>
                <div class="skill-block fade-in" data-delay="300">
                    <span class="skill-icon">◆</span>
                    <h4>Research &amp; Data</h4>
                    <ul>
                        <li>Survey development</li>
                        <li>Data analysis</li>
                        <li>Data management</li>
                        <li>Public-value research</li>
                    </ul>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 6.2: Browser-verify**

Reload. Expected:
- New section with light-blue background below the experience cards
- "Skills & Capabilities" heading centered
- 4 skill blocks in a row on desktop (≥1000px), 2×2 on tablet, stacked on mobile
- Each block: diamond glyph, heading, bullet list
- Blocks lift slightly on hover
- Fade-in animation staggers

- [ ] **Step 6.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add Skills & Capabilities grid"
```

---

## Task 7: Add condensed Profile section

**Files:**
- Modify: `Arts-Admin-Experience2.html` — insert a new `<section>` immediately after the Skills section and before `<footer>`.

- [ ] **Step 7.1: Insert the Profile section**

After the closing `</section>` of Skills & Capabilities and before `<footer>`, add:

```html
    <section class="section">
        <div class="container">
            <h2 class="section-title">Profile</h2>
            <div class="profile-body fade-in" data-delay="0">
                <p>Marcus is an upcoming Arts Administrator gaining a broad range of skills and experiences during his collegiate career. His deepest experience is in orchestra logistics and operations, earned as an Orchestra Operations Administrator for Prague Summer Nights — an international music festival running across the Czech Republic and Austria.</p>
                <p>Alongside operations, he has developed capabilities in scheduling, music librarianship, and personnel management during his time in the Czech Republic, and in data analysis and survey development as an intern for the Institute for Arts Public Value pilot project under Angela Meleca.</p>
                <p>He currently works as an Arts Administration Intern for Summermusik (Cincinnati Chamber Orchestra), where he manages social media across all platforms and contributes to grant writing, research, data management, and artist liaison work.</p>
            </div>
        </div>
    </section>
```

- [ ] **Step 7.2: Browser-verify**

Reload. Expected:
- "Profile" section below Skills
- Three shorter paragraphs in a narrow centered reading column
- Typography is lighter/muted compared to card body text
- Santa Fe is NOT mentioned here (it's already elevated in the Currently/Upcoming section — no duplication)

- [ ] **Step 7.3: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat(arts-admin): add condensed Profile section"
```

---

## Task 8: Full-page verification and polish

**Files:**
- `Arts-Admin-Experience2.html` (verification only; may include minor style tweaks)

- [ ] **Step 8.1: Full scroll-through check**

Open the page and scroll from top to bottom. Verify each section is present in this order:
1. Nav (unchanged)
2. Hero (eyebrow + title + italic tagline)
3. Currently & Upcoming (2 cards)
4. Impact at a Glance (4 stats)
5. Selected Experience (Prague, Summermusik, Try It Day, IAPV — 4 cards)
6. Skills & Capabilities (4 blocks on light-blue background)
7. Profile (condensed bio)
8. Footer (unchanged)

- [ ] **Step 8.2: Interaction checks**

Verify:
- Nav dropdown ("Experience ▼") opens and shows both sub-pages
- Summermusik video carousel: both videos play; prev/next cycles between them
- Try It Day image carousel: prev/next cycles through 3 photos
- All `fade-in` elements animate on scroll (scroll slowly from the top)
- Page has zero console errors/warnings (check browser devtools)

- [ ] **Step 8.3: Responsive spot-check**

Resize the browser to:
- ~1200px (desktop): 4-column stats and 4-column skills, side-by-side CU cards
- ~800px (tablet): 2×2 stats, 2×2 skills, side-by-side CU cards
- ~500px (mobile): stacked stats, stacked skills, stacked CU cards, stacked experience cards

Verify no horizontal scroll at any width, no text overflow, no broken layouts.

- [ ] **Step 8.4: Case-sensitivity check (GitHub Pages compatibility)**

Run:
```bash
grep -oE 'src="[^"]+\.(jpg|JPG|jpeg|png|mp4)"|url\([^)]+\.(jpg|JPG|jpeg|png|mp4)\)' /Users/nimaaref/marcus/Marcus/Arts-Admin-Experience2.html
```

For each referenced file, confirm an exactly-matching file exists:
```bash
ls -1 /Users/nimaaref/marcus/Marcus/ | grep -E '\.(jpg|JPG|jpeg|png|mp4)$'
```

Cross-check case. If any reference mismatches a real file's case, fix the HTML reference to match the file (not the other way around).

- [ ] **Step 8.5: If any polish fixes were made, commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "fix(arts-admin): polish and verification fixes from full-page review"
```

(If Step 8.4 found no mismatches and no other polish was needed, skip this commit.)

- [ ] **Step 8.6: Push the series of commits**

```bash
git push
```

Wait ~1 minute for GitHub Pages to rebuild, then load the live site and re-run the scroll-through and interaction checks against the hosted URL.

---

## Done Criteria

- All 8 tasks committed
- Visual scroll-through passes on local and hosted site
- Nav, carousels, and fade-in animations all work
- No console errors
- No case-mismatched image references
- Santa Fe Chamber Music Festival is visible in the top portion of the page (not buried in bio)
- Key metrics (800+, 150+, 3, 4) are visible as a stats strip
- IAPV has its own card in Selected Experience
- Prague and Summermusik use text-forward panels (no more generic Unsplash photos)
