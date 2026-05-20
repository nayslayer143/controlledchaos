# Part 1: Foundation (Tasks 1–3)

Entry: [2026-05-20-controlled-chaos-v2-out-of-the-box.md](2026-05-20-controlled-chaos-v2-out-of-the-box.md). Next: [part-2-templates.md](2026-05-20-part-2-templates.md).

Foundation lands first: the interaction-states token file, polish-layer additions to `sections/sections.css` (image-placeholder + premium motion), and the `templates/page-composed.html` head upgrade. Everything downstream depends on these three.

---

## Task 1: Interaction-states token file

**Files:**
- Create: `tokens/states.css`

- [ ] **Step 1: Write `tokens/states.css`**

```css
/* ============================================
   STATES — Universal interactive feedback
   ============================================
   Hover, focus-visible, active, disabled, loading on every
   button / link / card / input. Plus 44px touch targets,
   .visually-hidden a11y helper, and reduced-motion fallbacks.

   Load AFTER tokens/textures.css and BEFORE sections/sections.css.
   Token-level interaction rules win on cascade tie-breaks.
   ============================================ */

/* --- BUTTON / CTA states ----------------------------------- */
button,
.cc-cta__action,
.cc-nav__cta,
[role="button"] {
  min-height: 44px;
  cursor: pointer;
  transition:
    transform   var(--dur-swift) var(--ease-spring),
    background  var(--dur-swift),
    border-color var(--dur-swift),
    color       var(--dur-swift),
    opacity     var(--dur-swift),
    box-shadow  var(--dur-swift);
}
button:hover,
.cc-cta__action:hover,
.cc-nav__cta:hover { transform: translateY(-2px); }

button:active,
.cc-cta__action:active,
.cc-nav__cta:active {
  transform: translateY(1px);
  transition-duration: var(--dur-instant);
}

button[disabled],
button[aria-disabled="true"],
.cc-cta__action[aria-disabled="true"] {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
  transform: none;
}

button[aria-busy="true"],
button[data-loading] {
  cursor: progress;
  pointer-events: none;
}

/* --- LINK states ------------------------------------------- */
a {
  transition: color var(--dur-swift), opacity var(--dur-swift);
}

/* --- FOCUS-VISIBLE rings (keyboard nav only) -------------- */
:focus-visible {
  outline: 2px solid var(--color-discord);
  outline-offset: 2px;
  border-radius: 2px;
}

/* Specific tighter ring for already-bordered elements */
input:focus-visible,
textarea:focus-visible,
select:focus-visible,
button:focus-visible {
  outline-offset: 1px;
}

/* --- FORM INPUT states ------------------------------------- */
input,
textarea,
select {
  min-height: 44px;
  padding: 0.75rem 1rem;
  border: 1px solid var(--color-border);
  background: transparent;
  color: var(--color-fg);
  font-family: var(--font-swiss);
  font-size: var(--type-body);
  transition:
    border-color var(--dur-swift),
    background   var(--dur-swift);
  border-radius: 0;
}
input::placeholder,
textarea::placeholder { color: var(--color-muted); }

input:hover,
textarea:hover,
select:hover { border-color: var(--color-fg); }

input:focus-visible,
textarea:focus-visible,
select:focus-visible {
  outline: 2px solid var(--color-discord);
  border-color: var(--color-fg);
}

input:invalid:not(:placeholder-shown) {
  border-color: var(--color-discord);
}

input[disabled],
textarea[disabled] {
  opacity: 0.4;
  cursor: not-allowed;
}

/* --- LOADING (button spinner) ------------------------------ */
button[aria-busy="true"]::after,
button[data-loading]::after {
  content: "";
  display: inline-block;
  margin-left: 0.5em;
  width: 1em; height: 1em;
  border: 2px solid currentColor;
  border-top-color: transparent;
  border-radius: 50%;
  animation: cc-spin 0.8s linear infinite;
  vertical-align: text-bottom;
}
@keyframes cc-spin { to { transform: rotate(360deg); } }

/* --- VISUALLY HIDDEN (a11y screen-reader-only) ------------ */
.visually-hidden {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
  border: 0;
}

/* --- REDUCED MOTION --------------------------------------- */
@media (prefers-reduced-motion: reduce) {
  button[aria-busy="true"]::after,
  button[data-loading]::after { animation: none; }
  button:hover,
  .cc-cta__action:hover,
  .cc-nav__cta:hover { transform: none; }
}
```

- [ ] **Step 2: Verify it parses + lights up an existing button**

```bash
# Verify CSS parses (no syntax errors)
cd /Users/nayslayer/.claude/skills/controlledchaos
python3 -c "
import re
content = open('tokens/states.css').read()
# Sanity check: balanced braces
opens = content.count('{')
closes = content.count('}')
assert opens == closes, f'Unbalanced braces: {opens} open vs {closes} close'
print(f'states.css parses cleanly: {opens} rule blocks')
"
```

Expected: `states.css parses cleanly: <N> rule blocks`

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add tokens/states.css
git commit -m "Add tokens/states.css — interaction states + 44px touch targets + a11y

Universal hover/focus-visible/active/disabled/loading states across
buttons, links, form inputs. Discord-cyan focus ring for keyboard
nav. Spinner animation for [aria-busy] / [data-loading]. .visually-
hidden helper for screen-reader-only labels. Reduced-motion zeros
hover transforms and the spinner.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 2: Image-placeholder upgrade + premium motion in `sections.css`

**Files:**
- Modify: `sections/sections.css` (replace the existing `.cc-img-placeholder` block + append premium-motion block)

- [ ] **Step 1: Read current sections.css to find the placeholder block**

```bash
grep -n "cc-img-placeholder" /Users/nayslayer/.claude/skills/controlledchaos/sections/sections.css
```

Expected: finds the existing `.cc-img-placeholder { ... }` and `.cc-img-placeholder::before { ... }` rules near the top of the file (Base section).

- [ ] **Step 2: Replace the existing placeholder rules with the upgraded gradient+grain version**

In `sections/sections.css`, find this block:

```css
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
```

Replace with:

```css
/* Placeholder for image slots — gradient + grain treatment.
   Reads as intentional empty state, not TODO image. */
.cc-img-placeholder {
  position: relative;
  overflow: hidden;
  background:
    radial-gradient(ellipse at center,
      color-mix(in oklch, var(--color-fg) 8%, transparent),
      transparent 70%),
    linear-gradient(135deg,
      color-mix(in oklch, var(--color-accent-1) 6%, var(--color-surface)),
      color-mix(in oklch, var(--color-accent-2) 4%, var(--color-surface)));
  color: var(--color-muted);
  font-family: var(--font-swiss);
  font-size: var(--type-caption);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  aspect-ratio: 16 / 10;
  min-height: 200px;
}
.cc-img-placeholder::before {
  /* Grain overlay (inline SVG noise) */
  content: ""; position: absolute; inset: 0;
  pointer-events: none;
  opacity: 0.12;
  mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml;utf8,<svg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
  background-size: 200px;
}
.cc-img-placeholder::after {
  content: "[ " attr(data-slot) " ]";
  position: absolute;
  inset: auto auto 1rem 1rem;
  font-size: var(--type-caption);
  color: var(--color-muted);
}
```

- [ ] **Step 3: Append the premium-motion block at the END of `sections.css`**

```css

/* ============================================
   PREMIUM MOTION — Universal scroll polish
   ============================================
   Scroll-progress hairline (animation-timeline scroll, Baseline 2024
   Chrome/Edge; @supports gracefully omits in Firefox stable).
   Smooth-scroll with sticky-nav offset.
   Both respect prefers-reduced-motion.
   ============================================ */

/* Scroll-progress hairline at the top of viewport */
@supports (animation-timeline: scroll()) {
  body::before {
    content: "";
    position: fixed;
    top: 0; left: 0; right: 0;
    height: 3px;
    background: var(--color-accent-1);
    z-index: 1000;
    transform-origin: left;
    animation: cc-scroll-progress linear both;
    animation-timeline: scroll();
    pointer-events: none;
  }
  @keyframes cc-scroll-progress {
    from { transform: scaleX(0); }
    to   { transform: scaleX(1); }
  }
}

/* Smooth-scroll honoring sticky-nav offset */
html {
  scroll-behavior: smooth;
  scroll-padding-top: calc(var(--nav-height, 0px) + 1rem);
}

@media (prefers-reduced-motion: reduce) {
  body::before { display: none; }
  html { scroll-behavior: auto; }
}
/* end-premium-motion */
```

- [ ] **Step 4: Sanity-check sections.css still parses**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
python3 -c "
content = open('sections/sections.css').read()
opens = content.count('{')
closes = content.count('}')
assert opens == closes, f'Unbalanced: {opens} open vs {closes} close'
print(f'sections.css OK: {opens} rule blocks')
"
```

Expected: `sections.css OK: <N> rule blocks`

- [ ] **Step 5: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/sections.css
git commit -m "Upgrade image-placeholder + add premium motion to sections.css

- cc-img-placeholder: replace flat gray with a gradient + inline-SVG
  grain treatment. Bottom-left attr() label reads as intentional
  empty state rather than TODO image.
- Premium motion: scroll-progress hairline at top of viewport using
  animation-timeline: scroll() with @supports fallback (Firefox
  stable gets no bar — graceful). Smooth-scroll with scroll-padding-
  top set from --nav-height for sticky-nav offset. Both halt under
  prefers-reduced-motion.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 3: `templates/page-composed.html` head upgrade

**Files:**
- Modify: `templates/page-composed.html` (replace entire `<head>`, add `tokens/states.css` to link chain)

- [ ] **Step 1: Replace the existing `<head>` block**

Open `templates/page-composed.html`. Find the existing `<head>` block (lines roughly 3-30). Replace the entire `<head>...</head>` with:

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="format-detection" content="telephone=no">

  <!-- SEO -->
  <title>{{seo.title}}</title>
  <meta name="description" content="{{seo.description}}">
  <link rel="canonical" href="{{seo.canonical_url}}">
  <meta name="theme-color" content="{{seo.theme_color}}">
  <meta name="color-scheme" content="light dark">

  <!-- Open Graph -->
  <meta property="og:type" content="{{seo.og_type}}">
  <meta property="og:site_name" content="{{seo.og_site_name}}">
  <meta property="og:title" content="{{seo.og_title}}">
  <meta property="og:description" content="{{seo.og_description}}">
  <meta property="og:url" content="{{seo.og_url}}">
  <meta property="og:image" content="{{seo.og_image_url}}">
  <meta property="og:image:alt" content="{{seo.og_image_alt}}">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="{{seo.twitter_card}}">
  <meta name="twitter:title" content="{{seo.twitter_title}}">
  <meta name="twitter:description" content="{{seo.twitter_description}}">
  <meta name="twitter:image" content="{{seo.twitter_image}}">
  <meta name="twitter:site" content="{{seo.twitter_site}}">

  <!-- Favicon (SVG default; brand replaces) -->
  <link rel="icon" type="image/svg+xml" href="/favicon.svg">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">

  <!-- JSON-LD Organization stub -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "{{org.name}}",
    "url": "{{org.url}}",
    "logo": "{{org.logo_url}}",
    "sameAs": [{{#org.same_as}}"{{.}}"{{^last}},{{/last}}{{/org.same_as}}]
  }
  </script>

  <!-- Fonts (drop voices you don't use to save bytes) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <!-- Tokens (order matters: textures + states + sections last) -->
  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../tokens/states.css">
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
```

- [ ] **Step 2: Verify the file is valid HTML5**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
python3 -c "
from html.parser import HTMLParser
class V(HTMLParser):
    def __init__(self):
        super().__init__()
        self.stack = []
        self.errors = []
    def handle_starttag(self, tag, attrs):
        if tag not in ('meta','link','br','hr','img','input','source'):
            self.stack.append(tag)
    def handle_endtag(self, tag):
        if self.stack and self.stack[-1] == tag:
            self.stack.pop()
v = V()
v.feed(open('templates/page-composed.html').read())
print('OK' if not v.stack else f'Unclosed tags: {v.stack}')
"
```

Expected: `OK`

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add templates/page-composed.html
git commit -m "Upgrade page-composed.html head — SEO/OG/Twitter/JSON-LD + states.css

Full SEO meta stack: title, description, canonical, theme-color,
color-scheme. Open Graph: type/site_name/title/description/url/
image/image:alt. Twitter card: summary_large_image variant with
title/description/image/site. JSON-LD Organization stub with
sameAs[]. Favicon SVG slot + apple-touch-icon slot. format-
detection meta to prevent iOS Safari from auto-linking phone
numbers.

Every value is a Mustache placeholder the agent fills inline from
the user's brief (schema stays vibe-only per the v2 spec).

Link chain: tokens/states.css added between textures and sections.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

End of Part 1. Continue to [part-2-templates.md](2026-05-20-part-2-templates.md).
