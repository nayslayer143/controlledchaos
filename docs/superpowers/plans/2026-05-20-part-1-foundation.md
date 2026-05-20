# Part 1: Foundation (Tasks 1–3)

Entry: [2026-05-20-section-library-and-style-guide-adapter.md](2026-05-20-section-library-and-style-guide-adapter.md). Next: [part-2-sections-text.md](2026-05-20-part-2-sections-text.md).

---

## Task 1: Texture token file

**Files:**
- Create: `tokens/textures.css`

- [ ] **Step 1: Write `tokens/textures.css`**

```css
/* ============================================
   TEXTURES — Page material layer
   ============================================
   Activated via <html data-texture="...">.
   - none  (default) — no overlay
   - grain           — body::after SVG noise overlay, slow drift
   - glass           — .cc-glass utility class with backdrop-filter

   Load AFTER tokens/forces.css.
   Reuses the grain-shift @keyframes already defined in motion.css (line 163).
   ============================================ */

:root {
  --texture-grain-opacity:  0.08;
  --texture-grain-scale:    0.85;
  --texture-grain-duration: 8s;
  --texture-glass-blur:     20px;
  --texture-glass-saturate: 180%;
}

/* --- GRAIN -------------------------------------------------------- */
html[data-texture="grain"] body { position: relative; }

html[data-texture="grain"] body::after {
  content: "";
  position: fixed;
  inset: -50%;
  pointer-events: none;
  z-index: 9999;
  opacity: var(--texture-grain-opacity);
  mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml;utf8,\
<svg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'>\
<filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/></filter>\
<rect width='100%' height='100%' filter='url(%23n)' opacity='1'/></svg>");
  background-size: calc(200px * var(--texture-grain-scale));
  animation: grain-shift var(--texture-grain-duration) steps(8) infinite;
}

/* --- GLASS -------------------------------------------------------- */
html[data-texture="glass"] .cc-glass {
  backdrop-filter: blur(var(--texture-glass-blur))
                  saturate(var(--texture-glass-saturate));
  -webkit-backdrop-filter: blur(var(--texture-glass-blur))
                          saturate(var(--texture-glass-saturate));
  background: color-mix(in oklch, var(--color-bg) 60%, transparent);
  border: 1px solid color-mix(in oklch, var(--color-fg) 12%, transparent);
}

/* --- REDUCED-MOTION ---------------------------------------------- */
@media (prefers-reduced-motion: reduce) {
  html[data-texture="grain"] body::after {
    animation: none;
  }
}
```

- [ ] **Step 2: Verify with a quick test page**

Create a throwaway test page at `/tmp/cc-textures-test.html`:

```html
<!DOCTYPE html>
<html data-mood="dusk" data-forces="immersive" data-texture="grain">
<head>
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/layout.css">
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/typography.css">
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/color.css">
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/motion.css">
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/forces.css">
  <link rel="stylesheet" href="/Users/nayslayer/.claude/skills/controlledchaos/tokens/textures.css">
  <style>body { background: var(--color-bg); color: var(--color-fg); margin:0; padding:80px; font-family: var(--font-swiss); }</style>
</head>
<body>
  <h1>Grain texture should drift slowly across this page</h1>
  <p>Watch for noise grain. Should be subtle, animated.</p>
</body>
</html>
```

Open it: `open /tmp/cc-textures-test.html`

Expected: subtle grain overlay visible against the dusk background, drifting on an 8s loop.

Repeat with `data-texture="glass"` on the `<html>` and add `<div class="cc-glass" style="padding:40px;margin-top:40px">Glass panel</div>` — the div should have a frosted backdrop.

- [ ] **Step 3: Commit**

```bash
cd ~/.claude/skills/controlledchaos
git add tokens/textures.css
git commit -m "Add tokens/textures.css — grain + glass with reduced-motion halt"
```

Delete the test page: `rm /tmp/cc-textures-test.html`

---

## Task 2: Section CSS scaffold

**Files:**
- Create: `sections/sections.css`

- [ ] **Step 1: Write `sections/sections.css` (base only — per-section rules added by later tasks)**

```css
/* ============================================
   SECTIONS — Reusable section partials
   ============================================
   Per-section rules are appended below as each
   section partial ships. Base rhythm + reset only.

   Load AFTER tokens/textures.css.
   ============================================ */

/* --- Base ---------------------------------------------------- */
.cc-section {
  position: relative;
  padding-block: clamp(4rem, 8vh, 8rem);
}

.cc-section + .cc-section {
  margin-top: 0;
}

/* Bleed sections push to viewport edges */
.cc-section[data-pattern="full-bleed"],
.cc-section[data-variant="bleed"] {
  padding-inline: 0;
}

/* Placeholder for image slots */
.cc-img-placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--color-subtle);
  color: var(--color-muted);
  font-family: var(--font-swiss);
  font-size: var(--type-caption);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  aspect-ratio: 16 / 10;
  min-height: 200px;
}

.cc-img-placeholder::before {
  content: "[ image: " attr(data-slot) " ]";
}

/* Per-section rules — appended below by Tasks 4–15 */
```

- [ ] **Step 2: Commit**

```bash
git add sections/sections.css
git commit -m "Add sections/sections.css scaffold with cc-section base"
```

---

## Task 3: Page-composed template

**Files:**
- Create: `templates/page-composed.html`

- [ ] **Step 1: Write `templates/page-composed.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="signal" data-forces="immersive">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>

  <!-- Fonts (drop voices you don't use to save bytes) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <!-- Tokens (order matters: textures + sections last) -->
  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <!-- STYLE-GUIDE-OVERRIDES
       When driven from style.json, the adapter emits a <style> block
       here containing the custom mood scope, force overrides, voice
       font-family overrides, and motion-scale. Leave empty for
       three-decision (no style.json) authoring. -->
  <style id="style-overrides"></style>

  <style>
    body {
      font-family: var(--font-swiss);
      background: var(--color-bg);
      color: var(--color-fg);
      margin: 0;
    }
  </style>
</head>
<body>

  <!-- COMPOSE SECTIONS FROM sections/.
       Read sections/README.md for the catalog.
       Each section partial uses Mustache {{...}} placeholders the
       agent fills via string substitution.

       Recommended ordering for a typical page:
         hero → manifesto → stat-grid → quote → cta → footer

       For pages composed from style.json:
         Honor dos[] / donts[] when picking sections + variants. -->

  <!-- SECTION: HERO -->
  <!-- replace with content of sections/hero.html, fill placeholders -->

  <!-- SECTION: BODY -->
  <!-- one or more of: manifesto / stat-grid / quote / two-column /
       process / gallery / full-bleed / marquee / logo-wall -->

  <!-- SECTION: CTA -->
  <!-- replace with content of sections/cta.html -->

  <!-- SECTION: FOOTER -->
  <!-- replace with content of sections/footer.html -->

</body>
</html>
```

- [ ] **Step 2: Commit**

```bash
git add templates/page-composed.html
git commit -m "Add templates/page-composed.html — richer scaffold with section composition pattern"
```

---

End of Part 1. Continue to [part-2-sections-text.md](2026-05-20-part-2-sections-text.md).
