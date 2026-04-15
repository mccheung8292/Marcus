# Site Animations Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add subtle scroll-triggered fade-in animations, nav hover underlines, and hero/page-title entrance animations across all three HTML pages.

**Architecture:** Pure CSS transitions triggered by an Intersection Observer. Elements start invisible (`opacity: 0; transform: translateY(20px)`) and reveal when they enter the viewport via a `.visible` class added by JS. No external libraries. All code added inline to existing `<style>` and `<script>` blocks.

**Tech Stack:** HTML, CSS (keyframes, transitions, `::after` pseudo-elements), Vanilla JS (Intersection Observer API)

---

### Task 1: Animate index.html

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add animation CSS inside the existing `<style>` block, just before `</style>`**

```css
/* === Animations === */
@keyframes heroFadeIn {
    from { opacity: 0; transform: translateY(15px); }
    to   { opacity: 1; transform: translateY(0); }
}

.fade-in {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.visible {
    opacity: 1;
    transform: translateY(0);
}

/* Hero entrance on page load */
.hero h1  { animation: heroFadeIn 0.8s ease both; }
.hero p   { animation: heroFadeIn 0.8s ease 0.2s both; }
.hero .btn { animation: heroFadeIn 0.8s ease 0.4s both; }

/* Nav link underline slide-in */
.nav-links > a { position: relative; }
.nav-links > a::after {
    content: '';
    position: absolute;
    bottom: -3px;
    left: 0;
    width: 0;
    height: 1px;
    background-color: currentColor;
    transition: width 0.3s ease;
}
.nav-links > a:hover::after { width: 100%; }
```

- [ ] **Step 2: Add `transform: scale(1.03)` to the existing `.hero .btn:hover` rule**

Find:
```css
.hero .btn:hover {
    background-color: var(--white);
    color: var(--text-main);
}
```
Replace with:
```css
.hero .btn:hover {
    background-color: var(--white);
    color: var(--text-main);
    transform: scale(1.03);
}
```

- [ ] **Step 3: Add `.fade-in` classes and `data-delay` attributes to target elements**

1. `<div class="about-text">` → `<div class="about-text fade-in">`
2. The `<div>` wrapping the headshot `<img>` → `<div class="fade-in" data-delay="100">`
3. First `.booking-card` → add `fade-in data-delay="0"` to class/attributes
4. Second `.booking-card` → add `fade-in data-delay="100"`
5. Third `.booking-card` → add `fade-in data-delay="200"`
6. Contact inner div `<div style="padding: 4rem 0;">` → `<div class="fade-in" style="padding: 4rem 0;">`

- [ ] **Step 4: Add Intersection Observer script before `</body>`**

```html
<script>
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.style.transitionDelay = (entry.target.dataset.delay || 0) + 'ms';
                entry.target.classList.add('visible');
                observer.unobserve(entry.target);
            }
        });
    }, { threshold: 0.15 });

    document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
</script>
```

- [ ] **Step 5: Open `index.html` in a browser and verify**
  - Hero h1, subtitle, and button stagger-fade in on page load
  - Scrolling down: about text and headshot fade in (offset stagger)
  - Booking cards reveal with 0/100/200ms stagger
  - Contact section fades in on scroll
  - Nav links (Home, About, Booking, Contact) show a thin underline sliding in from left on hover
  - Hero button scales slightly on hover

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add scroll animations and hover effects to index.html"
```

---

### Task 2: Animate Musical-experience3.html

**Files:**
- Modify: `Musical-experience3.html`

- [ ] **Step 1: Add animation CSS inside the existing `<style>` block, just before `</style>`**

```css
/* === Animations === */
@keyframes pageHeadFadeIn {
    from { opacity: 0; transform: translateY(15px); }
    to   { opacity: 1; transform: translateY(0); }
}

.fade-in {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.visible {
    opacity: 1;
    transform: translateY(0);
}

/* Page title entrance on load */
.portfolio-header h1 { animation: pageHeadFadeIn 0.8s ease both; }
.portfolio-header p  { animation: pageHeadFadeIn 0.8s ease 0.2s both; }

/* Nav link underline slide-in */
.nav-links > a { position: relative; }
.nav-links > a::after {
    content: '';
    position: absolute;
    bottom: -3px;
    left: 0;
    width: 0;
    height: 1px;
    background-color: currentColor;
    transition: width 0.3s ease;
}
.nav-links > a:hover::after { width: 100%; }
```

- [ ] **Step 2: Add `.fade-in` classes and `data-delay` attributes to target elements**

1. `<div class="biography-section">` → `<div class="biography-section fade-in">`
2. First `.portfolio-card` → add `fade-in data-delay="0"`
3. Second `.portfolio-card` → add `fade-in data-delay="100"`
4. Third `.portfolio-card` → add `fade-in data-delay="200"`
5. Fourth `.portfolio-card` → add `fade-in data-delay="300"`
6. First `.gallery-item` → add `fade-in data-delay="0"`
7. Second `.gallery-item` → add `fade-in data-delay="100"`
8. Third `.gallery-item` → add `fade-in data-delay="200"`
9. Fourth `.gallery-item` → add `fade-in data-delay="300"`

- [ ] **Step 3: Add Intersection Observer script before `</body>`**

```html
<script>
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.style.transitionDelay = (entry.target.dataset.delay || 0) + 'ms';
                entry.target.classList.add('visible');
                observer.unobserve(entry.target);
            }
        });
    }, { threshold: 0.15 });

    document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
</script>
```

- [ ] **Step 4: Open `Musical-experience3.html` in a browser and verify**
  - Page h1 and subtitle fade in on load (staggered)
  - Biography block fades in as you scroll
  - Portfolio cards stagger in (0/100/200/300ms)
  - Gallery images stagger in (0/100/200/300ms)
  - Nav links show underline on hover

- [ ] **Step 5: Commit**

```bash
git add Musical-experience3.html
git commit -m "feat: add scroll animations and hover effects to Musical-experience3.html"
```

---

### Task 3: Animate Arts-Admin-Experience2.html

**Files:**
- Modify: `Arts-Admin-Experience2.html`

- [ ] **Step 1: Add animation CSS inside the existing `<style>` block, just before `</style>`**

```css
/* === Animations === */
@keyframes pageHeadFadeIn {
    from { opacity: 0; transform: translateY(15px); }
    to   { opacity: 1; transform: translateY(0); }
}

.fade-in {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.visible {
    opacity: 1;
    transform: translateY(0);
}

/* Page title entrance on load */
.portfolio-header h1 { animation: pageHeadFadeIn 0.8s ease both; }
.portfolio-header p  { animation: pageHeadFadeIn 0.8s ease 0.2s both; }

/* Nav link underline slide-in */
.nav-links > a { position: relative; }
.nav-links > a::after {
    content: '';
    position: absolute;
    bottom: -3px;
    left: 0;
    width: 0;
    height: 1px;
    background-color: currentColor;
    transition: width 0.3s ease;
}
.nav-links > a:hover::after { width: 100%; }
```

- [ ] **Step 2: Add `.fade-in` classes and `data-delay` attributes to target elements**

1. First `.portfolio-card` → add `fade-in data-delay="0"`
2. Second `.portfolio-card` → add `fade-in data-delay="150"`
3. Third `.portfolio-card` → add `fade-in data-delay="300"`

- [ ] **Step 3: Add observer code into the existing `<script>` block (carousel logic), at the end before `</script>`**

```javascript
// Scroll Animation Observer
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.style.transitionDelay = (entry.target.dataset.delay || 0) + 'ms';
            entry.target.classList.add('visible');
            observer.unobserve(entry.target);
        }
    });
}, { threshold: 0.15 });

document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
```

- [ ] **Step 4: Open `Arts-Admin-Experience2.html` in a browser and verify**
  - Page h1 and subtitle fade in on load
  - Portfolio cards stagger in on scroll (0/150/300ms)
  - Image carousel and video carousel still function correctly
  - Nav links show underline on hover

- [ ] **Step 5: Commit**

```bash
git add Arts-Admin-Experience2.html
git commit -m "feat: add scroll animations and hover effects to Arts-Admin-Experience2.html"
```
