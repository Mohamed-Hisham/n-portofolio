# CV Enrichment Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Enrich Nouran Tarek Soliman's academic portfolio by adding Education, Teaching, and Credentials sections, refining the hero, and adding page-wide visual polish — all inside a single inline HTML/CSS/JS file.

**Architecture:** Every change is in `index.html`. CSS is added inside the existing `<style>` block (before `</style>` at line 626). New HTML sections are inserted between existing sections. JS additions go before `</script>` at line 867. No files are created or deleted.

**Tech Stack:** Vanilla HTML5, CSS3 (custom properties, grid, IntersectionObserver animations), vanilla JS — zero dependencies, zero build step.

**Spec:** `docs/superpowers/specs/2026-06-08-cv-enrichment-design.md`

---

## File Map

| File | What changes |
|---|---|
| `index.html:84` | Hero background gradient |
| `index.html:118-129` | `.profile-img` — add glow to box-shadow |
| `index.html:626` | Insert all new CSS blocks before `</style>` |
| `index.html:634-641` | Header HTML — add watermark + institution label |
| `index.html:644-650` | Nav — add 3 new `<li>` items |
| `index.html:687-707` | Delete expertise cards block |
| `index.html:~709` | Insert Education section after About closing tag |
| `index.html:~after education` | Insert Teaching section |
| `index.html:760` | Wrap publications grid in timeline spine |
| `index.html:~after publications` | Insert Credentials section |
| `index.html:~between sections` | Insert 6 section dividers |
| `index.html:~628` | Insert progress dots HTML after `</nav>` |
| `index.html:842` | Extend navObserver JS + add dots JS + lang bar JS |
| `index.html:600-613` | Extend prefers-reduced-motion block |

---

## Task 1: Hero — Deepen Background + Gold Accents

**Files:** Modify `index.html:84`, `index.html:118-129`, insert new CSS before `</style>`

- [ ] **Step 1: Update hero background gradient**

Find line 84 in `index.html`:
```css
background: linear-gradient(135deg, var(--primary) 0%, #2a3f5f 100%);
```
Replace with:
```css
background: linear-gradient(135deg, #0d1520 0%, #1a2332 50%, #0f1c2e 100%);
```

- [ ] **Step 2: Add gold top border and glow CSS before `</style>`**

Insert before `</style>` (line 626):
```css
        /* Hero refinements */
        header::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--accent), transparent);
            z-index: 2;
        }

        .hero-glow {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -60%);
            width: 320px;
            height: 320px;
            background: radial-gradient(ellipse, rgba(193,154,107,0.08) 0%, transparent 70%);
            pointer-events: none;
            z-index: 0;
        }

        .hero-watermark {
            position: absolute;
            bottom: 20px;
            left: 0;
            right: 0;
            text-align: center;
            font-size: clamp(1.8rem, 8vw, 4.5rem);
            color: rgba(193, 154, 107, 0.06);
            direction: rtl;
            pointer-events: none;
            user-select: none;
            z-index: 0;
            font-family: 'Cormorant Garamond', serif;
            letter-spacing: 4px;
        }

        .hero-institution {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin-top: 16px;
            font-family: 'Crimson Pro', serif;
            font-size: 0.85rem;
            letter-spacing: 2px;
            color: rgba(193, 154, 107, 0.7);
            text-transform: uppercase;
            animation: fadeInUp 1s ease-out 0.8s both;
        }

        .hero-institution::before,
        .hero-institution::after {
            content: '';
            width: 30px;
            height: 1px;
            background: rgba(193, 154, 107, 0.4);
        }
```

- [ ] **Step 3: Update `.profile-img` box-shadow to add glow ring**

Find the `.profile-img` block (around line 118). Change only the `box-shadow` line from:
```css
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
```
To:
```css
            box-shadow: 0 20px 60px rgba(0,0,0,0.4), 0 0 40px rgba(193,154,107,0.12);
```

- [ ] **Step 4: Verify in browser**

Open `index.html` in a browser. The hero should be noticeably darker (near-black → navy), with a faint gold hairline at the very top edge and a subtle warm glow around the profile photo.

- [ ] **Step 5: Commit**
```bash
git add index.html
git commit -m "feat: hero dark luxury refinement — deeper bg, gold border, glow"
```

---

## Task 2: Hero HTML — Watermark + Institution Label

**Files:** Modify `index.html:634-641`

- [ ] **Step 1: Update header HTML**

Find lines 634–641:
```html
    <header>
        <div class="header-content">
            <img src="./assets/images/profile_pic-cmpr.png" alt="Nouran Tarek Soliman" class="profile-img" id="profileImage">
            <div class="subtitle">English Linguistics Expert</div>
            <h1>Nouran Tarek Soliman</h1>
            <div class="academic-title">Applied Linguistics Ph.D. Candidate</div>
        </div>
    </header>
```
Replace with:
```html
    <header>
        <div class="hero-glow" aria-hidden="true"></div>
        <div class="hero-watermark" aria-hidden="true">نوران طارق سليمان</div>
        <div class="header-content">
            <img src="./assets/images/profile_pic-cmpr.png" alt="Nouran Tarek Soliman" class="profile-img" id="profileImage">
            <div class="subtitle">English Linguistics Expert</div>
            <h1>Nouran Tarek Soliman</h1>
            <div class="academic-title">Applied Linguistics Ph.D. Candidate</div>
            <div class="hero-institution">AAST · AUC</div>
        </div>
    </header>
```

- [ ] **Step 2: Verify in browser**

Arabic text `نوران طارق سليمان` should be barely visible at the bottom of the hero — a ghost-like watermark. The institution label `AAST · AUC` with flanking gold rules appears below the academic title, fading in last.

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "feat: hero HTML — Arabic watermark and institution label"
```

---

## Task 3: Nav — Add 3 New Items

**Files:** Modify `index.html:644-650`

- [ ] **Step 1: Update nav HTML**

Find lines 644–650:
```html
    <nav>
        <ul>
            <li><a href="#about">About</a></li>
            <li><a href="#research">Research</a></li>
            <li><a href="#publications">Publications</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>
```
Replace with:
```html
    <nav>
        <ul>
            <li><a href="#about">About</a></li>
            <li><a href="#education">Education</a></li>
            <li><a href="#teaching">Teaching</a></li>
            <li><a href="#research">Research</a></li>
            <li><a href="#publications">Publications</a></li>
            <li><a href="#credentials">Credentials</a></li>
        </ul>
    </nav>
```

- [ ] **Step 2: Verify in browser**

Nav should now show 6 items. The active highlight still works when scrolling (the existing IntersectionObserver picks up new section IDs automatically once the sections are added).

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "feat: nav — add Education, Teaching, Credentials items"
```

---

## Task 4: Remove Expertise Cards

**Files:** Modify `index.html:287-333` (CSS) and `index.html:687-707` (HTML)

- [ ] **Step 1: Delete the expertise HTML block**

Find and delete lines 687–707 — the entire block from `<div style="margin-top: 80px;"` through its closing `</div></div>`:
```html
            <div style="margin-top: 80px;" class="fade-in">
                <h3 style="font-family: 'Cormorant Garamond', serif; font-size: 2.2rem; color: var(--primary); margin-bottom: 40px; text-align: center;">Areas of Expertise</h3>
                <div class="expertise-areas">
                    <div class="expertise-item">
                        <h4>🔬 Sociolinguistic Research</h4>
                        <p>Conducting empirical research on language ideologies, attitudes, and variation across Arabic dialects with focus on media discourse and social media metalinguistic commentary.</p>
                    </div>
                    <div class="expertise-item">
                        <h4>📚 English Language Teaching</h4>
                        <p>Extensive experience in academic English instruction, curriculum development, and pedagogical innovation for diverse learner populations.</p>
                    </div>
                    <div class="expertise-item">
                        <h4>✍️ Academic Editing & Writing</h4>
                        <p>Professional editing services with expertise in scholarly writing, research methodology, and publication standards across linguistics disciplines.</p>
                    </div>
                    <div class="expertise-item">
                        <h4>🤖 AI-Enhanced Language Learning</h4>
                        <p>Integrating artificial intelligence technologies with language pedagogy to create innovative learning experiences and tutoring solutions.</p>
                    </div>
                </div>
            </div>
```

- [ ] **Step 2: Also remove now-unused CSS**

Find and delete the `.expertise-areas`, `.expertise-item`, `.expertise-item:hover`, `.expertise-item h4`, `.expertise-item p` CSS blocks (around lines 310–333):
```css
        .expertise-areas {
            display: grid;
            gap: 20px;
        }

        .expertise-item {
            background: var(--bg-alt);
            padding: 25px;
            border-left: 4px solid var(--secondary);
            transition: all 0.3s;
        }

        .expertise-item:hover {
            transform: translateX(5px);
            box-shadow: 0 5px 20px var(--shadow);
        }

        .expertise-item h4 {
            font-family: 'Crimson Pro', serif;
            font-size: 1.3rem;
            color: var(--secondary);
            margin-bottom: 8px;
        }

        .expertise-item p {
            font-size: 0.95rem;
            color: var(--text-light);
            margin: 0;
        }
```

- [ ] **Step 3: Verify in browser**

About section should end cleanly after the contact card with no emoji cards and no orphaned whitespace.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "refactor: remove expertise emoji cards — superseded by Teaching section"
```

---

## Task 5: Education Section — CSS + HTML

**Files:** Insert CSS before `</style>`, insert HTML after the About `</section>` closing tag

- [ ] **Step 1: Add Swiss Grid + Education CSS before `</style>`**

```css
        /* ── Swiss Grid (shared by Education, Teaching, Publications) ── */
        .swiss-grid {
            display: grid;
            grid-template-columns: 100px 2px 1fr;
            gap: 0;
        }

        .swiss-rule {
            background: linear-gradient(180deg, var(--accent) 0%, rgba(193,154,107,0.15) 100%);
            margin: 0 24px;
        }

        .swiss-year {
            font-family: 'Crimson Pro', serif;
            font-size: 0.85rem;
            color: var(--secondary);
            font-weight: 600;
            text-align: right;
            padding-top: 4px;
            line-height: 1.4;
        }

        .swiss-entry {
            padding: 0 0 40px 24px;
        }

        .swiss-entry:last-child {
            padding-bottom: 0;
        }

        .swiss-entry h4 {
            font-family: 'Cormorant Garamond', serif;
            font-size: 1.4rem;
            color: var(--primary);
            margin-bottom: 4px;
            line-height: 1.3;
        }

        .swiss-entry .inst {
            font-family: 'Crimson Pro', serif;
            font-size: 1rem;
            color: var(--secondary);
            margin-bottom: 4px;
        }

        .swiss-entry .meta {
            font-size: 0.9rem;
            color: var(--text-light);
            font-style: italic;
        }

        .swiss-entry .gpa {
            display: inline-block;
            font-family: 'Crimson Pro', serif;
            font-size: 0.8rem;
            font-weight: 600;
            color: var(--secondary);
            letter-spacing: 1px;
            text-transform: uppercase;
            margin-left: 8px;
            font-style: normal;
        }

        .concurrent-badge {
            display: inline-block;
            font-family: 'Crimson Pro', serif;
            font-size: 0.75rem;
            letter-spacing: 1px;
            text-transform: uppercase;
            padding: 2px 10px;
            border: 1px solid var(--border);
            color: var(--secondary);
            margin-bottom: 12px;
        }

        .concurrent-group {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }

        .concurrent-group .swiss-entry {
            padding-left: 0;
            padding-bottom: 0;
        }

        @media (max-width: 600px) {
            .swiss-grid {
                grid-template-columns: 1fr;
            }
            .swiss-rule { display: none; }
            .swiss-year { text-align: left; padding-bottom: 4px; }
            .swiss-entry { padding-left: 0; }
            .concurrent-group { grid-template-columns: 1fr; }
        }
```

- [ ] **Step 2: Insert Education HTML after the About `</section>` closing tag**

Find the line `    </section>` that closes the About section (the one just before the Research section comment). Insert after it:

```html
    <!-- Education -->
    <section id="education" style="background: var(--bg-alt);">
        <div class="container">
            <div class="section-header fade-in">
                <h2 class="section-title">Education</h2>
                <p class="section-subtitle">Four degrees across two institutions, including concurrent postgraduate study</p>
            </div>

            <div class="swiss-grid fade-in">
                <div><div class="swiss-year">2024<br>Present</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <h4>Ph.D. in Applied Linguistics</h4>
                    <div class="inst">Arab Academy for Science, Technology &amp; Maritime Transport</div>
                    <div class="meta">College of Language &amp; Communication<span class="gpa">GPA 4.0</span></div>
                </div>

                <div><div class="swiss-year" style="padding-top: 40px;">2020<br>2024</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <div class="concurrent-badge">Concurrent Degrees</div>
                    <div class="concurrent-group">
                        <div class="swiss-entry">
                            <h4>MA TESOL</h4>
                            <div class="inst">The American University in Cairo</div>
                            <div class="meta">Applied Linguistics Dept. · <em>Thesis: Sociolinguistics</em><span class="gpa">GPA 3.8</span></div>
                        </div>
                        <div class="swiss-entry">
                            <h4>TAFL Diploma</h4>
                            <div class="inst">Arab Academy for Science, Technology &amp; Maritime Transport</div>
                            <div class="meta">Teaching Arabic as a Foreign Language<span class="gpa">GPA 3.9</span></div>
                        </div>
                    </div>
                </div>

                <div><div class="swiss-year" style="padding-top: 40px;">2015<br>2019</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <h4>BA in Language &amp; Translation</h4>
                    <div class="inst">Arab Academy for Science, Technology &amp; Maritime Transport</div>
                    <div class="meta">College of Language &amp; Communication · <em>Thesis: Audio Visual Translation</em><span class="gpa">GPA 3.78</span></div>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 3: Verify in browser**

Education section appears between About and Research. Four degree entries in a Swiss Grid: years right-aligned on the left, separated by a gold gradient rule. Concurrent degrees sit side-by-side with the badge above them. On mobile (< 600px) it collapses to a single column.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: Education section — Swiss Grid with 4 degrees and concurrent badge"
```

---

## Task 6: Teaching Section — CSS + HTML

**Files:** Insert CSS before `</style>`, insert HTML after Education `</section>`

- [ ] **Step 1: Add Teaching-specific CSS before `</style>`**

```css
        /* Teaching */
        .intl-teaching {
            display: flex;
            gap: 32px;
            margin-top: 48px;
            padding-top: 28px;
            border-top: 1px solid var(--border);
            flex-wrap: wrap;
        }

        .intl-chip {
            font-family: 'Crimson Pro', serif;
            font-size: 1rem;
            color: var(--text-light);
        }

        .intl-chip strong {
            color: var(--text);
        }
```

- [ ] **Step 2: Insert Teaching HTML after the Education `</section>`**

```html
    <!-- Teaching -->
    <section id="teaching">
        <div class="container">
            <div class="section-header fade-in">
                <h2 class="section-title">Teaching</h2>
                <p class="section-subtitle">Over 6 years of academic English instruction across three countries</p>
            </div>

            <div class="swiss-grid fade-in">
                <div><div class="swiss-year">2023<br>Present</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <h4>Research Paper Writing Coordinator</h4>
                    <div class="inst">German International University — Cairo, Egypt</div>
                    <div class="meta">Designed &amp; updated RPW teaching materials · Trained 10–15 instructors/semester on AI tools and pedagogy · Standardised materials across RPW classes</div>
                </div>

                <div><div class="swiss-year" style="padding-top:40px;">2021<br>Present</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <h4>English Instructor — Academic Purposes</h4>
                    <div class="inst">German International University — Cairo, Egypt</div>
                    <div class="meta">5 EAP courses: Research Paper Writing · Communication &amp; Presentation Skills · Critical Thinking &amp; Scientific Methodology · Academic Study Skills · Academic English · ~275 students/semester</div>
                </div>

                <div><div class="swiss-year" style="padding-top:40px;">2019<br>2021</div></div>
                <div class="swiss-rule"></div>
                <div class="swiss-entry">
                    <h4>Teaching Assistant — English for Specific Purposes</h4>
                    <div class="inst">Arab Academy for Science, Technology &amp; Maritime Transport — Alexandria, Egypt</div>
                    <div class="meta">900+ classes taught on-campus and online · Faculties: Engineering, Computer Science, Business &amp; Tourism · Maintained instruction through COVID-19</div>
                </div>
            </div>

            <div class="intl-teaching fade-in">
                <span class="intl-chip">🇪🇬 <strong>Cairo, Egypt</strong> — 2019–Present</span>
                <span class="intl-chip">🇨🇳 <strong>Ningbo, China</strong> — Summer 2018 (AIESEC)</span>
                <span class="intl-chip">🇺🇦 <strong>Kyiv, Ukraine</strong> — Summer 2017 (AIESEC)</span>
            </div>
        </div>
    </section>
```

- [ ] **Step 3: Verify in browser**

Teaching section appears between Education and Research. Three roles in Swiss Grid matching Education visually. International teaching chips appear below with flag emojis and dates. On mobile the grid collapses cleanly.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: Teaching section — Swiss Grid with 3 roles and international chips"
```

---

## Task 7: Publications Timeline Spine

**Files:** Modify `index.html` Publications section HTML, add CSS

- [ ] **Step 1: Add publications timeline CSS before `</style>`**

```css
        /* Publications timeline */
        .publications-timeline {
            display: grid;
            grid-template-columns: 80px 2px 1fr;
            gap: 0;
        }

        .pub-date-col {
            display: flex;
            flex-direction: column;
        }

        .pub-date-marker {
            font-family: 'Crimson Pro', serif;
            font-size: 0.8rem;
            color: var(--secondary);
            font-weight: 600;
            text-align: right;
            line-height: 1.3;
        }

        .pub-timeline-rule {
            background: linear-gradient(180deg, var(--accent) 0%, rgba(193,154,107,0.15) 100%);
            margin: 0 24px;
        }

        .pub-timeline-entries {
            display: flex;
            flex-direction: column;
            gap: 40px;
            padding-left: 24px;
        }

        @media (max-width: 600px) {
            .publications-timeline {
                grid-template-columns: 1fr;
            }
            .pub-timeline-rule { display: none; }
            .pub-date-col { display: none; }
            .pub-timeline-entries { padding-left: 0; gap: 30px; }
        }
```

- [ ] **Step 2: Wrap existing publications in timeline grid**

Find the existing `<div class="publications-grid">` wrapper in the Publications section. Replace the opening wrapper tag with:
```html
            <div class="publications-timeline fade-in">
                <div class="pub-date-col">
                    <div class="pub-date-marker" style="padding-top:4px;">Dec<br>2024</div>
                    <div style="flex:1;"></div>
                    <div class="pub-date-marker">Sep<br>2024</div>
                    <div style="flex:1;"></div>
                    <div class="pub-date-marker">Jan<br>2024</div>
                </div>
                <div class="pub-timeline-rule"></div>
                <div class="pub-timeline-entries">
```
And close with:
```html
                </div>
            </div>
```
The three existing `<article class="publication-card fade-in">` blocks go inside `.pub-timeline-entries` unchanged. Remove the old `<div class="publications-grid">` and its closing tag.

- [ ] **Step 3: Verify in browser**

Publications section shows a gold vertical rule on the left with `Dec 2024`, `Sep 2024`, `Jan 2024` markers. Cards are unchanged. On mobile the date spine hides and cards stack full-width.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: Publications — add Swiss Grid timeline spine"
```

---

## Task 8: Credentials Section — CSS + HTML + JS

**Files:** Insert CSS before `</style>`, insert HTML before `<footer>`, add JS near lang bar observer

- [ ] **Step 1: Add Credentials CSS before `</style>`**

```css
        /* Credentials & Languages */
        .cert-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 60px;
        }

        .cert-card {
            padding: 28px;
            border-radius: 4px;
            transition: transform 0.3s;
        }

        .cert-card:hover {
            transform: translateY(-3px);
        }

        .cert-card.cert-navy {
            background: var(--primary);
            color: var(--bg);
        }

        .cert-card.cert-brown {
            background: var(--secondary);
            color: var(--bg);
        }

        .cert-card.cert-parchment {
            background: var(--bg);
            border: 1px solid var(--border);
            color: var(--text);
        }

        .cert-card h4 {
            font-family: 'Cormorant Garamond', serif;
            font-size: 1.4rem;
            margin-bottom: 6px;
        }

        .cert-card .cert-issuer {
            font-family: 'Crimson Pro', serif;
            font-size: 0.9rem;
            opacity: 0.85;
            margin-bottom: 4px;
        }

        .cert-card .cert-date {
            font-size: 0.8rem;
            opacity: 0.65;
            letter-spacing: 1px;
        }

        .cert-card.cert-parchment h4 { color: var(--primary); }
        .cert-card.cert-parchment .cert-issuer { color: var(--secondary); opacity: 1; }

        .lang-section h3 {
            font-family: 'Cormorant Garamond', serif;
            font-size: 2rem;
            color: var(--primary);
            margin-bottom: 32px;
            text-align: center;
        }

        .lang-entry {
            display: grid;
            grid-template-columns: 160px 1fr 120px;
            align-items: center;
            gap: 20px;
            margin-bottom: 20px;
        }

        .lang-name {
            font-family: 'Crimson Pro', serif;
            font-size: 1.05rem;
            font-weight: 600;
            color: var(--primary);
        }

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
            transition: width 1.2s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .lang-level {
            font-family: 'Crimson Pro', serif;
            font-size: 0.85rem;
            color: var(--text-light);
            text-align: right;
        }

        @media (max-width: 600px) {
            .cert-grid { grid-template-columns: 1fr; }
            .lang-entry { grid-template-columns: 1fr; gap: 6px; }
            .lang-level { text-align: left; }
        }
```

- [ ] **Step 2: Insert Credentials HTML before `<footer>`**

Find `<footer>` and insert before it:
```html
    <!-- Credentials & Languages -->
    <section id="credentials" style="background: var(--bg-alt);">
        <div class="container">
            <div class="section-header fade-in">
                <h2 class="section-title">Credentials &amp; Languages</h2>
                <p class="section-subtitle">Professional certifications and linguistic competencies</p>
            </div>

            <div class="cert-grid fade-in">
                <div class="cert-card cert-navy">
                    <h4>CELTA</h4>
                    <div class="cert-issuer">University of Cambridge</div>
                    <div class="cert-date">Sep 2019 · Certificate no. ccpf684120</div>
                </div>
                <div class="cert-card cert-brown">
                    <h4>IELTS 7.5</h4>
                    <div class="cert-issuer">University of Cambridge</div>
                    <div class="cert-date">Jun 2020 · C1 Advanced · School of English</div>
                </div>
                <div class="cert-card cert-parchment">
                    <h4>Social &amp; Behavioral Research</h4>
                    <div class="cert-issuer">CITI — Collaborative Institutional Training Initiative</div>
                    <div class="cert-date">Sep 2020</div>
                </div>
                <div class="cert-card cert-parchment">
                    <h4>AI Career Essentials</h4>
                    <div class="cert-issuer">ALX Africa</div>
                    <div class="cert-date">Jul 2024</div>
                </div>
            </div>

            <div class="lang-section fade-in">
                <h3>Languages</h3>
                <div class="lang-entry">
                    <div class="lang-name">Arabic</div>
                    <div class="lang-bar-track">
                        <div class="lang-bar-fill" data-width="100%" style="background: var(--secondary);"></div>
                    </div>
                    <div class="lang-level">Native</div>
                </div>
                <div class="lang-entry">
                    <div class="lang-name">English</div>
                    <div class="lang-bar-track">
                        <div class="lang-bar-fill" data-width="85%" style="background: var(--secondary);"></div>
                    </div>
                    <div class="lang-level">C1 Advanced</div>
                </div>
                <div class="lang-entry">
                    <div class="lang-name">Spanish</div>
                    <div class="lang-bar-track">
                        <div class="lang-bar-fill" data-width="25%" style="background: var(--accent);"></div>
                    </div>
                    <div class="lang-level">A2 Elementary</div>
                </div>
            </div>
        </div>
    </section>
```

- [ ] **Step 3: Add lang bar animation JS**

Find the block near line 842 where `document.querySelectorAll('.fade-in').forEach(el => { observer.observe(el); });` appears. After that block, add:
```js
        // Language bar animation
        const langBars = document.querySelectorAll('.lang-bar-fill');
        const langObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.width = entry.target.dataset.width;
                    langObserver.unobserve(entry.target);
                }
            });
        }, { threshold: 0.5 });
        langBars.forEach(bar => langObserver.observe(bar));
```

- [ ] **Step 4: Verify in browser**

Credentials section appears before the footer. Four cert cards in a 2×2 grid — two dark (navy/brown), two light (parchment). Language bars animate from 0 when scrolled into view. On mobile, certs stack to single column.

- [ ] **Step 5: Commit**
```bash
git add index.html
git commit -m "feat: Credentials section — cert grid and animated language bars"
```

---

## Task 9: Section Dividers

**Files:** Insert HTML between sections, add CSS

- [ ] **Step 1: Add divider CSS before `</style>`**

```css
        /* Section dividers */
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

        .section-divider span {
            color: var(--accent);
            font-size: 0.7rem;
        }
```

- [ ] **Step 2: Insert 6 dividers between sections**

Insert `<div class="section-divider" aria-hidden="true"><span>✦</span></div>` in 6 places:
1. After About `</section>`, before Education `<section>`
2. After Education `</section>`, before Teaching `<section>`
3. After Teaching `</section>`, before Research `<section>`
4. After Research `</section>`, before Publications `<section>`
5. After Publications `</section>`, before Credentials `<section>`
6. After Credentials `</section>`, before `<footer>`

- [ ] **Step 3: Verify in browser**

A faint gold hairline with a centered ✦ appears between every section. Transitions between sections feel book-like rather than abrupt background-color switches.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: ornamental gold dividers between all sections"
```

---

## Task 10: Progress Dots

**Files:** Insert HTML after `</nav>` tag, add CSS, extend navObserver JS

- [ ] **Step 1: Add progress dots CSS before `</style>`**

```css
        /* Progress dots */
        .progress-dots {
            position: fixed;
            right: 24px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 50;
            background: none;
            border: none;
            padding: 0;
        }

        .progress-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            border: 1.5px solid var(--border);
            background: transparent;
            cursor: pointer;
            transition: background 0.3s, border-color 0.3s, transform 0.3s;
            padding: 0;
            display: block;
        }

        .progress-dot.active {
            background: var(--secondary);
            border-color: var(--secondary);
            transform: scale(1.35);
        }

        .progress-dot:hover {
            border-color: var(--secondary);
        }

        @media (max-width: 1024px) {
            .progress-dots { display: none; }
        }
```

- [ ] **Step 2: Insert progress dots HTML after the closing `</nav>` tag**

```html
    <!-- Progress dots navigation -->
    <nav class="progress-dots" aria-label="Page section progress">
        <button class="progress-dot active" data-target="about" aria-label="About section"></button>
        <button class="progress-dot" data-target="education" aria-label="Education section"></button>
        <button class="progress-dot" data-target="teaching" aria-label="Teaching section"></button>
        <button class="progress-dot" data-target="research" aria-label="Research section"></button>
        <button class="progress-dot" data-target="publications" aria-label="Publications section"></button>
        <button class="progress-dot" data-target="credentials" aria-label="Credentials section"></button>
    </nav>
```

- [ ] **Step 3: Extend navObserver and add dot click handler in the `<script>` section**

Find the existing `navObserver` block:
```js
        const navObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    navLinks.forEach(a => a.classList.remove('active'));
                    const active = document.querySelector(`nav a[href="#${entry.target.id}"]`);
                    if (active) active.classList.add('active');
                }
            });
        }, { threshold: 0.3, rootMargin: '-10% 0px -60% 0px' });

        sections.forEach(s => navObserver.observe(s));
```
Replace it with:
```js
        const navObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    navLinks.forEach(a => a.classList.remove('active'));
                    const active = document.querySelector(`nav a[href="#${entry.target.id}"]`);
                    if (active) active.classList.add('active');

                    document.querySelectorAll('.progress-dot').forEach(d => d.classList.remove('active'));
                    const dot = document.querySelector(`.progress-dot[data-target="${entry.target.id}"]`);
                    if (dot) dot.classList.add('active');
                }
            });
        }, { threshold: 0.3, rootMargin: '-10% 0px -60% 0px' });

        sections.forEach(s => navObserver.observe(s));

        document.querySelectorAll('.progress-dot').forEach(dot => {
            dot.addEventListener('click', () => {
                const target = document.getElementById(dot.dataset.target);
                if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
            });
        });
```

- [ ] **Step 4: Verify in browser**

On desktop (> 1024px): 6 dots appear on the right edge, vertically centered. Active dot (brown, slightly larger) updates as you scroll. Clicking a dot scrolls to that section. On mobile/tablet: dots are hidden.

- [ ] **Step 5: Commit**
```bash
git add index.html
git commit -m "feat: fixed progress dots — scroll-synced section navigation"
```

---

## Task 11: Reduced-Motion Guards

**Files:** Modify the existing `prefers-reduced-motion` block at `index.html:600-613`

- [ ] **Step 1: Extend the existing prefers-reduced-motion block**

Find the existing block:
```css
        @media (prefers-reduced-motion: reduce) {
            *,
            *::before,
            *::after {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }

            .fade-in {
                opacity: 1 !important;
                transform: none !important;
            }
        }
```
Replace with:
```css
        @media (prefers-reduced-motion: reduce) {
            *,
            *::before,
            *::after {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }

            .fade-in {
                opacity: 1 !important;
                transform: none !important;
            }

            .lang-bar-fill {
                transition: none !important;
            }

            .progress-dot {
                transition: none !important;
            }

            .cert-card {
                transition: none !important;
            }
        }
```

- [ ] **Step 2: Verify with reduced-motion**

In Chrome DevTools → Rendering → Emulate CSS media feature → `prefers-reduced-motion: reduce`. Page should load with all fade-in elements visible, language bars at full width instantly, no transitions on dots or cert cards.

- [ ] **Step 3: Final visual check — scroll through all sections**

- [ ] Hero: darker bg, faint gold top line, Arabic watermark at hero bottom, institution label
- [ ] Nav: 6 items, active highlight tracks scroll
- [ ] About: ends cleanly, no expertise cards
- [ ] Gold ✦ divider between About and Education
- [ ] Education: Swiss Grid, 4 entries, Concurrent badge, concurrent side-by-side
- [ ] Gold ✦ divider between Education and Teaching
- [ ] Teaching: Swiss Grid, 3 roles, international chips at bottom
- [ ] Gold ✦ divider between Teaching and Research
- [ ] Research: unchanged
- [ ] Gold ✦ divider between Research and Publications
- [ ] Publications: timeline spine on left with date markers
- [ ] Gold ✦ divider between Publications and Credentials
- [ ] Credentials: 2×2 cert grid (navy/brown/parchment/parchment), language bars animate on scroll
- [ ] Gold ✦ divider between Credentials and Footer
- [ ] Progress dots: 6 dots, active updates on scroll, clickable, hidden on mobile
- [ ] Footer: unchanged

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: reduced-motion guards for lang bars, dots, cert cards"
```

---

## Self-Review

**Spec coverage check:**

| Spec section | Task |
|---|---|
| Hero — deepened bg | Task 1 Step 1 |
| Hero — gold radial glow | Task 1 Step 2 (`.hero-glow`) |
| Hero — gold top border | Task 1 Step 2 (`header::after`) |
| Hero — Arabic watermark | Task 2 Step 1 (`.hero-watermark`) |
| Hero — institution label | Task 2 Step 1 (`.hero-institution`) |
| Nav — 3 new items | Task 3 |
| Remove expertise cards | Task 4 |
| Education Swiss Grid | Task 5 |
| Concurrent badge | Task 5 |
| Teaching Swiss Grid | Task 6 |
| International chips | Task 6 |
| Publications timeline spine | Task 7 |
| Credentials cert grid | Task 8 |
| Language bars animated | Task 8 Step 3 |
| Section dividers | Task 9 |
| Progress dots | Task 10 |
| Reduced-motion guards | Task 11 |

All spec requirements covered. ✓
