# HG Project — Interactive HTML Technical Reports

## Project Overview
This repo contains standalone interactive HTML technical reports (single-file, no external deps).
Reports are in `reports/` directory. Each is a self-contained HTML file with embedded CSS, JS, and content.

## Current Reports
- `mesh_network.html` — Mesh network routing (Pyodide/WebAssembly, runnable Python)
- `uw_ofdm_modem_.html` — Underwater acoustic OFDM modem design (static HTML)
- `uuv_rl_navigation.html` — UUV reinforcement learning navigation (reference for methodology style)
- `squid_magnetometry.html` — SQUID magnetometry

## Style Reference
**`mesh_network.html`** is the primary style reference. All reports must match its visual design.
**`uuv_rl_navigation.html`** is the reference for methodology structure.

### HTML Structure Standards
- Topbar: `<header id="topbar">` with `.title-bar` and `.runtime-badge`
- Sidebar: `<nav id="sidebar">` (or `class="sidebar"`) with `.nav-group`, `.nav-item`, `.nav-num`, `.nav-dot`
- Sections: `<section class="section" id="...">` or `<div class="section" id="...">`
- Section headers: `<div class="section-header"><span class="section-num">NUM</span><h2 class="st">Title</h2></div>`
- Section numbers appear LEFT of titles (flex layout), not above
- Hero section: `.hero` > `.hero-tag` + `<h1>` + `.hero-sub` + `.hero-meta`

### Numbering Convention
- 00: Introduction (hero)
- 01: Abstract
- 02: Problem Statement
- 03: Methodology
- Q1–Qn: Question sections
- Appendix sections use sequential numbers after questions (e.g., 08, 09, 10 or 14, 15, 16)

### Methodology Section (match uuv_rl_navigation)
Four-step `.meth-grid` with `.meth-card` items:
1. ① State of the Art — survey techniques with comparison table + citations
2. ② Comparative Analysis — pros/cons, CHOSEN box with justification
3. ③ Solution Planning — architecture, math, design decisions, diagrams
4. ④ Solution & Discussion — implementation, results, analysis

### Question Structure (within each Q section)
Each question follows the 4-step methodology:
- `<h3 class="sub">① State of the Art</h3>` with comparison table
- `<h3 class="sub">② Comparative Analysis & Selection</h3>` with CHOSEN box
- `<h3 class="sub">③ Solution Planning</h3>`
- `<h3 class="sub">④ Implementation / Solution</h3>`

### Bibliography Style
```html
<p style="color:var(--text2);font-size:14px;margin-bottom:16px">The following references...</p>
<div>
  <div class="bib-item" id="ref-N">
    <div class="bib-num"><a href="#ref-N" class="bib-link">[N]</a></div>
    <div class="bib-text">Author, <em>Title</em>, Publication, Year.</div>
  </div>
</div>
<hr class="div">
<p style="text-align:center;...">Report footer</p>
```

### CSS Color Scheme (dark theme)
- `--cyan` for primary accent, section nums, active nav
- `--violet` for question section nums (`.section-num.v`)
- `--green` for selected/positive items (`.section-num.g`)
- `--amber` for warnings/special sections (`.section-num.a`)

### Interactive Reports (Pyodide)
- Python scripts in `<script type="text/x-python" id="src-qN">` tags
- Each script is self-contained (no cross-script imports)
- Topology visualization: `draw_ascii_topology()` or `draw_topology()` showing node IDs at physical positions
- Runtime badge toggles between "Loading Python..." and "Ready"

## User Preferences
- No unnecessary emojis
- Match reference file styles exactly — consistency is critical
- When user gives feedback as a list, address every item systematically
- Content accuracy questions should be flagged rather than silently changed
- Keep reports self-contained (single HTML file)
