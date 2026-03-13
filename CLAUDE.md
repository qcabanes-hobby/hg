# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static site of standalone interactive HTML technical reports — each is a single self-contained file with embedded CSS, JS, and content (no external dependencies except Google Fonts and optionally Pyodide CDN). Deployed to GitHub Pages via `main` branch push.

**Live site:** https://qcabanes-hobby.github.io/hg/

## Architecture

```
index.html          ← Landing page (card grid linking to reports)
reports/
  mesh_network.html       ← Interactive (Pyodide) — PRIMARY STYLE REFERENCE
  uuv_rl_navigation.html  ← Interactive (Pyodide) — METHODOLOGY STRUCTURE REFERENCE
  squid_magnetometry.html  ← Interactive (Pyodide)
  uw_ofdm_modem_.html     ← Static HTML (18k lines, largest report)
.github/workflows/pages.yml ← GitHub Pages deploy on push to main
```

No build step, no package manager, no tests. Reports are opened directly in a browser or served via GitHub Pages.

## Deployment

Push to `main` triggers `.github/workflows/pages.yml` which deploys the entire repo root to GitHub Pages. Every file in the repo is publicly served.

## Report Types

**Interactive (Pyodide):** Load Python 3.12 via Pyodide CDN. Python source lives in `<script type="text/x-python" id="src-qN">` tags. Each script is self-contained (no cross-script imports). A runtime badge in the topbar shows loading state → "Ready". Users click "Run" buttons to execute scripts; output renders into `#out-qN` divs.

**Static:** Pure HTML/CSS/JS. Badge shows "HTML · Static Report". No Python runtime.

## Style & Structure Reference

**`mesh_network.html`** is the primary visual style reference. All new reports must match it.
**`uuv_rl_navigation.html`** is the reference for methodology structure (but note its section headers use inline spans in h2, while the canonical pattern uses flex div wrappers — prefer the mesh_network pattern for new reports).

### Page Layout
- Topbar: `<header id="topbar">` with `.title-bar` and `.runtime-badge` (56px fixed)
- Sidebar: `<nav>` with class `.sidebar` or id `#sidebar` — contains `.nav-group`, `.nav-item`, `.nav-num`, `.nav-dot` (260px fixed left)
- Main: `.main` or `#main` — scrollable content area

### Section Structure
```html
<section class="section" id="sec-name">
  <div class="section-header">
    <span class="section-num">NUM</span>
    <h2 class="st">Title</h2>
  </div>
  <!-- content -->
</section>
```
Section numbers appear **LEFT** of titles via flex layout, not above.

### Hero Section
`.hero` > `.hero-tag` + `<h1>` + `.hero-sub` + `.hero-meta`

### Section Numbering Convention
- 00: Introduction (hero)
- 01: Abstract
- 02: Problem Statement
- 03: Methodology
- Q1–Qn: Question sections (use `.section-num.v` violet color)
- Appendix sections use sequential numbers after questions (e.g., 08, 09, 10)

### Methodology Section
Four-step `.meth-grid` with `.meth-card` items:
1. ① State of the Art — survey techniques with comparison table + citations
2. ② Comparative Analysis — pros/cons, CHOSEN box with justification
3. ③ Solution Planning — architecture, math, design decisions, diagrams
4. ④ Solution & Discussion — implementation, results, analysis

### Question Structure (within each Q section)
Each question follows the 4-step methodology:
- `<h3 class="sub">① State of the Art</h3>` with comparison table
- `<h3 class="sub">② Comparative Analysis & Selection</h3>` with `.chosen` box
- `<h3 class="sub">③ Solution Planning</h3>`
- `<h3 class="sub">④ Implementation / Solution</h3>`

### Key CSS Classes
- `.tw` — table wrapper (`.tw table`, `.tw th`, `.tw td`)
- `.formula` — monospaced math display blocks
- `.code-card` — code blocks with `.code-toolbar`, `.code-lang`, `.code-body`
- `.chosen` — highlighted selection box with `.chosen-label`
- `.bib-item` — bibliography entries with `.bib-num` and `.bib-text`

### CSS Color Scheme (dark theme)
- `--cyan` — primary accent, section nums, active nav
- `--violet` — question section nums (`.section-num.v`)
- `--green` — selected/positive items (`.section-num.g`)
- `--amber` — warnings/special sections (`.section-num.a`)
- Each color has a `-dim` variant (e.g., `--cyan-dim`) for backgrounds

### Bibliography Style
```html
<div class="bib-item" id="ref-N">
  <div class="bib-num"><a href="#ref-N" class="bib-link">[N]</a></div>
  <div class="bib-text">Author, <em>Title</em>, Publication, Year.</div>
</div>
```

### Common JS Patterns
- **Scroll spy:** IntersectionObserver on `.section[id]` toggles `.active` on `.nav-item[href]`
- **Syntax highlighting:** Custom function for Python (keywords, imports, strings, comments, numbers)
- **Pyodide init:** Load from CDN → populate `SCRIPTS` object from `<script type="text/x-python">` tags → run on button click → capture stdout to output div

## User Preferences
- No unnecessary emojis
- Match reference file styles exactly — consistency is critical
- When user gives feedback as a list, address every item systematically
- Content accuracy questions should be flagged rather than silently changed
- Keep reports self-contained (single HTML file)
