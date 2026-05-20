# Controlled Chaos v2 — Out of the Box Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Upgrade the controlledchaos skill so it produces SMB-tech-grade landing pages out of the box. Ship 10 new section partials, a universal interaction-states polish layer, an image-placeholder upgrade, premium motion (scroll-progress + smooth-scroll), and full SEO/OG/JSON-LD template scaffolding. Re-render SF Vibe Fest with the new library as the proof point. Keep Lovetel and all five existing showcase pages identical (backwards compatibility).

**Architecture:** Additive. Every new file is new; every modified file gets appended-to (not restructured). The polish layer is a new `tokens/states.css` loaded after `tokens/textures.css` — its rules win on cascade tie-breaks. New section partials follow the same authoring contract as the existing 12. `style.json` schema is unchanged (decision locked in spec); content fills inline.

**Tech Stack:** Pure HTML5 + CSS custom properties + native `<details>` for accordions. No JS framework. Minimal vanilla JS (≤30 lines total across nav drawer / accordion / cta-banner dismiss). Google Fonts via existing `<link>`. `color-mix()` (Baseline 2023). `animation-timeline: scroll()` (Chrome/Edge stable; `@supports` fallback for Firefox).

**Spec:** [docs/superpowers/specs/2026-05-20-controlled-chaos-v2-out-of-the-box-design.md](../specs/2026-05-20-controlled-chaos-v2-out-of-the-box-design.md)

---

## Read Order

This plan is split into parts. Read in order:

1. **This file** — file structure, task list, verification, execution handoff
2. **[part-1-foundation.md](2026-05-20-part-1-foundation.md)** — Tasks 1–3: `tokens/states.css`, image-placeholder + premium motion in `sections.css`, `templates/page-composed.html` head upgrade
3. **[part-2-templates.md](2026-05-20-part-2-templates.md)** — Tasks 4–7: `meta.partial.html`, `404.html`, `privacy.html`, `terms.html`
4. **[part-3-sections-conversion.md](2026-05-20-part-3-sections-conversion.md)** — Tasks 8–11: `nav`, `feature-grid`, `product-showcase`, `pricing` (the conversion-critical four)
5. **[part-4-sections-trust.md](2026-05-20-part-4-sections-trust.md)** — Tasks 12–15: `faq`, `testimonial`, `signup-form`, `integration-logos` (the social-proof + trust four)
6. **[part-5-sections-utility.md](2026-05-20-part-5-sections-utility.md)** — Tasks 16–17: `cta-banner`, `comparison-table`
7. **[part-6-catalog-showcase-docs.md](2026-05-20-part-6-catalog-showcase-docs.md)** — Tasks 18–23: `icons.md`, catalog update, SF Vibe Fest 15-section re-render, docs updates, backwards-compat verification, push

---

## File Structure

### Files created

```
tokens/states.css                    # Task 1 — interaction states
sections/icons.md                    # Task 18 — reference SVG snippets
sections/nav.html                    # Task 8
sections/feature-grid.html           # Task 9
sections/product-showcase.html       # Task 10
sections/pricing.html                # Task 11
sections/faq.html                    # Task 12
sections/testimonial.html            # Task 13
sections/signup-form.html            # Task 14
sections/integration-logos.html      # Task 15
sections/cta-banner.html             # Task 16
sections/comparison-table.html       # Task 17
templates/meta.partial.html          # Task 4
templates/404.html                   # Task 5
templates/privacy.html               # Task 6
templates/terms.html                 # Task 7
```

### Files modified

```
sections/sections.css                # Task 2 + every section task (rules appended)
sections/README.md                   # Task 19 — catalog grows from 12 to 22 entries
templates/page-composed.html         # Task 3 — head + link chain
showcase/test-sf-vibe-fest.html      # Task 20 — re-rendered to 15 sections
SKILL.md                             # Task 21
README.md                            # Task 21
CLAUDE.md                            # Task 21
showcase/index.html                  # Task 21 — adds any new showcase links if needed
```

### Final `<link>` chain in `templates/page-composed.html`

```html
<link rel="stylesheet" href="../tokens/layout.css">
<link rel="stylesheet" href="../tokens/typography.css">
<link rel="stylesheet" href="../tokens/color.css">
<link rel="stylesheet" href="../tokens/motion.css">
<link rel="stylesheet" href="../tokens/forces.css">
<link rel="stylesheet" href="../tokens/textures.css">
<link rel="stylesheet" href="../tokens/states.css">         <!-- NEW (Task 1) -->
<link rel="stylesheet" href="../sections/sections.css">
```

`states.css` loads after `textures.css` so `:hover`, `:focus-visible`, `:active`, `:disabled`, and `:invalid` rules can override base section styles.

---

## Task Summary

| # | Task | Output |
|---|---|---|
| 1 | `tokens/states.css` | Interaction states: hover, focus-visible, active, disabled, loading, touch-targets, reduced-motion |
| 2 | Image-placeholder + premium motion in `sections.css` | Gradient+grain placeholder; scroll-progress hairline (`@supports` fallback); smooth-scroll with nav offset |
| 3 | `templates/page-composed.html` head | Full SEO/OG/Twitter/JSON-LD meta; new `<link>` chain order |
| 4 | `templates/meta.partial.html` | Standalone copy-pasteable head meta stack |
| 5 | `templates/404.html` | Style-driven 404 — hero + cta + footer |
| 6 | `templates/privacy.html` | Editorial privacy-policy scaffold |
| 7 | `templates/terms.html` | Editorial terms-of-service scaffold |
| 8 | `sections/nav.html` | Sticky top nav + mobile drawer (default / centered / minimal variants) |
| 9 | `sections/feature-grid.html` | 3/4/2-col icon+heading+body grid (default / wide / compact) |
| 10 | `sections/product-showcase.html` | Browser-framed mockup with callouts (split / centered / callouts) |
| 11 | `sections/pricing.html` | Tier comparison (cards / table) |
| 12 | `sections/faq.html` | Native `<details>` accordion (accordion / static / numbered) |
| 13 | `sections/testimonial.html` | Quote + monogram-avatar (card / grid / featured) |
| 14 | `sections/signup-form.html` | Accessible email form (inline / stacked / framed) |
| 15 | `sections/integration-logos.html` | Tech-stack grid (grid / inline / categorized) |
| 16 | `sections/cta-banner.html` | Inline + sticky-bottom dismissible CTA |
| 17 | `sections/comparison-table.html` | Feature matrix vs competitors (vs-2 / vs-3 / vs-many) |
| 18 | `sections/icons.md` | 12-15 reference inline SVG snippets |
| 19 | `sections/README.md` catalog update | 22 entries |
| 20 | SF Vibe Fest re-render | 15-section layout exercising every new component |
| 21 | `SKILL.md`, `README.md`, `CLAUDE.md` updates | New triggers + new tables + SMB-baseline note |
| 22 | Backwards-compat verification | Lovetel + 5 existing showcase pages render identically |
| 23 | Push to all three remotes | `git pushall` after explicit user authorization |

---

## Verification (whole-plan acceptance)

After all 23 tasks ship, the implementation is complete when:

1. **All 10 new section partials render** — open `showcase/test-sf-vibe-fest.html`; every new section appears, no `{{…}}` placeholders leak, no console errors.
2. **Polish layer active** — every interactive element shows hover transform, focus ring on Tab, active press, disabled state. 44px touch targets verified via DevTools.
3. **Scroll-progress hairline** — visible orange bar at the top of viewport, fills as page scrolls (in Chrome/Edge; absent in Firefox-stable, graceful).
4. **Image-placeholder upgrade** — old flat-gray gone; new gradient+grain with bottom-left label visible everywhere `cc-img-placeholder` appears.
5. **SF Vibe Fest re-render** — 15 sections, browser-verified at desktop and mobile widths. Lovetel renders identically to its committed state.
6. **A11y baseline** — Tab-cycle navigates every interactive element with visible focus ring; `prefers-reduced-motion` disables progress bar + scroll smoothing.
7. **Backwards-compat** — five existing showcase pages (swiss/brutalist/immersive/kinetic/gallery) render unchanged.
8. **Schema validates** — `adapters/examples/lovetel.json` and `sf-vibe-fest.json` still parse against `style-schema.json`.
9. **Pushed to GitHub** — `git pushall` ran successfully to `nayslayer143/controlledchaos`.

---

## Execution Handoff

**Plan complete. Two execution options:**

**1. Subagent-Driven (recommended)** — Dispatch a fresh subagent per task, review between tasks, fast iteration. Best for a 23-task plan with self-contained units.

**2. Inline Execution** — Execute tasks in this session using `executing-plans`, batch execution with checkpoints. You see every step as it happens.

**Which approach?**
