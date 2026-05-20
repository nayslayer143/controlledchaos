# Section Library + Style-Guide Adapter Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship a 12-section reusable HTML partial library and a `style.json` adapter that lets external style-guide apps drive a Controlled Chaos page's palette / typography / forces / motion / texture — without touching existing token files or breaking any existing CC page.

**Architecture:** Additive. New folders `sections/` (partials + their CSS) and `adapters/` (schema + workflow doc + examples). New token file `tokens/textures.css`. New template `templates/page-composed.html`. Two new showcase pages. Three documentation file updates. Existing token files, the five aesthetic modes, and the nine moods stay untouched.

**Tech Stack:** Pure HTML5 + CSS custom properties. No JS runtime. No build step. No package manager. Google Fonts via `<link>` (already in use). `color-mix()` for runtime-derived colors (Baseline 2023).

**Spec:** [docs/superpowers/specs/2026-05-20-section-library-and-style-guide-adapter-design.md](../specs/2026-05-20-section-library-and-style-guide-adapter-design.md)

---

## Read Order

This plan is split into parts. Read in order:

1. **This file** — file structure, task list, verification, execution handoff
2. **[part-1-foundation.md](2026-05-20-part-1-foundation.md)** — Tasks 1–3: textures.css, sections.css scaffold, page-composed template
3. **[part-2-sections-text.md](2026-05-20-part-2-sections-text.md)** — Tasks 4–8: hero, manifesto, quote, two-column, cta
4. **[part-3-sections-data.md](2026-05-20-part-3-sections-data.md)** — Tasks 9–12: stat-grid, process, footer, logo-wall
5. **[part-4-sections-visual.md](2026-05-20-part-4-sections-visual.md)** — Tasks 13–15: gallery, full-bleed, marquee
6. **[part-5-catalog-adapter.md](2026-05-20-part-5-catalog-adapter.md)** — Tasks 16–19: sections/README, schema, workflow doc, examples
7. **[part-6-showcase-docs.md](2026-05-20-part-6-showcase-docs.md)** — Tasks 20–23: showcase pages, docs, backwards-compat verification

---

## File Structure

### Files created

```
tokens/textures.css                        # Task 1 — grain + glass textures
sections/sections.css                      # Task 2 — section-specific CSS, written incrementally
sections/hero.html                         # Task 4
sections/manifesto.html                    # Task 5
sections/quote.html                        # Task 6
sections/two-column.html                   # Task 7
sections/cta.html                          # Task 8
sections/stat-grid.html                    # Task 9
sections/process.html                      # Task 10
sections/footer.html                       # Task 11
sections/logo-wall.html                    # Task 12
sections/gallery.html                      # Task 13
sections/full-bleed.html                   # Task 14
sections/marquee.html                      # Task 15
sections/README.md                         # Task 16 — catalog index
templates/page-composed.html               # Task 3 — richer scaffold
adapters/style-schema.json                 # Task 17 — JSON Schema
adapters/from-style-json.md                # Task 18 — agent workflow
adapters/README.md                         # Task 18 — adapter overview
adapters/examples/minimal.json             # Task 19
adapters/examples/dusk.json                # Task 19
adapters/examples/signal.json              # Task 19
showcase/sections-demo.html                # Task 20 — all 12 sections
showcase/style-driven.html                 # Task 21 — driven by dusk.json
```

### Files modified

```
SKILL.md                                   # Task 22 — new workflow path, expanded triggers
README.md                                  # Task 22 — section catalog + schema + texture tables
CLAUDE.md                                  # Task 22 — two new guidance bullets
showcase/index.html                        # Task 22 — links to new showcase pages
```

### Spec amendment (one decision not in the original spec)

The spec doesn't say where the section-specific CSS (the `.cc-stat-grid`, `.cc-marquee`, etc. classes) lives. Decision: **`sections/sections.css`**, co-located with the partials, linked AFTER `tokens/textures.css` in the load order. This keeps `tokens/` as the layer of design primitives and `sections/` self-contained.

### Final `<link>` chain (in templates/page-composed.html)

```html
<link rel="stylesheet" href="tokens/layout.css">
<link rel="stylesheet" href="tokens/typography.css">
<link rel="stylesheet" href="tokens/color.css">
<link rel="stylesheet" href="tokens/motion.css">
<link rel="stylesheet" href="tokens/forces.css">
<link rel="stylesheet" href="tokens/textures.css">     <!-- NEW -->
<link rel="stylesheet" href="sections/sections.css">   <!-- NEW -->
```

`tokens/textures.css` and `sections/sections.css` are loaded last so they can override anything in the token layer when needed.

---

## Task Summary

| # | Task | Output |
|---|---|---|
| 1 | Texture token file | `tokens/textures.css` (grain + glass + reduced-motion) |
| 2 | Section CSS scaffold | `sections/sections.css` with `.cc-section` base |
| 3 | Page-composed template | `templates/page-composed.html` with new `<link>` chain + section composition pattern |
| 4 | `hero` section | partial + CSS + demo entry |
| 5 | `manifesto` section | partial + CSS + demo entry |
| 6 | `quote` section | partial + CSS + demo entry |
| 7 | `two-column` section | partial + CSS + demo entry |
| 8 | `cta` section | partial + CSS + demo entry |
| 9 | `stat-grid` section | partial + CSS + demo entry |
| 10 | `process` section | partial + CSS + demo entry |
| 11 | `footer` section | partial + CSS + demo entry |
| 12 | `logo-wall` section | partial + CSS + demo entry |
| 13 | `gallery` section | partial + CSS + demo entry |
| 14 | `full-bleed` section | partial + CSS + demo entry |
| 15 | `marquee` section | partial + CSS + demo entry |
| 16 | Section catalog | `sections/README.md` |
| 17 | Adapter schema | `adapters/style-schema.json` |
| 18 | Adapter workflow doc | `adapters/from-style-json.md`, `adapters/README.md` |
| 19 | Adapter examples | `adapters/examples/{minimal,dusk,signal}.json` |
| 20 | Sections showcase | `showcase/sections-demo.html` |
| 21 | Style-driven showcase | `showcase/style-driven.html` |
| 22 | Documentation updates | `SKILL.md`, `README.md`, `CLAUDE.md`, `showcase/index.html` |
| 23 | Backwards-compat verification | Visual check of `showcase/swiss.html` (and one other) against pre-change render |

---

## Verification (whole-plan acceptance)

After all 23 tasks ship, the implementation is complete when:

1. **All 12 section partials render** — open `showcase/sections-demo.html` in a browser; every section appears, no `{{…}}` placeholders leak through, no console errors.
2. **`style.json`-driven page renders** — open `showcase/style-driven.html`; palette / fonts / forces / motion / texture from `adapters/examples/dusk.json` are visibly applied.
3. **Backwards compatibility** — open `showcase/swiss.html`; renders identically to pre-change state (no unexpected font shift, layout drift, or color change).
4. **Reduced motion** — toggle macOS System Settings → Accessibility → Display → Reduce Motion ON; reload `showcase/sections-demo.html` with the marquee section in view; marquee scroll halts; grain texture's drift halts.
5. **Schema validates** — pasting `adapters/examples/dusk.json` into any online JSON-Schema validator against `adapters/style-schema.json` passes.

---

## Execution Handoff

**Plan complete. Two execution options:**

**1. Subagent-Driven (recommended)** — I dispatch a fresh subagent per task, review between tasks, fast iteration. Best for a plan this size with 23 self-contained tasks.

**2. Inline Execution** — Execute tasks in this session using executing-plans, batch execution with checkpoints. Best if you want to drive each task interactively.

**Which approach?**
