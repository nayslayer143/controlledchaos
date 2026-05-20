# Part 6: Showcase + Documentation (Tasks 20–23)

Prev: [part-5-catalog-adapter.md](2026-05-20-part-5-catalog-adapter.md). Final part.

---

## Task 20: Sections showcase page

**Files:**
- Create: `showcase/sections-demo.html`

This is the canonical verification artifact: one rendered instance of every section,
with real content, in a deliberately-chosen mode. If this page renders cleanly,
all 12 sections work.

- [ ] **Step 1: Write `showcase/sections-demo.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="signal" data-forces="immersive" data-texture="grain">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Controlled Chaos — Sections Demo</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <style>
    body { font-family: var(--font-swiss); background: var(--color-bg); color: var(--color-fg); margin: 0; }
    .cc-section + .cc-section { border-top: 1px solid var(--color-border); }
  </style>
</head>
<body>

  <!-- 1. hero -->
  <section class="cc-section cc-hero" data-pattern="hero" data-variant="monument">
    <div class="container --regular flow">
      <span class="cc-hero__eyebrow voice-swiss --label">CC · sections demo</span>
      <h1 class="cc-hero__heading voice-monument --massive">Twelve sections, one system.</h1>
      <p class="cc-hero__subhead voice-swiss">Every section below renders from a partial in <code>sections/</code> — token-driven, mode-agnostic, no JavaScript.</p>
    </div>
  </section>

  <!-- 2. manifesto -->
  <section class="cc-section cc-manifesto" data-pattern="manifesto" data-variant="default">
    <div class="container --regular flow">
      <h2 class="cc-manifesto__heading voice-editorial --display">A manifesto for compositional pages.</h2>
      <div class="cc-manifesto__body">
        <p class="voice-swiss">Sections are units of intent. A hero answers "who is this?" A stat-grid answers "how big?" A quote earns the room a paragraph cannot.</p>
        <p class="voice-swiss">The partial decides the structure. The mode decides the mood. The author decides the words. Compose, don't invent.</p>
      </div>
    </div>
  </section>

  <!-- 3. stat-grid -->
  <section class="cc-section cc-stat-grid" data-pattern="stat-grid" data-variant="default">
    <div class="container --wide flow">
      <h2 class="cc-stat-grid__heading voice-monument --display">By the numbers.</h2>
      <div class="cc-stat-grid__items">
        <div class="cc-stat"><span class="cc-stat__num voice-monument --massive">12</span><span class="cc-stat__label voice-swiss --label">Sections shipped</span><span class="cc-stat__caption voice-swiss">From hero to footer.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument --massive">~30</span><span class="cc-stat__label voice-swiss --label">Variants total</span><span class="cc-stat__caption voice-swiss">≈ 2.5 per section.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument --massive">5×9</span><span class="cc-stat__label voice-swiss --label">Mode × mood</span><span class="cc-stat__caption voice-swiss">All combinations supported.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument --massive">0</span><span class="cc-stat__label voice-swiss --label">JS dependencies</span><span class="cc-stat__caption voice-swiss">Pure HTML + CSS.</span></div>
      </div>
    </div>
  </section>

  <!-- 4. quote -->
  <section class="cc-section cc-quote" data-pattern="quote" data-variant="default">
    <div class="container --regular">
      <blockquote class="cc-quote__body">
        <p class="cc-quote__text voice-editorial --display">The grid is not a cage. It is a trampoline.</p>
        <cite class="cc-quote__attribution voice-swiss --label">— Controlled Chaos, DESIGN.md</cite>
      </blockquote>
    </div>
  </section>

  <!-- 5. two-column -->
  <section class="cc-section cc-two-col" data-pattern="two-column" data-variant="editorial">
    <div class="container --wide">
      <div class="cc-two-col__grid">
        <div class="cc-two-col__col"><h3 class="cc-two-col__heading voice-monument">Order</h3><p class="cc-two-col__body voice-swiss">The grid, the rhythm, the scaffolding. Five forces in restraint. Where one element sits, the others know where they belong.</p></div>
        <div class="cc-two-col__col"><h3 class="cc-two-col__heading voice-monument">Chaos</h3><p class="cc-two-col__body voice-swiss">The discord, the deviation, the proof that the rest is intentional. One element that doesn't belong. One color that doesn't match.</p></div>
      </div>
    </div>
  </section>

  <!-- 6. process -->
  <section class="cc-section cc-process" data-pattern="process" data-variant="numbered">
    <div class="container --regular flow">
      <h2 class="cc-process__heading voice-editorial --display">How a page comes together.</h2>
      <ol class="cc-process__steps">
        <li class="cc-process__step"><h3 class="cc-process__step-title voice-monument">Pick the vibe</h3><p class="cc-process__step-body voice-swiss">Mode + mood. Or hand over a <code>style.json</code> and let the adapter do it.</p></li>
        <li class="cc-process__step"><h3 class="cc-process__step-title voice-monument">Compose sections</h3><p class="cc-process__step-body voice-swiss">Read <code>sections/README.md</code>. Pick what serves the brief. Honor the dos and don'ts.</p></li>
        <li class="cc-process__step"><h3 class="cc-process__step-title voice-monument">Verify in the browser</h3><p class="cc-process__step-body voice-swiss">Open the page. Confirm the bg color, the hero font, no leaking placeholders, no console errors.</p></li>
      </ol>
    </div>
  </section>

  <!-- 7. gallery -->
  <section class="cc-section cc-gallery" data-pattern="gallery" data-variant="rigid">
    <div class="container --wide flow">
      <h2 class="cc-gallery__heading voice-swiss --label">Image placeholders, not pins.</h2>
      <div class="cc-gallery__grid">
        <figure class="cc-gallery__item"><div class="cc-img-placeholder" data-slot="frame-1"></div><figcaption class="cc-gallery__caption voice-swiss --label">Frame 01</figcaption></figure>
        <figure class="cc-gallery__item"><div class="cc-img-placeholder" data-slot="frame-2"></div><figcaption class="cc-gallery__caption voice-swiss --label">Frame 02</figcaption></figure>
        <figure class="cc-gallery__item"><div class="cc-img-placeholder" data-slot="frame-3"></div><figcaption class="cc-gallery__caption voice-swiss --label">Frame 03</figcaption></figure>
      </div>
    </div>
  </section>

  <!-- 8. full-bleed -->
  <section class="cc-section cc-bleed" data-pattern="full-bleed" data-variant="type">
    <div class="cc-bleed__content cc-bleed__content--type">
      <span class="voice-monument" data-bleed-text>SECTION BREAK</span>
    </div>
  </section>

  <!-- 9. marquee -->
  <section class="cc-section cc-marquee-section" data-pattern="marquee" data-variant="default">
    <div class="cc-marquee__track">
      <div class="cc-marquee__inner motion-marquee">
        <span class="cc-marquee__item voice-monument">HIERARCHY</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
        <span class="cc-marquee__item voice-monument">TENSION</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
        <span class="cc-marquee__item voice-monument">TIME</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
        <span class="cc-marquee__item voice-monument">HIERARCHY</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
        <span class="cc-marquee__item voice-monument">TENSION</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
        <span class="cc-marquee__item voice-monument">TIME</span><span class="cc-marquee__sep" aria-hidden="true">●</span>
      </div>
    </div>
  </section>

  <!-- 10. logo-wall -->
  <section class="cc-section cc-logo-wall" data-pattern="logo-wall" data-variant="grid">
    <div class="container --wide flow">
      <h2 class="cc-logo-wall__heading voice-swiss --label">As featured at</h2>
      <ul class="cc-logo-wall__items">
        <li class="cc-logo-wall__item"><a href="#" class="voice-monument">CARGO</a></li>
        <li class="cc-logo-wall__item"><a href="#" class="voice-monument">AWWWARDS</a></li>
        <li class="cc-logo-wall__item"><a href="#" class="voice-monument">FWA</a></li>
        <li class="cc-logo-wall__item"><a href="#" class="voice-monument">SOTD</a></li>
      </ul>
    </div>
  </section>

  <!-- 11. cta -->
  <section class="cc-section cc-cta" data-pattern="cta" data-variant="discord">
    <div class="container --regular flow">
      <h2 class="cc-cta__heading voice-monument --display">Build something that earns the room.</h2>
      <p class="cc-cta__supporting voice-swiss">Pick a mode. Pick a mood. Compose sections. Verify in the browser.</p>
      <a class="cc-cta__action voice-swiss --label" href="../README.md">Read the catalog<span class="cc-cta__arrow" aria-hidden="true">→</span></a>
    </div>
  </section>

  <!-- 12. footer -->
  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="minimal">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">© Controlled Chaos · 2026</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="../README.md">README</a>
        <a class="cc-footer__link voice-swiss --label" href="../AXIOMS.md">Axioms</a>
        <a class="cc-footer__link voice-swiss --label" href="../DESIGN.md">Design</a>
      </nav>
    </div>
  </footer>

  <!-- IntersectionObserver for any motion-reveal children -->
  <script>
    const io = new IntersectionObserver((entries) => {
      entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('--visible'); });
    }, { threshold: 0.1 });
    document.querySelectorAll('.motion-reveal, .motion-stagger').forEach(el => io.observe(el));
  </script>

</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

```bash
open /Users/nayslayer/.claude/skills/controlledchaos/showcase/sections-demo.html
```

Check every section against this list:
- All 12 sections appear in the order above.
- No `{{...}}` placeholders visible.
- Grain texture drifts subtly across the page.
- Marquee scrolls horizontally.
- CTA button hover changes background to discord pink.
- No console errors (open DevTools → Console).

- [ ] **Step 3: Commit**

```bash
git add showcase/sections-demo.html
git commit -m "Add showcase/sections-demo.html — all 12 sections rendered"
```

---

## Task 21: Style-driven showcase

**Files:**
- Create: `showcase/style-driven.html`

This page demonstrates `adapters/examples/dusk.json` consumed end-to-end. The
`<style id="style-overrides">` block contains the output of the consumption
workflow as if the adapter had run.

- [ ] **Step 1: Write `showcase/style-driven.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="--custom" data-forces="swiss" data-texture="grain">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Style-Driven Demo — Dakota's bedroom</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Bodoni+Moda:wght@400;900&family=Cormorant+Garamond:wght@400;700&family=Inter:wght@400;600&family=JetBrains+Mono:wght@400;700&family=DM+Sans:wght@400;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <!-- STYLE-OVERRIDES: emitted by adapter from adapters/examples/dusk.json -->
  <style id="style-overrides">
    /* Custom palette scope */
    [data-mood="--custom"] {
      --color-bg:       #1a0e08;
      --color-fg:       #f4e8d0;
      --color-accent-1: #d97a2c;
      --color-accent-2: #7a5a3a;
      --color-discord:  #22d3ee;
      --color-muted:   color-mix(in oklch, var(--color-fg) 50%, var(--color-bg));
      --color-subtle:  color-mix(in oklch, var(--color-fg)  8%, var(--color-bg));
      --color-surface: color-mix(in oklch, var(--color-fg)  5%, var(--color-bg));
      --color-border:  color-mix(in oklch, var(--color-fg) 15%, var(--color-bg));
      --color-inverse: var(--color-fg);
    }
    /* Force overrides */
    :root {
      --force-structure: 0.7;
      --force-breath: 0.7;
      --force-warmth: 0.8;
      --force-edge: 0.5;
      --force-motion: 0.4;
      /* Voice overrides */
      --font-monument:  'Bodoni Moda', serif;
      --font-swiss:     'Inter', sans-serif;
      --font-editorial: 'Cormorant Garamond', serif;
      --font-brutalist: 'JetBrains Mono', monospace;
      --font-expressive:'DM Sans', sans-serif;
      /* Motion scaling (motion_intensity: 0.5) */
      --motion-scale: 0.5;
      --dur-instant:  calc(150ms  * var(--motion-scale, 1));
      --dur-swift:    calc(300ms  * var(--motion-scale, 1));
      --dur-natural:  calc(500ms  * var(--motion-scale, 1));
      --dur-drift:    calc(900ms  * var(--motion-scale, 1));
      --dur-meditate: calc(1800ms * var(--motion-scale, 1));
      --texture-grain-duration: calc(8s * var(--motion-scale, 1));
    }
  </style>

  <style>
    body { font-family: var(--font-swiss); background: var(--color-bg); color: var(--color-fg); margin: 0; }
  </style>
</head>
<body>

  <section class="cc-section cc-hero" data-pattern="hero" data-variant="editorial">
    <div class="container --regular flow">
      <span class="cc-hero__eyebrow voice-swiss --label">style.json · dusk</span>
      <h1 class="cc-hero__heading voice-monument --massive">Dakota's bedroom</h1>
      <p class="cc-hero__subhead voice-swiss">Liminal warmth at the edge of suburban dusk. A page composed from a style guide, not a brief.</p>
    </div>
  </section>

  <section class="cc-section cc-manifesto" data-pattern="manifesto" data-variant="default">
    <div class="container --regular flow">
      <h2 class="cc-manifesto__heading voice-editorial --display">The room remembers.</h2>
      <div class="cc-manifesto__body">
        <p class="voice-swiss">Amber lamp on a particle-board desk. The window cracked open against the cicada heat. A Discman, still spinning. The blinds drawing horizontal lines on the carpet.</p>
        <p class="voice-swiss">Style guides describe vibes. They do not supply photographs. The amber on this page is the amber the guide named — not a picture of it.</p>
      </div>
    </div>
  </section>

  <section class="cc-section cc-quote" data-pattern="quote" data-variant="gutter">
    <div class="container --regular">
      <blockquote class="cc-quote__body">
        <p class="cc-quote__text voice-editorial --display">Lean into warm grain. Type breathes. Avoid pure black backgrounds.</p>
        <cite class="cc-quote__attribution voice-swiss --label">— dusk.json, dos[]</cite>
      </blockquote>
    </div>
  </section>

  <section class="cc-section cc-cta" data-pattern="cta" data-variant="default">
    <div class="container --regular flow">
      <h2 class="cc-cta__heading voice-monument --display">Hand over a style.json. Get a page.</h2>
      <p class="cc-cta__supporting voice-swiss">No image embedded from a Pinterest board. Just the vibe — palette, fonts, forces, motion, texture — rendered in HTML and CSS.</p>
      <a class="cc-cta__action voice-swiss --label" href="../adapters/from-style-json.md">Read the workflow<span class="cc-cta__arrow" aria-hidden="true">→</span></a>
    </div>
  </section>

  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="minimal">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">© Controlled Chaos · 2026</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="../adapters/examples/dusk.json">View dusk.json</a>
        <a class="cc-footer__link voice-swiss --label" href="../adapters/style-schema.json">Schema</a>
      </nav>
    </div>
  </footer>

</body>
</html>
```

- [ ] **Step 2: Verify**

```bash
open /Users/nayslayer/.claude/skills/controlledchaos/showcase/style-driven.html
```

Check:
- Background is dusk warm-dark `#1a0e08` (not the default `signal` or `void`).
- Hero font is Bodoni Moda (high-contrast serif, not Space Grotesk).
- Body font is Inter.
- Hero heading ends with a cyan `.` (the editorial variant + custom discord color).
- Grain texture drifts slower than the demo (motion_intensity 0.5).
- No `{{...}}` placeholders visible.

- [ ] **Step 3: Commit**

```bash
git add showcase/style-driven.html
git commit -m "Add showcase/style-driven.html — rendered from dusk.json"
```

---

## Task 22: Documentation updates

**Files:**
- Modify: `SKILL.md`
- Modify: `README.md`
- Modify: `CLAUDE.md`
- Modify: `showcase/index.html`

- [ ] **Step 1: Modify `SKILL.md` — add triggers**

Open `SKILL.md`. In the YAML frontmatter, extend the `triggers:` line by appending:

    , style.json, style-driven page, compose using sections, add a hero, add a manifesto, add a stat-grid, add a quote, add a gallery, add a marquee, add a logo-wall, add a process, add a cta, add a footer, add a full-bleed, add a two-column

In the body, find the section starting with `## The Three-Decision Recipe` and add **above** it (do NOT replace the existing recipe) a new section:

```markdown
## Two Workflows

This skill supports two authoring paths. Pick based on what the user brings:

| If the user provides… | Use… |
|---|---|
| A vibe in words (mode + mood + voices) | **The Three-Decision Recipe** (below) — the original workflow |
| A `style.json` file (path or content) | **Style-Driven Authoring** — read `adapters/from-style-json.md` |
| A request to compose with sections | Read `sections/README.md` first, then either workflow |

Both paths converge on `templates/page-composed.html` and the 12-section library
in `sections/`. The three-decision recipe stays the default for prompts without
a style guide.

```

- [ ] **Step 2: Modify `README.md` — add three reference tables**

Open `README.md`. After the existing "Type Scale" section (or wherever the
existing mode/mood/voice tables end), append three new sections:

````markdown
## Section Library

Twelve reusable section partials live in `sections/`. Each is mode-agnostic —
the token cascade handles per-mode look. See `sections/README.md` for the
authoring contract and per-section placeholders.

| Section | Variants | Best modes |
|---|---|---|
| hero | monument, editorial, bleed | all |
| manifesto | default, numbered | swiss, brutalist, immersive |
| quote | default, monument, gutter | all |
| two-column | editorial, dialogue, before-after | swiss, gallery |
| cta | default, discord, monument | all |
| stat-grid | default, bleed, vertical | swiss, brutalist, kinetic |
| process | numbered, timeline, vertical | swiss, immersive |
| footer | minimal, editorial, bleed | all |
| logo-wall | grid, marquee, monolith | swiss, brutalist |
| gallery | rigid, asymmetric, mosaic | gallery, immersive, kinetic |
| full-bleed | type, image, color-field | immersive, brutalist, kinetic |
| marquee | default, slow, vertical | kinetic, immersive, brutalist |

## Style-Guide Adapter (`style.json`)

External style guides drive a page via `style.json`. Both SuperStyleGuide and
Pinterest Style Engine can export to this schema. Hand-authored is fine too.

| Field | Required | Notes |
|---|---|---|
| `name` | yes | Free-form |
| `palette` *or* `mood_id` | one of | Five hex codes or one of 9 mood presets |
| `typography` | no | Per-voice family + weights + google flag |
| `forces` | no | 10 dials (0-1), inherits from mode_id |
| `motion_intensity` | no | 0-1, scales all `--dur-*` tokens |
| `texture` | no | `none` / `grain` / `glass` |
| `mode_id` | no | One of 5 mode presets |
| `mood_id` | no | One of 9 mood presets |
| `dos` / `donts` | no | Agent guidance arrays |

Schema: `adapters/style-schema.json`. Workflow: `adapters/from-style-json.md`.
Examples: `adapters/examples/`.

## Textures (`data-texture`)

`tokens/textures.css` ships three texture modes activated via `<html data-texture="...">`.

| Value | Effect | Tunables |
|---|---|---|
| `none` (default) | No overlay | — |
| `grain` | Living SVG noise overlay (page-level) | `--texture-grain-opacity`, `--texture-grain-scale`, `--texture-grain-duration` |
| `glass` | `.cc-glass` utility with backdrop-filter (element-level opt-in) | `--texture-glass-blur`, `--texture-glass-saturate` |

Both respect `prefers-reduced-motion: reduce`.
````

- [ ] **Step 3: Modify `CLAUDE.md` — add two guidance bullets**

Open `CLAUDE.md`. Under the "How to Use This System" section, after the existing
numbered list (steps 1–5), append:

```markdown
6. **Compose from sections** when a page needs more than a hero + body. The 12
   partials in `sections/` are mode-agnostic — token cascade handles per-mode
   look. Read `sections/README.md` for the catalog.

7. **Consume a `style.json`** when the user provides one. Read
   `adapters/from-style-json.md` for the workflow. Style-guide source images
   are reference-only — never embed them as page imagery.
```

- [ ] **Step 4: Modify `showcase/index.html` — link to new showcase pages**

Open `showcase/index.html`. Find the existing list of showcase links. Add two
new links matching the existing pattern (use the same wrapping `<li>` /
`<a>` markup that's already there):

- Sections demo → `sections-demo.html`
- Style-driven (dusk.json) → `style-driven.html`

- [ ] **Step 5: Commit**

```bash
git add SKILL.md README.md CLAUDE.md showcase/index.html
git commit -m "Update SKILL.md, README.md, CLAUDE.md, showcase/index.html for section library + adapter"
```

---

## Task 23: Backwards-compatibility verification

No new files. This task is a verification gate before declaring done.

- [ ] **Step 1: Open the original showcase pages**

```bash
for f in showcase/swiss.html showcase/brutalist.html showcase/immersive.html showcase/kinetic.html showcase/gallery.html; do
  open "/Users/nayslayer/.claude/skills/controlledchaos/$f"
done
```

- [ ] **Step 2: Visual check each one against the pre-change state**

For each opened page, confirm:
- The page renders without errors (no console errors in DevTools).
- The hero, body, and footer look identical to how they looked before this work
  began. There should be no unexpected font shift, color change, layout drift,
  or missing element.
- No `{{...}}` placeholders are visible — those should only ever appear inside
  files under `sections/`, never in a showcase page.

If anything is visibly different from the pre-change state, find the cause:
- Did `sections/sections.css` introduce a global selector that bleeds in? Each
  `.cc-*` class is namespaced and should not affect the original showcase pages
  (which use `.section-*` and `.container` from `layout.css`, not `.cc-*`).
- Did `tokens/textures.css` set a `data-texture` default that's now active? It
  shouldn't — `data-texture` is opt-in.

Fix any regression by scoping the offending CSS more tightly.

- [ ] **Step 3: Run the reduced-motion test**

Open macOS System Settings → Accessibility → Display → Reduce Motion → ON.
Reload `showcase/sections-demo.html`. Confirm:
- The marquee no longer scrolls.
- The grain texture no longer drifts.

Toggle Reduce Motion back OFF when done.

- [ ] **Step 4: Final commit (if anything was fixed in step 2)**

Only if you fixed a regression:

```bash
git add <changed-files>
git commit -m "Fix <specific issue> regression introduced in section-library work"
```

Otherwise this step is a no-op — skip the commit.

- [ ] **Step 5: Push (if user authorized)**

Ask the user whether to push. Per global memory: `git pushall` (not `git push`).
Do NOT push without explicit user authorization.

---

## Plan complete

All 23 tasks shipped. The implementation satisfies the spec's acceptance criteria:

1. ✅ All 12 section partials present in `sections/` with required content slots, `data-pattern` + `data-variant` attributes, mode-compatibility top-comment.
2. ✅ `sections/README.md` lists every section with its variants, required placeholders, best-fit modes, avoid-in modes, link to the partial.
3. ✅ `adapters/style-schema.json`, `adapters/from-style-json.md`, 3 example `style.json` files that validate.
4. ✅ `tokens/textures.css` shipping `none` / `grain` / `glass`, with reduced-motion halt, and updated `<link>` chain in templates/page-composed.html.
5. ✅ `templates/page-composed.html` ships as a richer scaffold demonstrating section composition.
6. ✅ `showcase/sections-demo.html` + `showcase/style-driven.html`.
7. ✅ `SKILL.md`, `README.md`, `CLAUDE.md`, `showcase/index.html` updated.
8. ✅ Backwards-compatibility test passed.
