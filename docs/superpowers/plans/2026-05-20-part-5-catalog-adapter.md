# Part 5: Catalog + Adapter (Tasks 16–19)

Prev: [part-4-sections-visual.md](2026-05-20-part-4-sections-visual.md). Next: [part-6-showcase-docs.md](2026-05-20-part-6-showcase-docs.md).

---

## Task 16: Section catalog

**Files:**
- Create: `sections/README.md`

- [ ] **Step 1: Write `sections/README.md`**

```markdown
# Section Library

Reusable HTML partials. Each section is a single `.html` file with Mustache
placeholders the agent fills via string substitution. No runtime, no JS.

## Authoring contract

Every partial:
- Begins with a top-comment listing best/ok/avoid modes + variants + placeholders
- Root element carries `class="cc-section cc-<name>"`, `data-pattern="<name>"`, `data-variant="<default>"`
- Uses Mustache: `{{scalar}}` for substitution, `{{#items}}…{{/items}}` for repeats
- Image slots use `<div class="cc-img-placeholder" data-slot="<name>"></div>`
- Section-specific CSS lives in `sections/sections.css`

## Catalog

| Section | Variants | Required placeholders | Best modes | Avoid in |
|---|---|---|---|---|
| [hero](hero.html) | monument, editorial, bleed | heading, subhead, (optional) eyebrow | all | — |
| [manifesto](manifesto.html) | default, numbered | heading, paragraphs[] (text) | swiss, brutalist, immersive | gallery |
| [quote](quote.html) | default, monument, gutter | quote, attribution, (optional) source | all | — |
| [two-column](two-column.html) | editorial, dialogue, before-after | left{heading,body}, right{heading,body} | swiss, gallery | kinetic |
| [cta](cta.html) | default, discord, monument | heading, action_text, action_href, (optional) supporting | all | — |
| [stat-grid](stat-grid.html) | default, bleed, vertical | heading, stats[] (num, label, optional caption) | swiss, brutalist, kinetic | gallery |
| [process](process.html) | numbered, timeline, vertical | heading, steps[] (title, body) | swiss, immersive | gallery |
| [footer](footer.html) | minimal, editorial, bleed | copyright, links[] (label, href) | all | — |
| [logo-wall](logo-wall.html) | grid, marquee, monolith | heading, logos[] (label, optional href) | swiss, brutalist | kinetic |
| [gallery](gallery.html) | rigid, asymmetric, mosaic | heading, items[] (slot_name, optional caption) | gallery, immersive, kinetic | brutalist |
| [full-bleed](full-bleed.html) | type, image, color-field | type → content; image → slot_name; color-field → content, optional --bleed-color | immersive, brutalist, kinetic | — |
| [marquee](marquee.html) | default, slow, vertical | items[] (text) | kinetic, immersive, brutalist | gallery |

## Image policy

Sections with image slots (`hero.bleed`, `gallery`, `full-bleed.image`, `logo-wall`) use
placeholder / agent-sourced / user-supplied content. **Never** pull images from a
style-guide source board — style-guide imagery is reference-only.

## Use from a template

Read `templates/page-composed.html` for a complete scaffold. The recommended order
for a typical page is:

    hero → manifesto / stat-grid / quote / two-column / process / gallery / full-bleed / marquee
         → cta → footer

For style.json-driven authoring, honor `dos[]` / `donts[]` when picking sections and
variants — see `adapters/from-style-json.md`.
```

- [ ] **Step 2: Commit**

```bash
git add sections/README.md
git commit -m "Add sections/README.md catalog index"
```

---

## Task 17: Adapter JSON Schema

**Files:**
- Create: `adapters/style-schema.json`

- [ ] **Step 1: Write `adapters/style-schema.json`**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://controlledchaos.local/adapters/style-schema.json",
  "title": "Controlled Chaos style.json",
  "type": "object",
  "required": ["name"],
  "anyOf": [
    { "required": ["palette"] },
    { "required": ["mood_id"] }
  ],
  "additionalProperties": false,
  "properties": {
    "name": { "type": "string", "minLength": 1 },
    "thesis": { "type": "string" },
    "palette": {
      "type": "object",
      "required": ["bg", "fg"],
      "additionalProperties": false,
      "properties": {
        "bg":       { "type": "string", "pattern": "^#[0-9a-fA-F]{3,8}$" },
        "fg":       { "type": "string", "pattern": "^#[0-9a-fA-F]{3,8}$" },
        "accent_1": { "type": "string", "pattern": "^#[0-9a-fA-F]{3,8}$" },
        "accent_2": { "type": "string", "pattern": "^#[0-9a-fA-F]{3,8}$" },
        "discord":  { "type": "string", "pattern": "^#[0-9a-fA-F]{3,8}$" }
      }
    },
    "typography": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "monument":   { "$ref": "#/$defs/voice" },
        "swiss":      { "$ref": "#/$defs/voice" },
        "editorial":  { "$ref": "#/$defs/voice" },
        "brutalist":  { "$ref": "#/$defs/voice" },
        "expressive": { "$ref": "#/$defs/voice" }
      }
    },
    "forces": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "structure":  { "$ref": "#/$defs/unitDial" },
        "disruption": { "$ref": "#/$defs/unitDial" },
        "density":    { "$ref": "#/$defs/unitDial" },
        "breath":     { "$ref": "#/$defs/unitDial" },
        "edge":       { "$ref": "#/$defs/unitDial" },
        "warmth":     { "$ref": "#/$defs/unitDial" },
        "motion":     { "$ref": "#/$defs/unitDial" },
        "shout":      { "$ref": "#/$defs/unitDial" },
        "stillness":  { "$ref": "#/$defs/unitDial" },
        "whisper":    { "$ref": "#/$defs/unitDial" }
      }
    },
    "motion_intensity": { "$ref": "#/$defs/unitDial" },
    "texture": { "enum": ["none", "grain", "glass"] },
    "mode_id": { "enum": ["swiss", "brutalist", "immersive", "kinetic", "gallery"] },
    "mood_id": { "enum": ["void", "void-inverse", "dusk", "signal", "earth", "frost", "blaze", "bruise", "concrete"] },
    "dos":   { "type": "array", "items": { "type": "string" } },
    "donts": { "type": "array", "items": { "type": "string" } }
  },
  "$defs": {
    "unitDial": { "type": "number", "minimum": 0, "maximum": 1 },
    "voice": {
      "type": "object",
      "required": ["family"],
      "additionalProperties": false,
      "properties": {
        "family":  { "type": "string", "minLength": 1 },
        "weights": { "type": "array", "items": { "type": "integer", "minimum": 100, "maximum": 1000 } },
        "google":  { "type": "boolean" }
      }
    }
  }
}
```

- [ ] **Step 2: Verify the schema is valid JSON**

Run: `python3 -c "import json; json.load(open('adapters/style-schema.json'))" && echo OK`

Expected: `OK`. (No need for a schema validator at this step — Task 19 verifies real example files parse against this schema.)

- [ ] **Step 3: Commit**

```bash
git add adapters/style-schema.json
git commit -m "Add adapters/style-schema.json — JSON Schema for style.json"
```

---

## Task 18: Adapter workflow + overview docs

**Files:**
- Create: `adapters/README.md`
- Create: `adapters/from-style-json.md`

- [ ] **Step 1: Write `adapters/README.md`**

```markdown
# Style-Guide Adapters

Style guides describe a vibe. Controlled Chaos renders it. The bridge is `style.json`.

This folder is the *contract layer*:

| File | Purpose |
|---|---|
| `style-schema.json` | Machine-readable JSON Schema. SSG/PSE exporters target this. |
| `from-style-json.md` | Agent-readable workflow. Read this when consuming a `style.json`. |
| `examples/minimal.json` | Bare minimum that validates (name + palette only). |
| `examples/dusk.json` | Rich example: editorial warmth, full palette + typography + forces. |
| `examples/signal.json` | Rich example: neon kinetic, motion-heavy, grain texture. |

## Direction

One-way: external tools emit `style.json`; this skill consumes it. SSG-side and
PSE-side exporters live in their own repos and target `style-schema.json` as the
spec.

## Image policy

Style guides describe a vibe — they do not supply page imagery. The schema
deliberately omits image fields. Source-board images are reference-only.
```

- [ ] **Step 2: Write `adapters/from-style-json.md`**

```markdown
# Consuming `style.json`

Workflow for the agent when the user hands over a `style.json` (path or content).

## 1. Read and parse

Read the file. Parse as JSON. If parsing fails, report the line/column and stop.

## 2. Validate

Open `adapters/style-schema.json`. Walk the user's `style.json`:
- `name` must be a non-empty string.
- One of `palette` or `mood_id` must be present.
- Any present `palette` must have `bg` and `fg` (other keys optional).
- Any present `typography.<voice>` must have `family`.
- Any present `forces.<dial>` must be 0 ≤ value ≤ 1.
- Any present `motion_intensity` must be 0 ≤ value ≤ 1.
- Any present `texture` must be one of `none`, `grain`, `glass`.
- Any present `mode_id` must be one of `swiss`, `brutalist`, `immersive`, `kinetic`, `gallery`.
- Any present `mood_id` must be one of `void`, `void-inverse`, `dusk`, `signal`, `earth`, `frost`, `blaze`, `bruise`, `concrete`.

If any check fails, report the failing field path and stop.

## 3. Pick base mode

- If `mode_id` present → use as `data-forces` on `<html>`.
- Else if `forces` present → find closest preset by L2 distance from the table below
  (sum over only the dials the user supplied, then sqrt). Tie-breaker: prefer `swiss`.
- Else → default `swiss`.

| mode_id | structure | disruption | density | breath | warmth | edge | stillness | motion | whisper | shout |
|---|---|---|---|---|---|---|---|---|---|---|
| swiss | 0.85 | 0.15 | 0.40 | 0.65 | 0.10 | 0.85 | 0.80 | 0.10 | 0.50 | 0.40 |
| brutalist | 0.40 | 0.80 | 0.85 | 0.10 | 0.00 | 1.00 | 0.85 | 0.05 | 0.00 | 1.00 |
| immersive | 0.25 | 0.60 | 0.20 | 0.80 | 0.60 | 0.40 | 0.10 | 0.90 | 0.50 | 0.40 |
| kinetic | 0.55 | 0.45 | 0.40 | 0.50 | 0.30 | 0.65 | 0.00 | 1.00 | 0.30 | 0.70 |
| gallery | 0.90 | 0.05 | 0.10 | 1.00 | 0.30 | 0.60 | 0.80 | 0.20 | 0.90 | 0.05 |

## 4. Pick base mood

- If `mood_id` present AND no `palette` → use as `data-mood`. No scope emitted.
- If `palette` present → set `data-mood="--custom"` on `<html>` and emit the custom
  scope (Step 5).
- If neither → default `void-inverse`.

## 5. Emit the style overrides block

Into `<style id="style-overrides">` in `templates/page-composed.html`'s head, write:

### Custom palette scope (if `palette` present)

```css
[data-mood="--custom"] {
  --color-bg:       <palette.bg>;
  --color-fg:       <palette.fg>;
  --color-accent-1: <palette.accent_1 || mix(fg, bg, 50%)>;
  --color-accent-2: <palette.accent_2 || mix(fg, bg, 30%)>;
  --color-discord:  <palette.discord  || palette.accent_1>;
  --color-muted:   color-mix(in oklch, var(--color-fg) 50%, var(--color-bg));
  --color-subtle:  color-mix(in oklch, var(--color-fg)  8%, var(--color-bg));
  --color-surface: color-mix(in oklch, var(--color-fg)  5%, var(--color-bg));
  --color-border:  color-mix(in oklch, var(--color-fg) 15%, var(--color-bg));
  --color-inverse: var(--color-fg);
}
```

Use the standard CSS `color-mix()` function for the four derived values. The four
required overrides (bg, fg, accent_1, accent_2, discord) get literal hex.

### Force overrides (if `forces` present)

```css
:root {
  --force-<dial>: <value>;   /* repeat per supplied dial */
}
```

Only emit the dials the user actually supplied; missing ones inherit from `mode_id`.

### Voice overrides (if `typography` present)

```css
:root {
  --font-monument:   '<typography.monument.family>', <fallback>;
  --font-swiss:      '<typography.swiss.family>', <fallback>;
  --font-editorial:  '<typography.editorial.family>', <fallback>;
  --font-brutalist:  '<typography.brutalist.family>', <fallback>;
  --font-expressive: '<typography.expressive.family>', <fallback>;
}
```

Fallback per voice: monument → sans-serif, swiss → sans-serif, editorial → serif,
brutalist → monospace, expressive → sans-serif.

### Motion scaling (if `motion_intensity` present)

```css
:root {
  --motion-scale: <max(motion_intensity, 0.01)>;
  --dur-instant:  calc(150ms  * var(--motion-scale, 1));
  --dur-swift:    calc(300ms  * var(--motion-scale, 1));
  --dur-natural:  calc(500ms  * var(--motion-scale, 1));
  --dur-drift:    calc(900ms  * var(--motion-scale, 1));
  --dur-meditate: calc(1800ms * var(--motion-scale, 1));
  --texture-grain-duration: calc(8s * var(--motion-scale, 1));
}
```

Clamp to a minimum of 0.01 so transitions stay parseable.

### Texture (if `texture` present and not `none`)

Set `<html data-texture="<value>">`. No CSS to emit — the `tokens/textures.css`
rules activate automatically.

## 6. Emit font links

For each `typography.<voice>` with `google: true`, append to the existing Google
Fonts `<link>` URL in `templates/page-composed.html`. Family names are
URL-encoded; weights joined with `;`. Example:

    family=Bodoni+Moda:wght@400;900

If `google` is false or missing, the agent assumes the family is system-available
or user-installed — no link emitted.

## 7. Pick sections

Read `sections/README.md`. Pick sections matching the brief and the style guide's
thesis. Honor `dos[]` / `donts[]`:
- A `dont` referencing "pure black backgrounds" steers away from `void` mood.
- A `do` referencing "warm grain" suggests `texture: "grain"` and a warm-mood
  palette.
- A `do` referencing "type that breathes" suggests `gallery` mode or generous
  `breath` force.

These are heuristics, not hard rules — pick what serves the brief.

## 8. Assemble

For each chosen section, read its partial, fill `{{...}}` placeholders by string
substitution, concatenate into the `<body>` of `templates/page-composed.html` in
the recommended order.

## 9. Verify

Open the page in a browser. Check that:
- bg color matches `palette.bg`
- hero font matches `typography.monument.family`
- No `{{...}}` placeholders leak through
- No console errors
- Texture (if not none) is visible
```

- [ ] **Step 3: Commit**

```bash
git add adapters/README.md adapters/from-style-json.md
git commit -m "Add adapters/README.md + from-style-json.md (consumption workflow)"
```

---

## Task 19: Adapter examples

**Files:**
- Create: `adapters/examples/minimal.json`
- Create: `adapters/examples/dusk.json`
- Create: `adapters/examples/signal.json`

- [ ] **Step 1: Write `adapters/examples/minimal.json`**

```json
{
  "name": "Minimal example",
  "palette": {
    "bg": "#f5f3ee",
    "fg": "#1a1a1a"
  }
}
```

- [ ] **Step 2: Write `adapters/examples/dusk.json`**

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
    "monument":   { "family": "Bodoni Moda",          "weights": [400, 900], "google": true },
    "swiss":      { "family": "Inter",                "weights": [400, 600], "google": true },
    "editorial":  { "family": "Cormorant Garamond",   "weights": [400, 700], "google": true },
    "brutalist":  { "family": "JetBrains Mono",       "weights": [400, 700], "google": true },
    "expressive": { "family": "DM Sans",              "weights": [400, 700], "google": true }
  },
  "forces": {
    "structure": 0.7, "breath": 0.7, "warmth": 0.8, "edge": 0.5, "motion": 0.4
  },
  "motion_intensity": 0.5,
  "texture": "grain",
  "mode_id": "swiss",
  "dos":   ["lean into warm grain", "type that breathes"],
  "donts": ["pure black backgrounds", "neon discord"]
}
```

- [ ] **Step 3: Write `adapters/examples/signal.json`**

```json
{
  "name": "Signal — neon kinetic",
  "thesis": "Terminal glow at 2 a.m. The screen is the city.",
  "palette": {
    "bg":       "#0a0a0f",
    "fg":       "#e0ffe0",
    "accent_1": "#00ff88",
    "accent_2": "#0088ff",
    "discord":  "#ff0066"
  },
  "typography": {
    "monument":  { "family": "Space Grotesk", "weights": [400, 700], "google": true },
    "swiss":     { "family": "Inter",         "weights": [400, 600], "google": true },
    "brutalist": { "family": "Space Mono",    "weights": [400, 700], "google": true }
  },
  "motion_intensity": 1.0,
  "texture": "grain",
  "mode_id": "kinetic",
  "dos":   ["sharp edges", "loud type", "motion has volume"],
  "donts": ["warm tones", "negative space"]
}
```

- [ ] **Step 4: Verify all three parse as JSON**

```bash
for f in adapters/examples/*.json; do
  python3 -c "import json; json.load(open('$f'))" && echo "$f OK"
done
```

Expected: each line ends with `<path> OK`.

- [ ] **Step 5: Verify all three validate against the schema**

```bash
python3 -c "
import json, sys
try:
    import jsonschema
except ImportError:
    print('jsonschema not installed; skipping deep validation. Install with: pip install jsonschema')
    sys.exit(0)
schema = json.load(open('adapters/style-schema.json'))
for f in ['adapters/examples/minimal.json', 'adapters/examples/dusk.json', 'adapters/examples/signal.json']:
    data = json.load(open(f))
    jsonschema.validate(instance=data, schema=schema)
    print(f, 'OK')
"
```

Expected: each example logs `OK`. If `jsonschema` is not installed, the script
exits cleanly with a one-line note — this is an optional defense-in-depth check.

- [ ] **Step 6: Commit**

```bash
git add adapters/examples/
git commit -m "Add adapters/examples — minimal, dusk, signal style.json"
```

---

End of Part 5. Continue to [part-6-showcase-docs.md](2026-05-20-part-6-showcase-docs.md).
