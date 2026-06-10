# Portfolio Enrichment — CV Sections Design Spec
**Date:** 2026-06-08
**Branch:** `enrichment/cv-sections`
**File:** `index.html` (single inline HTML/CSS/JS file, no build step)

---

## 1. Scope

Add three new sections (Education, Teaching, Credentials), restyle the hero, unify visual language across all sections with a Swiss Grid pattern, and add four page-wide visual enhancements. No new files, no dependencies — all CSS and JS stay inline in `index.html`.

---

## 2. Hero — Dark Luxury Refined

**Changes to existing hero:**
- Background gradient deepened: `linear-gradient(135deg, #0d1520 0%, #1a2332 50%, #0f1c2e 100%)`
- Gold radial glow behind profile photo: `radial-gradient(ellipse, rgba(193,154,107,0.08) 0%, transparent 70%)`, positioned absolute, centered, ~300px diameter
- Top border accent: `height: 2px`, `background: linear-gradient(90deg, transparent, #c19a6b, transparent)`
- Arabic calligraphy watermark: `نوران طارق سليمان` positioned absolute at bottom of hero, `font-size: clamp(2rem, 8vw, 5rem)`, `color: rgba(193,154,107,0.06)`, `direction: rtl`, `pointer-events: none`, `user-select: none`
- Institution label below academic title: `—— AAST · AUC ——` in gold, `font-family: Crimson Pro`, `letter-spacing: 2px`, `color: rgba(193,154,107,0.7)`
- Existing animated grid pattern and animations: **kept unchanged**

**No layout change** — remains centered single column.

---

## 3. Nav Update

Add three new nav items:
```
About · Education · Teaching · Research · Publications · Credentials
```
- `href="#education"` → Education section
- `href="#teaching"` → Teaching section
- `href="#credentials"` → Credentials section

Existing active nav IntersectionObserver handles new sections automatically.

---

## 4. Remove: Expertise Cards

Delete the entire "Areas of Expertise" block (the `<div style="margin-top: 80px;">` containing the four emoji `expertise-item` cards in the About section). Superseded by the Teaching section.

---

## 5. New Section: Education

**Placement:** After About. Background: `var(--bg-alt)`. Section id: `education`.

**Layout — Swiss Grid:**
```css
.edu-grid {
  display: grid;
  grid-template-columns: 100px 2px 1fr;
  gap: 0 24px;
}
.edu-rule {
  background: linear-gradient(180deg, #c19a6b 0%, rgba(193,154,107,0.2) 100%);
}
.edu-year {
  font-family: 'Crimson Pro', serif;
  font-size: 0.9rem;
  color: var(--secondary);
  font-weight: 600;
  text-align: right;
  padding-top: 4px;
}
.edu-entry { padding-bottom: 40px; }
.edu-entry:last-child { padding-bottom: 0; }
```

**Entries:**
1. `2024 – Present` | **Ph.D. Applied Linguistics** | AAST | GPA: 4.0
2. `2020 – 2024` | `[Concurrent]` badge spanning:
   - **MA TESOL** | AUC | GPA: 3.8 | *Thesis: Sociolinguistics*
   - **TAFL Diploma** | AAST | GPA: 3.9
3. `2015 – 2019` | **BA Language & Translation** | AAST | GPA: 3.78 | *Thesis: Audio Visual Translation*

**Concurrent badge:**
```css
.concurrent-badge {
  display: inline-block;
  font-family: 'Crimson Pro', serif;
  font-size: 0.75rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 2px 10px;
  border: 1px solid var(--border);
  color: var(--secondary);
  margin-bottom: 8px;
}
```

**Responsive:** On `max-width: 768px`, collapse to single column — year above entry, rule hidden.

---

## 6. New Section: Teaching

**Placement:** After Education. Background: `var(--bg)`. Section id: `teaching`.

**Layout:** Same Swiss Grid as Education — identical CSS classes.

**Entries:**
1. `2023 – Present` | **Research Paper Writing Coordinator** | German International University, Cairo
   - Trained 10–15 instructors/semester · Curriculum design · AI tools integration
2. `2021 – Present` | **English Instructor — EAP** | German International University, Cairo
   - 5 courses: RPW · CPS · SM · AS · AE · ~275 students/semester
3. `2019 – 2021` | **Teaching Assistant — ESP** | Arab Academy, Alexandria
   - 900+ classes · On-campus & online through COVID-19

**International Teaching row** (below grid, separated by `border-top: 1px solid var(--border)`):
```html
<div class="intl-teaching">
  <span class="intl-chip">🇪🇬 Cairo, Egypt — 2019–Present</span>
  <span class="intl-chip">🇨🇳 Ningbo, China — 2018</span>
  <span class="intl-chip">🇺🇦 Kyiv, Ukraine — 2017</span>
</div>
```

---

## 7. Publications — Timeline Spine

Wrap existing `publications-grid` in a Swiss Grid container:
```css
.publications-timeline {
  display: grid;
  grid-template-columns: 80px 2px 1fr;
  gap: 0 24px;
}
```
Year markers: `Dec 2024` / `Sep 2024` / `Jan 2024` in `edu-year` style. Same gold rule. Existing publication cards unchanged — just repositioned to content column.

**Responsive:** Collapse to single column on mobile.

---

## 8. New Section: Credentials & Languages

**Placement:** After Publications, before Footer. Background: `var(--bg-alt)`. Section id: `credentials`.

### Certifications — 2×2 Grid
```css
.cert-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 60px;
}
@media (max-width: 480px) { .cert-grid { grid-template-columns: 1fr; } }
```

| Cert | Style | Detail |
|---|---|---|
| CELTA | navy bg, white text | University of Cambridge · Sep 2019 |
| IELTS 7.5 C1 | brown bg, white text | University of Cambridge · Jun 2020 |
| CITI Research | parchment + border | Collaborative Institutional Training Initiative · Sep 2020 |
| AI Career Essentials | parchment + border | ALX Africa · Jul 2024 |

### Languages — Proficiency Bars
```css
.lang-bar-track {
  height: 6px;
  background: var(--border);
  border-radius: 3px;
  overflow: hidden;
}
.lang-bar-fill {
  height: 100%;
  border-radius: 3px;
  width: 0;
  transition: width 1s ease;
}
.lang-bar-fill.visible { width: var(--bar-width); }
```

Entries (animate on scroll via IntersectionObserver):
- **Arabic** · Native · `--bar-width: 100%` · fill: `var(--secondary)`
- **English** · C1 Advanced · `--bar-width: 85%` · fill: `var(--secondary)`
- **Spanish** · A2 Elementary · `--bar-width: 25%` · fill: `var(--accent)`

---

## 9. Page-wide Visual Additions

### Ornamental Section Dividers
Between every `<section>`:
```html
<div class="section-divider" aria-hidden="true"><span>✦</span></div>
```
```css
.section-divider {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 30px;
  max-width: 1200px;
  margin: 0 auto;
}
.section-divider::before,
.section-divider::after {
  content: '';
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent), transparent);
}
.section-divider span { color: var(--accent); font-size: 0.75rem; }
```

### Fixed Side Progress Dots
```html
<nav class="progress-dots" aria-label="Page sections">
  <!-- one <button class="dot"> per section, data-target="section-id" -->
</nav>
```
```css
.progress-dots {
  position: fixed;
  right: 24px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 10px;
  z-index: 50;
}
.dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  border: 1.5px solid var(--border);
  background: transparent;
  cursor: pointer;
  transition: background 0.3s, border-color 0.3s, transform 0.3s;
  padding: 0;
}
.dot.active {
  background: var(--secondary);
  border-color: var(--secondary);
  transform: scale(1.3);
}
@media (max-width: 1024px) { .progress-dots { display: none; } }
```
JS: extend existing `navObserver` to update `.dot.active`. Clicking a dot smooth-scrolls to target.

---

## 10. Reduced Motion

Extend the existing `prefers-reduced-motion` block:
```css
@media (prefers-reduced-motion: reduce) {
  .lang-bar-fill { transition: none; }
  .dot { transition: none; }
}
```

---

## 11. Implementation Order

1. Hero — deepened bg, glow, gold border, Arabic watermark, institution label
2. Nav — add 3 items
3. Remove Expertise cards block
4. Education section
5. Teaching section + international chips
6. Publications timeline spine
7. Credentials section (certs + language bars)
8. Section dividers
9. Progress dots (HTML + CSS + JS)
10. Reduced-motion guards
