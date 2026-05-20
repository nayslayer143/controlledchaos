# Controlled Chaos — Section Library + Style-Guide Adapter

**Status:** Draft for review
**Date:** 2026-05-20
**Scope:** Phase 1 of the controlledchaos presentation upgrade
**Out of scope:** Multi-slide deck format (Phase 2, separate spec)

---

## Overview

This spec expands the `controlledchaos` skill (`~/.claude/skills/controlledchaos/`) along two axes without breaking any existing behavior:

1. **A 12-section reusable HTML partial library** that composes under any existing mode/mood — turning 5 modes × 1 showcase layout each into thousands of distinct compositions.
2. **A `style.json` adapter** that lets external style-guide apps (SuperStyleGuide, Pinterest Style Engine, hand-authored) drive a Controlled Chaos page's palette, typography, force values, motion, and texture without modifying the token files.

Both pieces are additive. Existing CC pages, the five `<link>` token files, the `data-mood` / `data-forces` attributes, and the three-decision workflow (mode + mood + voices) remain untouched. Pure HTML + CSS — no build step, no JS framework.

**Out of scope (deferred):**
- Multi-slide / deck format with keyboard nav and PDF export — Phase 2
- New aesthetic modes beyond the existing 5 (Swiss / Brutalist / Immersive / Kinetic / Gallery)
- Composable forces as a primary affordance (covered by per-page `forces` override in `style.json`)
- Image fields in `style.json` (style-guide source images are reference-only by user decision; see Memory note)
- JSON-schema render pipeline (parked; HTML partials cover v1 needs)
- SSG-side / PSE-side exporters (their authors target `adapters/style-schema.json` separately)

---

## Architecture

### File layout (additive — no renames, no moves)

```
~/.claude/skills/controlledchaos/
├── SKILL.md                ← MODIFIED — new workflow path, expanded triggers
├── README.md               ← MODIFIED — section catalog + schema ref + texture options
├── CLAUDE.md               ← MODIFIED — two new guidance bullets
│
├── sections/               ← NEW
│   ├── README.md           ← catalog index + per-section authoring guide
│   ├── hero.html
│   ├── manifesto.html
│   ├── stat-grid.html
│   ├── quote.html
│   ├── gallery.html
│   ├── two-column.html
│   ├── full-bleed.html
│   ├── process.html
│   ├── cta.html
│   ├── footer.html
│   ├── marquee.html
│   └── logo-wall.html
│
├── adapters/               ← NEW
│   ├── README.md           ← how to author / consume style.json
│   ├── style-schema.json   ← JSON Schema (validation reference for SSG/PSE)
│   ├── from-style-json.md  ← agent-readable workflow (read on style-driven runs)
│   └── examples/
│       ├── dusk.json       ← rich example (warm, editorial)
│       ├── signal.json     ← rich example (neon, kinetic)
│       └── minimal.json    ← bare-minimum example (name + palette only)
│
├── tokens/
│   ├── layout.css          ← UNCHANGED
│   ├── typography.css      ← UNCHANGED
│   ├── color.css           ← UNCHANGED
│   ├── motion.css          ← UNCHANGED (grain-shift keyframe already exists at L163)
│   ├── forces.css          ← UNCHANGED
│   └── textures.css        ← NEW (6th token file — grain / glass / none)
│
├── templates/
│   ├── page.html           ← UNCHANGED (still works for quick scaffolds)
│   └── page-composed.html  ← NEW (richer scaffold demonstrating section + style.json usage)
│
└── showcase/
    ├── index.html          ← MODIFIED — link to new showcase pages
    ├── swiss.html          ← UNCHANGED
    ├── brutalist.html      ← UNCHANGED
    ├── immersive.html      ← UNCHANGED
    ├── kinetic.html        ← UNCHANGED
    ├── gallery.html        ← UNCHANGED
    ├── sections-demo.html  ← NEW (all 12 sections, multiple modes)
    └── style-driven.html   ← NEW (driven by adapters/examples/dusk.json)
```

### Compatibility guarantee

Every existing CC artifact behaves identically after this change. The new behavior is opt-in: a page becomes "style-driven" only when the agent consults `style.json`; a page uses sections only when the agent picks them from `sections/`. A user running `controlledchaos` with the same prompt they used a week ago gets the same output.

---

## The Section Library

### Placeholder convention

Mustache, two forms only:

| Form | Use | Example |
|---|---|---|
| `{{name}}` | Scalar substitution | `<h2>{{heading}}</h2>` |
| `{{#items}}…{{/items}}` | Repeat block | `{{#stats}}<div>{{num}}</div>{{/stats}}` |

No Mustache runtime — the agent fills placeholders by string substitution. The syntax is just an unambiguous marker the agent recognizes.

### Section root attributes

Every section's root element carries:

```html
<section class="cc-section cc-<name>"
         data-pattern="<name>"
         data-variant="<variant>">
```

- `data-pattern` — names the section for agent/tooling discovery
- `data-variant` — selects between 1–3 documented layout variants per section
- `cc-section` is a shared base class providing default rhythm/spacing
- `cc-<name>` is the section-specific CSS hook

### Mode compatibility (advisory)

Each partial begins with a top-comment describing best/ok/avoid modes and available variants:

```html
<!-- cc-section: marquee
     best:  kinetic, immersive, brutalist
     ok:    swiss
     avoid: gallery (kills the silence)
     variants: default, slow, vertical -->
```

The agent reads this when picking sections. It is **guidance, not enforcement** — every CC rule exists to be broken by an author who understands why it exists (per `AXIOMS.md`).

### The 12 sections

| # | Section | `data-variant`s | Required content | Best modes |
|---|---|---|---|---|
| 1 | **hero** | `monument`, `editorial`, `bleed` | heading, subhead | all |
| 2 | **manifesto** | `default`, `numbered` | heading, paragraphs[] | swiss, brutalist, immersive |
| 3 | **stat-grid** | `default`, `bleed`, `vertical` | heading, stats[] (num, label, optional caption) | swiss, brutalist, kinetic |
| 4 | **quote** | `default`, `monument`, `gutter` | quote, attribution | all |
| 5 | **gallery** | `rigid`, `asymmetric`, `mosaic` | items[] (caption optional) | gallery, immersive, kinetic |
| 6 | **two-column** | `editorial`, `dialogue`, `before-after` | left{heading,body}, right{heading,body} | swiss, gallery |
| 7 | **full-bleed** | `type`, `image`, `color-field` | content (text or img placeholder) | immersive, brutalist, kinetic |
| 8 | **process** | `numbered`, `timeline`, `vertical` | heading, steps[] (title, body) | swiss, immersive |
| 9 | **cta** | `default`, `discord`, `monument` | heading, action_text, action_href | all |
| 10 | **footer** | `minimal`, `editorial`, `bleed` | copyright, links[] | all |
| 11 | **marquee** | `default`, `slow`, `vertical` | items[] | kinetic, immersive, brutalist |
| 12 | **logo-wall** | `grid`, `marquee`, `monolith` | logos[] (label, optional href) | swiss, brutalist |

**Combinatorial reach:** 12 sections × avg 2.5 variants ≈ 30 compositional patterns, applied across 5 modes × 9 moods → ~1,350 mode/mood/variant combinations before content even differs.

### Image handling

Sections with image slots (`hero.bleed`, `gallery`, `full-bleed.image`, `logo-wall`) use placeholder / agent-sourced / user-supplied content. **Never** pull images from a style guide's source board — style-guide source imagery is reference material only. Each such section's partial ships with a clear placeholder pattern:

```html
<div class="cc-img-placeholder"
     aria-label="Replace with image"
     data-slot="hero-image"></div>
```

### `sections/README.md`

A tabular catalog listing all 12 sections with: name, variants, required placeholders, best-fit modes, avoid-in modes, link to the partial file. This is the single source of truth the agent (and a human browsing) scans when selecting sections.

---

## The Style-Guide Adapter

### Schema (`adapters/style-schema.json`)

Final shape (JSON Schema in repo will encode types + required fields):

```json
{
  "name": "Dakota's bedroom",
  "thesis": "Liminal warmth at the edge of suburban dusk",

  "palette": {
    "bg":       "#1a0e08",
    "fg":       "#f4e8d0",
    "accent_1": "#d97a2c",
    "accent_2": "#7a5a3a",
    "discord":  "#22d3ee"
  },

  "typography": {
    "monument":   { "family": "Bodoni Moda",         "weights": [400, 900], "google": true },
    "swiss":      { "family": "Söhne",               "weights": [400, 600] },
    "editorial":  { "family": "Cormorant Garamond",  "weights": [400, 700], "google": true },
    "brutalist":  { "family": "JetBrains Mono",      "weights": [400, 700], "google": true },
    "expressive": { "family": "DM Sans",             "weights": [400, 700], "google": true }
  },

  "forces": {
    "structure": 0.7, "disruption": 0.3, "density": 0.4, "breath": 0.7,
    "edge": 0.5,      "warmth": 0.8,     "motion": 0.4,  "shout": 0.3,
    "stillness": 0.6, "whisper": 0.5
  },

  "motion_intensity": 0.5,
  "texture": "grain",

  "mode_id": "swiss",
  "mood_id": "dusk",

  "dos":   ["lean into warm grain", "type breathes"],
  "donts": ["pure black backgrounds", "neon discord"]
}
```

### Field tiers

- **Required:** `name` + one of (`palette` OR `mood_id`). Nothing else.
- **Recommended:** `typography` (the 5 voice mappings), `mode_id` for force preset.
- **Optional:** `forces` (numeric overrides), `motion_intensity`, `texture`, `thesis`, `dos`/`donts`, per-voice `weights` / `google` font-loader hints.

### Field semantics

- **`palette`** — hex codes that override CC's mood variables for this page. When provided, the adapter writes a CSS scope `[data-mood="--custom"] { … }` and the page's `<html>` sets `data-mood="--custom"`. Required keys: `bg`, `fg`, `accent_1`, `accent_2`, `discord`. Other CC mood vars (`muted`, `subtle`, `surface`, `border`) are derived (see Derived Color Values below).
- **`typography`** — font-family + weights per voice. When provided, the adapter emits `@font-face` declarations or Google Fonts `<link>` extensions and overrides `--font-monument` / `--font-swiss` / etc. variables.
- **`forces`** — explicit numeric overrides for `--force-*` custom properties. CC ships 10 force dials: `structure`, `disruption`, `density`, `breath`, `edge`, `warmth`, `motion`, `shout`, `stillness`, `whisper`. All 10 are valid keys in `forces`; missing keys inherit from whatever `mode_id` selected. `stillness` and `whisper` are rarely tuned in practice — they read as the inverse-leaning siblings of `motion` and `shout` but are independent dials in the CSS.
- **`motion_intensity`** — single 0–1 dial. Maps to scaling of motion durations (`--dur-*` token group) and grain animation speed. `0` = static, `1` = current kinetic defaults.
- **`texture`** — one of `"none"` (default) / `"grain"` / `"glass"`. Sets `data-texture` on `<html>`.
- **`mode_id` / `mood_id`** — when provided alone (without `forces` / `palette`), they act as lookup keys into existing presets. When provided alongside numeric overrides, the numeric values win field-by-field.
- **`dos` / `donts`** — agent-readable authoring guidance. Not rendered as CSS. The agent reads these before picking sections and writing content (e.g., a `donts: ["pure black backgrounds"]` steers the agent away from a `void` mood).

### Derived color values

When `palette` is provided but minimal, the adapter generates missing CC color variables by mixing `bg` and `fg`. Two implementation options — this spec mandates **option A (runtime CSS)** because it stays consistent with CC's pure-CSS philosophy and lets the page survive palette tweaks without re-running the agent:

**Option A (chosen): runtime CSS via `color-mix()`** — the emitted scope uses the standard CSS `color-mix()` function (Baseline 2023, broadly supported):

```css
[data-mood="--custom"] {
  --color-bg:       #1a0e08;
  --color-fg:       #f4e8d0;
  --color-accent-1: #d97a2c;
  --color-accent-2: #7a5a3a;
  --color-discord:  #22d3ee;
  /* Derived — computed by the browser, not the agent */
  --color-muted:   color-mix(in oklch, var(--color-fg) 50%, var(--color-bg));
  --color-subtle:  color-mix(in oklch, var(--color-fg)  8%, var(--color-bg));
  --color-surface: color-mix(in oklch, var(--color-fg)  5%, var(--color-bg));
  --color-border:  color-mix(in oklch, var(--color-fg) 15%, var(--color-bg));
  --color-inverse: var(--color-fg); /* approximation; full swap not feasible in pure CSS */
}
```

Option B (rejected): agent computes hex values at adapter-time and emits literal hex. Loses CSS-only re-tunability; bloats the emitted block; couples adapter to a color-math library.

Both options documented in `adapters/from-style-json.md` for completeness.

### Consumption workflow

When the agent receives a path to a `style.json`:

1. **Read** the file. Parse as JSON.
2. **Validate** — agent-driven, no JS runtime. The agent reads `adapters/style-schema.json` once for the field contract, then checks the user's `style.json` against it: required field presence (`name` + one of `palette` / `mood_id`), type sanity, enum membership for `texture` / `mode_id` / `mood_id`. Fail fast with a clear message naming the missing or invalid field.
3. **Pick base mode**:
   - If `mode_id` present → use as `data-forces` value.
   - Else if `forces` numerics present → find closest preset by L2 distance using the **Mode Force Profiles** table below.
   - Else default to `swiss`.
4. **Pick base mood**:
   - If `mood_id` present and no `palette` → use as `data-mood`, no scope emitted.
   - If `palette` present → emit `[data-mood="--custom"]` scope, set `data-mood="--custom"` on `<html>`.
5. **Emit inline `<style>` block** in `<head>` containing:
   - Custom mood scope (if palette)
   - Custom force overrides (if forces)
   - Voice font-family overrides (if typography)
   - Motion intensity scaling (if motion_intensity)
6. **Emit font links** for each `typography.*` entry with `google: true`. Extend the existing Google Fonts `<link>` in `templates/page-composed.html`.
7. **Set texture attribute**: `<html data-texture="<texture>">` if provided.
8. **Read** the brief and `dos` / `donts`.
9. **Read** `sections/README.md` for the catalog.
10. **Choose sections** matching the brief; respect `dos` / `donts`.
11. **Assemble**: for each chosen section, read its partial, fill placeholders, concatenate into the page `<body>`.
12. **Verify** in browser (see Verification below).

### Mode Force Profiles (lookup table for step 3)

When the user supplies numeric `forces` but no `mode_id`, the adapter picks the closest preset by L2 distance. Values mirror the existing CC presets in `tokens/forces.css` exactly:

| mode_id | structure | disruption | density | breath | warmth | edge | stillness | motion | whisper | shout |
|---|---|---|---|---|---|---|---|---|---|---|
| `swiss` | 0.85 | 0.15 | 0.40 | 0.65 | 0.10 | 0.85 | 0.80 | 0.10 | 0.50 | 0.40 |
| `brutalist` | 0.40 | 0.80 | 0.85 | 0.10 | 0.00 | 1.00 | 0.85 | 0.05 | 0.00 | 1.00 |
| `immersive` | 0.25 | 0.60 | 0.20 | 0.80 | 0.60 | 0.40 | 0.10 | 0.90 | 0.50 | 0.40 |
| `kinetic` | 0.55 | 0.45 | 0.40 | 0.50 | 0.30 | 0.65 | 0.00 | 1.00 | 0.30 | 0.70 |
| `gallery` | 0.90 | 0.05 | 0.10 | 1.00 | 0.30 | 0.60 | 0.80 | 0.20 | 0.90 | 0.05 |

L2 distance: `sqrt(Σ (user_force - preset_force)²)` across whichever dials the user actually supplied (missing dials excluded from the sum). Tie-breaker (rare): prefer `swiss`.

### Motion intensity scaling

`motion_intensity` (0–1) scales CC's existing motion duration tokens. The adapter emits a single override:

```css
:root {
  --motion-scale: <value>;   /* e.g., 0.5 */
  --dur-instant:  calc(150ms  * var(--motion-scale, 1));
  --dur-swift:    calc(300ms  * var(--motion-scale, 1));
  --dur-natural:  calc(500ms  * var(--motion-scale, 1));
  --dur-drift:    calc(900ms  * var(--motion-scale, 1));
  --dur-meditate: calc(1800ms * var(--motion-scale, 1));
  --texture-grain-duration: calc(8s * var(--motion-scale, 1));
}
```

Reference points: `motion_intensity: 0` → `--motion-scale: 0.01` (effectively static, but not zero — preserves `transition: ...` syntax validity); `0.5` → half speed; `1.0` → CC defaults. The adapter does NOT emit a `--motion-scale` of exactly `0` to avoid breaking CSS that assumes a non-zero duration.

### One-way contract

This spec defines `style.json` as a one-way schema: external tools emit, the skill consumes. SSG-side and PSE-side exporters that produce `style.json` are out of scope here — their authors target `adapters/style-schema.json` as the spec. Hand-authored `style.json` is a first-class use case.

---

## The Texture Mini-System

### `tokens/textures.css`

A 6th token file (~80 lines) shipping three textures driven by `<html data-texture="…">`:

| `data-texture` | Implementation | Effect |
|---|---|---|
| `none` (default) | No-op | Clean — current CC behavior preserved |
| `grain` | `body::after` overlay, inline SVG noise data-URI, `mix-blend-mode: overlay`, `animation: grain-shift 8s steps(8) infinite` | Subtle, living material grain across the page |
| `glass` | `.cc-glass` utility class with `backdrop-filter: blur(20px) saturate(180%)` | Frosted-glass panels — element-level opt-in |

**Reuse:** the `grain-shift` keyframe already exists in `motion.css` (line 163). The new `textures.css` wires it to `body::after[data-texture="grain"]`.

### Tunable custom properties

```css
:root {
  --texture-grain-opacity:  0.08;
  --texture-grain-scale:    0.85;
  --texture-grain-duration: 8s;
  --texture-glass-blur:     20px;
  --texture-glass-saturate: 180%;
}
```

These can be overridden inline from `style.json`'s `motion_intensity` (slower drift, lower opacity for calm pages).

### Page-level vs element-level

- **Grain** is page-level. `data-texture="grain"` on `<html>` activates the body overlay. No per-element opt-in needed.
- **Glass** is element-level. `data-texture="glass"` on `<html>` activates the `.cc-glass` utility *availability* — individual elements (cards, headers, overlays) opt in by adding `class="cc-glass"`. Applying glass page-wide destroys text legibility against blurred content underneath.

Both can be active simultaneously (page has grain overlay AND specific cards use glass).

### Accessibility

Textures ride CC's existing `prefers-reduced-motion: reduce` system handling (`motion.css` lines 300+). One additional rule in `textures.css` explicitly halts the grain drift (not just shrinks it to 0.01ms):

```css
@media (prefers-reduced-motion: reduce) {
  body[data-texture="grain"]::after {
    animation: none;
  }
}
```

Glass has no animation — no special handling needed.

`<link>` load order: after `forces.css`, last:

```html
<link rel="stylesheet" href="tokens/layout.css">
<link rel="stylesheet" href="tokens/typography.css">
<link rel="stylesheet" href="tokens/color.css">
<link rel="stylesheet" href="tokens/motion.css">
<link rel="stylesheet" href="tokens/forces.css">
<link rel="stylesheet" href="tokens/textures.css">  ← new
```

---

## Workflow + Documentation Updates

### SKILL.md

Add to **When to Use** triggers:
- *"build a presentation/landing page with style.json"*
- *"use [path/to/style.json] for the design"*
- *"compose using sections"* / *"use the section library"*
- *"add a stat grid"* / *"add a manifesto"* / any single section name from the catalog

Add a new **Style-Driven Authoring** workflow path (sibling to the existing **Three-Decision Recipe**):

```
1. Read adapters/from-style-json.md     (workflow guide)
2. Read the user-supplied style.json    (parse + light validate)
3. Pick mode_id / mood_id (or custom scope from palette)
4. Read sections/README.md              (section catalog)
5. Choose sections matching the brief + dos/donts
6. Scaffold templates/page-composed.html
7. Read each chosen section partial, fill placeholders, concat into <body>
8. Emit inline <style> block with custom scope (palette, forces, voices, textures)
9. Emit @font-face / Google Fonts links for typography entries
10. Open in browser, verify against the style guide's thesis
```

The existing three-decision workflow stays as the default for prompts without a style guide.

### README.md

Add three new reference tables (parallel to existing mode/mood/voice tables):
- **Section catalog** — 12 sections + variants + best-fit modes (mirrors `sections/README.md`)
- **Style.json schema** — required vs optional fields, types, link to examples
- **Texture options** — `none` / `grain` / `glass` + tunable custom properties

### CLAUDE.md

Two new bullets under "How to Use This System":
- *Compose pages from the 12-section library in `sections/`. Each section is a mode-agnostic partial — token cascade handles per-mode look.*
- *If a `style.json` is provided, read `adapters/from-style-json.md` for the consumption workflow. Style-guide source images are reference-only — never embed them as page imagery.*

---

## Verification

Every generated page must pass three checks before the agent reports "done":

1. **Renders in a browser** — open the page; visually confirm hero / sections match the brief.
2. **`style.json` applied (when used)** — confirm custom palette / voices / forces are visible (bg color matches `palette.bg`; hero font matches `typography.monument.family`; etc.).
3. **No console errors** — open dev tools; check for missing fonts, unresolved `{{…}}` placeholders, broken `data-pattern` attributes.

When the agent has `preview_*` tools available it drives these checks directly (start preview server, snapshot, network, inspect). When running in a context without preview tools (a CC user without that MCP), the SKILL.md instructs the agent to ask the user to verify and screenshot back.

---

## Acceptance Criteria

This spec is complete when the implementation ships:

1. **Section library** — all 12 partials present in `sections/`, each with required content slots, `data-pattern` + `data-variant` attributes, mode-compatibility top-comment, and at least the default variant rendering correctly across all 5 modes × 9 moods.
2. **Section catalog** — `sections/README.md` lists every section with its variants, required placeholders, best-fit modes, avoid-in modes, link to the partial.
3. **Style-guide adapter** — `adapters/style-schema.json` (machine-readable), `adapters/from-style-json.md` (agent-readable workflow), 3 example `style.json` files (`dusk`, `signal`, `minimal`) that validate.
4. **Texture system** — `tokens/textures.css` shipping `none` / `grain` / `glass`, with reduced-motion halt rule, and updated `<link>` chain in both templates.
5. **Templates** — `templates/page-composed.html` ships as a richer scaffold demonstrating section composition and `style.json` consumption hooks.
6. **Showcase** — `showcase/sections-demo.html` demonstrates all 12 sections across at least 3 modes; `showcase/style-driven.html` runs from `adapters/examples/dusk.json`.
7. **Documentation** — `SKILL.md`, `README.md`, `CLAUDE.md` updated per Workflow section above. Existing language untouched except where this spec calls out changes.
8. **Backwards-compatibility test** — at least one existing showcase page (e.g., `showcase/swiss.html`) opens and renders identically to its pre-change state.

---

## Open Questions for Implementation

None outstanding at spec time. Anything that surfaces during implementation should be raised before merging.

---

## Related

- `~/.claude/skills/controlledchaos/AXIOMS.md` — design first principles (hierarchy / tension / time)
- `~/.claude/skills/controlledchaos/DESIGN.md` — original system overview (modes / moods / voices)
- `~/.claude/projects/-Users-nayslayer/memory/project_controlledchaos_style_guides.md` — locked decision: style-guide source images are reference-only, never embedded
- SuperStyleGuide repo — produces structured `GuideAtoms` that an SSG-side exporter would convert to `style.json`
- Pinterest Style Engine — already registered as MCP; an MCP tool or CLI export would emit `style.json`
