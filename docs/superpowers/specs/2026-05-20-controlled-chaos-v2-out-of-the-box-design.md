# Controlled Chaos v2 — "Out of the Box"

**Status:** Draft for review
**Date:** 2026-05-20
**Scope:** Skill upgrade to ship SMB-tech-grade landing pages out of the box
**Predecessor:** [2026-05-20-section-library-and-style-guide-adapter-design.md](2026-05-20-section-library-and-style-guide-adapter-design.md)

---

## Overview

Upgrade the `controlledchaos` skill (`~/.claude/skills/controlledchaos/`) so that pages it generates feel like something an SMB tech company would deploy *out of the box*. SF Vibe Fest 2027 is the canary brand: every new component and polish layer must look right when re-rendered for it before it ships.

**Three big additions:**

1. **+10 new sections** to the library (12 → 22): `nav`, `feature-grid`, `product-showcase`, `pricing`, `faq`, `testimonial`, `signup-form`, `integration-logos`, `cta-banner`, `comparison-table`.
2. **A universal polish layer** — interaction states (`tokens/states.css`), premium motion (scroll-progress, sticky-nav-aware smooth scroll, button micro-interactions), and an image-placeholder upgrade.
3. **Template scaffolding** — `templates/page-composed.html` upgraded to ship deploy-ready: full SEO/OG/Twitter/JSON-LD meta, favicon slot, plus new `404.html`, `privacy.html`, `terms.html`, `meta.partial.html`.

**Polish baseline locked: Editorial-Generous** (Stripe / Anthropic / Cursor-marketing DNA). Large breathing type, multi-column features with real prose, narrative pacing, soft 400-700ms eases. This is the closest fit for CC's existing editorial voice and the brand vibe Fog City Futurism asserts. Polish modes A (Minimal-Tight) and C (Dashboard-Bold) are deferred to a later spec.

**Schema decision: `style.json` stays vibe-only.** All new section content is filled inline by the agent from the user's brief — same authoring pattern as the existing 12 sections. The schema does not balloon. SEO meta and other page-level data are written inline into `templates/page-composed.html`'s `<head>` placeholders, not into `style.json`.

**Compatibility guarantee:** every existing CC page renders identically. The polish layer is additive CSS (new classes / new selectors) that does not modify existing rules. New section partials are new files. Both adapter tests (`adapters/examples/lovetel.json` driving `showcase/test-lovetel.html`, and `adapters/examples/sf-vibe-fest.json` driving `showcase/test-sf-vibe-fest.html`) continue to render — SF Vibe Fest gets a v2 expansion (15 sections instead of 11), Lovetel is untouched.

**Out of scope:**
- Polish modes A and C — only B ships in v2.
- `style.json` schema extension — vibe stays vibe.
- Deployment tooling — no CLI, no one-click deploy. Skill produces HTML; deployment is the user's problem.
- Form *handling* — `signup-form` ships as accessible markup only. Backend wiring (Formspree, Resend, etc.) is per-brand.
- A11y certification beyond focus-rings, touch-targets, semantic markup, and reduced-motion. No WCAG 2.2 AA cert claim.
- Polish-mode-specific section variants. All new sections ship in the Editorial-Generous baseline only.

---

## Architecture

### File layout (additive — no renames, no moves except documented modifications)

```
~/.claude/skills/controlledchaos/
├── SKILL.md                       MODIFIED — new-section triggers, two-workflow note
├── README.md                      MODIFIED — section catalog grows to 22, polish + scaffolding tables
├── CLAUDE.md                      MODIFIED — one bullet on SMB-tech baseline
│
├── sections/
│   ├── README.md                  MODIFIED — catalog index, 22 entries
│   ├── sections.css               MODIFIED — polish + new-section rules appended
│   ├── icons.md                   NEW — 12–15 reference inline SVG snippets
│   ├── nav.html                   NEW
│   ├── feature-grid.html          NEW
│   ├── product-showcase.html      NEW
│   ├── pricing.html               NEW
│   ├── faq.html                   NEW
│   ├── testimonial.html           NEW
│   ├── signup-form.html           NEW
│   ├── integration-logos.html     NEW
│   ├── cta-banner.html            NEW
│   ├── comparison-table.html      NEW
│   └── (existing 12 partials)     UNCHANGED
│
├── tokens/
│   ├── states.css                 NEW — interaction states, 8th token file
│   └── (existing 7 token files)   UNCHANGED
│
├── templates/
│   ├── page-composed.html         MODIFIED — SEO/OG/JSON-LD head, nav-aware scroll offset
│   ├── 404.html                   NEW — Fog City Futurism by default, style.json-overridable
│   ├── privacy.html               NEW — placeholder structure + content scaffold
│   ├── terms.html                 NEW — placeholder structure + content scaffold
│   ├── meta.partial.html          NEW — the <head> meta stack as an includeable snippet
│   └── page.html                  UNCHANGED
│
├── showcase/
│   ├── test-sf-vibe-fest.html     MODIFIED — 15-section re-render using new library
│   └── (existing showcase pages)  UNCHANGED
│
└── adapters/                      UNCHANGED — schema stays vibe-only
```

### Final `<link>` chain in `page-composed.html`

```html
<link rel="stylesheet" href="../tokens/layout.css">
<link rel="stylesheet" href="../tokens/typography.css">
<link rel="stylesheet" href="../tokens/color.css">
<link rel="stylesheet" href="../tokens/motion.css">
<link rel="stylesheet" href="../tokens/forces.css">
<link rel="stylesheet" href="../tokens/textures.css">
<link rel="stylesheet" href="../tokens/states.css">       <!-- NEW -->
<link rel="stylesheet" href="../sections/sections.css">
```

`states.css` is loaded after `textures.css` so its `:hover`, `:focus-visible`, and `:active` rules can override base section styles where needed.

---

## The Polish Baseline: Editorial-Generous

The skill ships a single polish baseline in v2. Its conventions guide every new section's default variant and inform polish-layer tuning.

- **Type ratios** — generous. Hero `clamp(2.5rem, 11vw, 7rem)` italic display serif; section headings `clamp(1.75rem, 7vw, 3.5rem)` editorial serif; body `var(--type-body)` with `line-height: 1.55-1.65`.
- **Vertical rhythm** — `padding-block: clamp(4rem, 8vh, 8rem)` per section default. Sections breathe.
- **Type alignment** — left-aligned default. Centered only when explicitly chosen.
- **Color usage** — accent colors used as *signal* not decoration. Discord color reserved for trust/AI/contrast moments (focus rings, gutter borders, single italicized phrases).
- **Button shape** — square-cornered by default (radius: 0). Variants can opt into 6-8px radius. No pills.
- **Animation timing** — 400-700ms eases for section reveals (`var(--dur-natural)` / `var(--dur-drift)`), 200-300ms for hover (`var(--dur-swift)`), 80-120ms for active/press (`var(--dur-instant)`).
- **Decoration** — hairline borders (`1px solid var(--color-border)`) preferred over shadows. Where shadows appear, they're long and soft, never the default browser drop.
- **Voice** — sentence case headings by default (override the `voice-monument.--display` uppercase via page or component CSS).

---

## The 10 New Sections

Each follows the same authoring contract as the existing 12:

- Begins with a top-comment listing best/ok/avoid modes + variants + placeholders.
- Root element carries `class="cc-section cc-<name>"`, `data-pattern="<name>"`, `data-variant="<default>"`.
- Uses Mustache: `{{scalar}}` and `{{#items}}…{{/items}}`.
- Section-specific CSS lives in `sections/sections.css`, appended after the existing rules.
- Image slots use `<div class="cc-img-placeholder" data-slot="<name>"></div>`.

### 13. `nav`

**Sticky top bar.** First thing visitors see; persists during scroll.

| Variant | Layout |
|---|---|
| `default` (default) | logo left, links + CTA right |
| `centered` | logo center, links flanking |
| `minimal` | logo + one CTA only (no link nav) |

**Placeholders:** `brand` (string), `links[]` (label, href), `cta_text`, `cta_href`.

**Best modes:** all (with per-mode density tuning).
**Avoid in:** —

**Editorial-Generous notes:**
- `position: sticky; top: 0; z-index: 100`. `backdrop-filter: saturate(180%) blur(20px)` on scroll for a frosted bar. Hairline bottom border that appears once scrolled past 100px (via `[data-scrolled]` attribute, toggled by 8 lines of JS).
- Mobile: collapses to a hamburger that opens a full-screen drawer overlay. Drawer animates from the right at `var(--dur-natural)`. Closes on Escape, on link click, on backdrop click.
- Touch target: ≥44px on every nav item.
- Sets `--nav-height: 64px` (desktop) / `56px` (mobile) as a CSS variable so the smooth-scroll offset works.

### 14. `feature-grid`

**3-6 cards each with icon + heading + body.** Different from `stat-grid` (numbers) and `gallery` (images).

| Variant | Layout |
|---|---|
| `default` (default) | 3-column grid, equal cards |
| `wide` | 2-column grid, longer body copy per card |
| `compact` | 4-column grid, shorter body copy per card |

**Placeholders:** `heading`, `eyebrow?`, `features[]` (icon (raw SVG slot), title, body).

**Best modes:** swiss, gallery, immersive.
**Avoid in:** brutalist, kinetic.

**Editorial-Generous notes:**
- Icon is an inline `<svg viewBox="0 0 24 24">` slot the agent fills per feature. `sections/icons.md` carries 12-15 reference SVG snippets (stroke-based, 1.5-2px stroke-width, currentColor) the agent can crib. Icon color: `var(--color-accent-1)`.
- Card has no border, no background by default. Generous padding (`clamp(1.5rem, 3vw, 2.5rem)`). Hairline `border-top: 1px solid var(--color-border)` for grid lines.
- Hover: subtle `transform: translateY(-2px)` and icon color shift to `--color-discord`.

### 15. `product-showcase`

**Annotated screenshot/mockup.** The biggest gap in v1 — pure type can't sell software.

| Variant | Layout |
|---|---|
| `split` (default) | Image left (50% width), copy right |
| `centered` | Copy above, image full-width below |
| `callouts` | Image with numbered annotation pills connected to image regions |

**Placeholders:** `heading`, `body`, `image_slot`, `callouts[]` (num, label) — for `callouts` variant only.

**Best modes:** gallery, immersive.
**Avoid in:** brutalist.

**Editorial-Generous notes:**
- Image placeholder gets a "browser frame" treatment: rounded top-left/right corners (8px), three small traffic-light dots in the top bar, the actual image area uses the upgraded `cc-img-placeholder` gradient + grain.
- For `callouts` variant: numbered pills (1, 2, 3…) positioned absolutely over the image with leader lines (SVG `<line>` elements). Label text appears below/beside the pill. The agent fills (x, y) percentages per callout.
- Image area: `min-height: 50vh; max-height: 80vh` on `split`, `min-height: 60vh` on `centered`.

### 16. `pricing`

**Tier comparison.** 2-4 tiers with feature checkmarks. Conversion-critical for B2B SaaS.

| Variant | Layout |
|---|---|
| `cards` (default) | Each tier in its own card, side-by-side |
| `table` | Full comparison table, features as rows, tiers as columns |

**Placeholders:** `heading`, `eyebrow?`, `tiers[]` (name, price, period, features[] (string), cta_text, cta_href, recommended?).

**Best modes:** swiss, gallery.
**Avoid in:** brutalist.

**Editorial-Generous notes:**
- Recommended tier elevates with a hairline border in `--color-accent-1` and a small "Recommended" eyebrow above the tier name in monospace caps.
- Price typography: `var(--font-monument)` for the number, `var(--font-brutalist)` for the period (e.g., "/mo", "/yr"). Aligned right by leveraging monospace.
- Feature checkmark: inline SVG `<svg viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>` in `--color-accent-1`. Cross for "not included" in `--color-muted`.
- Hover on tier card: subtle lift (`translateY(-2px)`) + hairline accent shift.

### 17. `faq`

**Accordion of Q&A.** Native `<details>`/`<summary>` — no JS framework.

| Variant | Layout |
|---|---|
| `accordion` (default) | One-at-a-time (8 lines of JS to close siblings on open) |
| `static` | All Q&A visible — no toggle behavior |
| `numbered` | Numbered list of Q&A, no toggle |

**Placeholders:** `heading`, `items[]` (question, answer).

**Best modes:** swiss, gallery, immersive.
**Avoid in:** kinetic.

**Editorial-Generous notes:**
- Each item is `<details><summary>question</summary><div>answer</div></details>`.
- `<summary>` styled with `list-style: none; cursor: pointer; padding: 1.5rem 0; border-bottom: 1px solid var(--color-border)`. Custom marker: a `+` glyph that rotates to `×` on `[open]` via CSS transition.
- Answer content: max-width 65ch, `padding-bottom: 1.5rem`, `line-height: 1.6`.
- For `accordion` variant: 8 lines of JS that listen for `toggle` events on `<details>` and close siblings on open. Honors `prefers-reduced-motion` (no animation when reduced).

### 18. `testimonial`

**Quote + author + role + company + avatar slot.** Social proof.

| Variant | Layout |
|---|---|
| `card` (default) | Single testimonial in a bordered card |
| `grid` | 3 testimonials side-by-side |
| `featured` | Large single testimonial with company logo prominent |

**Placeholders:** `testimonials[]` (quote, author, role, company, avatar_slot?).

**Best modes:** all.
**Avoid in:** —

**Editorial-Generous notes:**
- Quote rendered in `var(--font-editorial)` italic. Decorative opening `"` glyph in `--color-accent-1` at 1.4x line-height, positioned to the upper-left of the quote.
- Avatar slot: 56px circular `cc-img-placeholder` variant. When empty, displays the author's monogram (first letter of first + last name) centered, in `var(--font-monument)` at 60% of placeholder size, in `--color-accent-2`.
- Hover on `card` variant: card lifts subtly, border shifts to `--color-accent-2`.

### 19. `signup-form`

**Email input + submit button.** Accessible markup only — no backend wiring.

| Variant | Layout |
|---|---|
| `inline` (default) | Label + input + button on one row |
| `stacked` | Label / input above button |
| `framed` | Entire form in a card with surrounding heading + supporting context |

**Placeholders:** `heading?`, `supporting?`, `placeholder_text`, `button_text`, `action_href`, `field_name?` (default: "email").

**Best modes:** all.
**Avoid in:** brutalist.

**Editorial-Generous notes:**
- `<form>` with `method="POST"`, `action="{{action_href}}"`. Includes hidden `<input name="form_type" value="signup">` for routing on the backend the brand wires up.
- Input: `<input type="email" required autocomplete="email" id="signup-email" name="email" placeholder="{{placeholder_text}}">`. Visible `<label for="signup-email">` — hidden visually with `.visually-hidden` class but present for screen readers (`{{heading}}` is the visible heading; the label is for the input itself).
- Input states: default has hairline border + transparent background. Focus: `outline: 2px solid var(--color-discord); outline-offset: 2px` via `:focus-visible`. Invalid: `border-color: var(--color-discord)` via `:invalid:not(:placeholder-shown)`. Disabled: `opacity: 0.4`.
- Button: matches `.cc-cta__action` style for visual consistency.
- ARIA: `aria-describedby` for any supporting text. Help message in `<small>` below the input.

### 20. `integration-logos`

**Tech-stack / partner logos.** Different from `logo-wall` ("as featured at"); this is "we integrate with."

| Variant | Layout |
|---|---|
| `grid` (default) | Rigid grid with hairline gridlines between cells |
| `inline` | Single horizontal row, equal spacing |
| `categorized` | Logos grouped under category labels |

**Placeholders:** `heading`, `logos[]` (label, href?, category?).

**Best modes:** swiss, gallery.
**Avoid in:** kinetic.

**Editorial-Generous notes:**
- Logos are text-only (label rendered in `var(--font-monument)` or `var(--font-swiss)` — variant-controlled). We do not ship actual logo SVGs. Brand-specific logo SVGs are the brand's responsibility.
- Grid lines: `border-right: 1px solid var(--color-border)` on cells (with `:last-child` exception). Vertical hairlines between rows via `border-bottom`.
- Hover on linked logos: opacity shift from 0.6 to 1.0 at `var(--dur-swift)`.

### 21. `cta-banner`

**Inline CTA strip — or sticky-bottom variant.**

| Variant | Layout |
|---|---|
| `inline` (default) | Sits between sections, hairline above + below |
| `sticky-bottom` | Fixed at bottom on scroll, dismissible (X button) |
| `divider` | Between-sections, more decorative — orange/copper hairline |

**Placeholders:** `heading`, `supporting?`, `action_text`, `action_href`, `dismissible?` (bool, defaults to true for sticky-bottom).

**Best modes:** all.
**Avoid in:** —

**Editorial-Generous notes:**
- `sticky-bottom`: `position: fixed; bottom: 0; left: 0; right: 0; z-index: 90`. Backdrop blur via `backdrop-filter: blur(20px) saturate(180%)`. Hairline border-top. Slide up from bottom via `transform: translateY(100%) → 0` at `var(--dur-natural)` on page load (skipped if `prefers-reduced-motion`).
- Dismiss button: `<button type="button" aria-label="Dismiss">×</button>` in the top-right of the banner. On click, sets `localStorage[<page-key>-cta-banner-dismissed] = '1'`. JS checks key on load; if set, banner stays hidden.
- The localStorage key uses `data-key="<page-slug>"` on the banner element so different pages dismiss independently.

### 22. `comparison-table`

**Feature matrix vs competitors.** High-intent B2B shoppers.

| Variant | Layout |
|---|---|
| `vs-2` (default) | You vs 1 competitor (2-column table) |
| `vs-3` | You vs 2 competitors (3-column table) |
| `vs-many` | You vs N competitors, horizontally scrollable on mobile |

**Placeholders:** `heading`, `columns[]` (name, is_you?), `rows[]` (feature, values[] aligned with columns — each value is a string or "✓"/"—").

**Best modes:** swiss, gallery.
**Avoid in:** brutalist, kinetic.

**Editorial-Generous notes:**
- "You" column highlighted with `background: color-mix(in oklch, var(--color-accent-1) 6%, transparent)` and a hairline accent border. The "is_you" column header in `var(--font-monument)` accent color.
- Checkmark / em-dash for boolean cells. Text strings for nuanced cells. Hairline grid lines.
- On mobile (≤720px): horizontal-scroll the table inside its container. Sticky first column (feature names) via `position: sticky; left: 0`.

---

## The Polish Layer

### `tokens/states.css` — interaction states (~120 lines)

```css
/* ============================================
   STATES — Universal interactive feedback
   ============================================
   Hover, focus-visible, active, disabled, loading on every
   button/link/card/input. Load AFTER all section CSS so
   these win on cascade tie-breaks.
   ============================================ */

/* --- BUTTON / CTA states ------------------------------------ */
button, .cc-cta__action, .cc-nav__cta, [role="button"] {
  min-height: 44px;  /* touch-target */
  cursor: pointer;
  transition:
    transform   var(--dur-swift)   var(--ease-spring),
    background  var(--dur-swift),
    border-color var(--dur-swift),
    color       var(--dur-swift),
    opacity     var(--dur-swift);
}
button:hover, .cc-cta__action:hover { transform: translateY(-2px); }
button:active, .cc-cta__action:active {
  transform: translateY(1px);
  transition-duration: var(--dur-instant);
}
button[disabled], button[aria-disabled="true"] {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
}
button[aria-busy="true"], button[data-loading] {
  cursor: progress;
  pointer-events: none;
}

/* --- LINK states -------------------------------------------- */
a {
  transition: color var(--dur-swift), opacity var(--dur-swift);
}

/* --- FOCUS-VISIBLE rings (keyboard nav only) --------------- */
:focus-visible {
  outline: 2px solid var(--color-discord);
  outline-offset: 2px;
  border-radius: 2px;
}

/* --- FORM input states -------------------------------------- */
input, textarea, select {
  min-height: 44px;
  padding: 0.75rem 1rem;
  border: 1px solid var(--color-border);
  background: transparent;
  color: var(--color-fg);
  font-family: var(--font-swiss);
  font-size: var(--type-body);
  transition: border-color var(--dur-swift), background var(--dur-swift);
}
input::placeholder, textarea::placeholder { color: var(--color-muted); }
input:hover, textarea:hover, select:hover { border-color: var(--color-fg); }
input:focus-visible, textarea:focus-visible, select:focus-visible {
  outline: 2px solid var(--color-discord);
  outline-offset: 2px;
  border-color: var(--color-fg);
}
input:invalid:not(:placeholder-shown) {
  border-color: var(--color-discord);
}
input[disabled], textarea[disabled] { opacity: 0.4; cursor: not-allowed; }

/* --- LOADING (button pulse) -------------------------------- */
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

/* --- VISUALLY HIDDEN (a11y for screen-reader-only labels) -- */
.visually-hidden {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
  border: 0;
}

/* --- REDUCED MOTION ---------------------------------------- */
@media (prefers-reduced-motion: reduce) {
  button[aria-busy="true"]::after,
  button[data-loading]::after {
    animation: none;
  }
}
```

### Premium motion additions

Added to `sections/sections.css`:

- **Scroll-progress hairline** at top of viewport, using `animation-timeline: scroll()` with `@supports` fallback (no bar in older browsers — graceful):

```css
@supports (animation-timeline: scroll()) {
  body::before {
    content: "";
    position: fixed; top: 0; left: 0; right: 0;
    height: 3px;
    background: var(--color-accent-1);
    z-index: 1000;
    transform-origin: left;
    animation: cc-scroll-progress linear both;
    animation-timeline: scroll();
  }
  @keyframes cc-scroll-progress {
    from { transform: scaleX(0); }
    to   { transform: scaleX(1); }
  }
}
@media (prefers-reduced-motion: reduce) {
  body::before { display: none; }
}
```

- **Smooth-scroll with sticky-nav offset:**

```css
html {
  scroll-behavior: smooth;
  scroll-padding-top: calc(var(--nav-height, 64px) + 1rem);
}
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
}
```

- **Section enter directions** documented per section type via existing `motion-reveal` + `motion-stagger` classes. No new keyframes needed.

### Image-placeholder upgrade

Replaces flat-gray in `sections/sections.css`:

```css
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
}
```

---

## Template Scaffolding

### `templates/page-composed.html` `<head>` upgrade

Includes the following Mustache placeholders the agent fills from the brief (NOT from `style.json` — schema stays vibe-only):

- `{{seo.title}}`, `{{seo.description}}`, `{{seo.canonical_url}}`
- `{{seo.og_title}}`, `{{seo.og_description}}`, `{{seo.og_image_url}}`, `{{seo.og_image_alt}}`, `{{seo.og_url}}`, `{{seo.og_type}}` (default "website"), `{{seo.og_site_name}}`
- `{{seo.twitter_card}}` (default "summary_large_image"), `{{seo.twitter_title}}`, `{{seo.twitter_description}}`, `{{seo.twitter_image}}`, `{{seo.twitter_site?}}`
- `{{seo.theme_color}}` (defaults to the page's `--color-bg`)
- `{{org.name}}`, `{{org.url}}`, `{{org.logo_url}}`, `{{org.same_as[]}}` for JSON-LD Organization

A separate `templates/meta.partial.html` carries the same stack as a copy-pasteable snippet for brands that want to extract it.

### `templates/404.html`

Renders with the same style.json that drives the rest of the brand. Structure:

```
<hero data-variant="editorial">
  heading: "404 · the room is dark."  (brand-tunable)
  subhead: "the page you wanted didn't make it past the door."
</hero>
<cta data-variant="default">
  heading: "back to the surface."
  action: "↩  Home"  → "/"
</cta>
<footer>
```

Three sections. Standalone HTML page that links the same `<link>` chain as `page-composed.html`.

### `templates/privacy.html` and `templates/terms.html`

Placeholder structure with editorial typography. The page is built from `manifesto` (sentence-case heading), `flow` body container, and `footer`. Lorem-style placeholder content the brand replaces with real policy text.

### `templates/meta.partial.html`

Just the `<head>` `<meta>` stack with Mustache placeholders. Useful for brands building outside the `page-composed.html` scaffold but who want CC's SEO/OG defaults.

---

## SF Vibe Fest re-render (proof point)

Once everything above lands, re-render `showcase/test-sf-vibe-fest.html` using the new conventions:

**15-section structure:**

1. `nav` (sticky) — `SF VIBE FEST` brand, links: Program · Sponsors · Civic · Apply
2. `hero` (editorial) — "The future gets a neighborhood." [unchanged]
3. `manifesto` — "San Francisco has always been..." [unchanged]
4. `stat-grid` — proof cards [unchanged]
5. `feature-grid` (default, NEW) — 6 cards: Capital Summit · Core Conference · Civic Festival · Neighborhood Nights · Civic Return · Fog City Futurism
6. `product-showcase` (callouts, NEW) — "City as campus" annotated map with callouts: Moscone, Embarcadero, Dogpatch, Mission, Castro
7. `quote` (gutter) — "If you aren't in the room..." [unchanged]
8. `process` (timeline) — Seven nights, seven neighborhoods [unchanged]
9. `testimonial` (featured, NEW) — Host-committee placeholder (attribution: "Host Committee" until real names lock — per v2 style guide's no-fake-bios rule)
10. `pricing` (cards, NEW) — Three tiers: Public Festival (free) / Core Conference (apply) / Capital Summit (apply). All CTAs say "Apply" — SF Vibe Fest is invitation-tiered, not commerce
11. `integration-logos` (grid, NEW) — Venue labels: Moscone · SFMOMA · SHACK15 · Exploratorium · Ferry Building · Dogpatch Power Station · Fort Mason · Frontier Tower
12. `full-bleed` (type) — "The future does not need another convention..." [unchanged]
13. `faq` (accordion, NEW) — Common questions: How do I get a pass? · Is it public? · What's the civic return? · When do block parties happen?
14. `cta-banner` (sticky-bottom, NEW) — "Once a year, the room opens." → mailto:jordan@shiny.new (dismissible)
15. `footer` (editorial) — [unchanged]

**Honest constraints the re-render must honor (per the SF Vibe Fest style guide's "donts"):**

- **No fake testimonial bios.** Use "Host Committee" as the attribution. No fabricated names, no fake photos.
- **No real pricing.** Currency-formatted tiers would lie about the festival's actual invitation-only structure. Tiers show "Apply" as the CTA.
- **No competitor comparison.** `comparison-table` is skipped for SF Vibe Fest specifically — the festival has no direct competitor a comparison makes sense against. The section ships in the skill for other brands.

---

## Documentation Updates

### `SKILL.md`

Triggers extended with the new section names (`nav`, `feature-grid`, `product-showcase`, `pricing`, `faq`, `testimonial`, `signup-form`, `integration-logos`, `cta-banner`, `comparison-table`). One new note under "Two Workflows":

> **SMB tech baseline:** The default polish baseline is *Editorial-Generous* (Stripe / Anthropic / Cursor-marketing DNA). Section variants are tuned for this baseline; the polish layer in `tokens/states.css` makes every interactive element feel shipping-grade out of the box.

### `README.md`

Three reference tables expanded:

- **Section catalog** grows from 12 to 22 entries (mirrors `sections/README.md`).
- **Polish layer** new table: hover / focus-visible / active / disabled / loading / touch-target / scroll-progress / smooth-scroll / image-placeholder.
- **Template scaffolding** new table: page-composed.html / 404.html / privacy.html / terms.html / meta.partial.html.

### `CLAUDE.md`

One new bullet under "How to Use This System":

> **SMB tech baseline is opt-in via class adoption.** The polish layer in `tokens/states.css` applies automatically when included in the `<link>` chain. Section partials use the Editorial-Generous variant defaults; choose other variants per brand context.

### `sections/README.md`

Catalog grows to 22 entries. Each new section gets its row in the table with variants, required placeholders, best/avoid modes, link to the partial.

---

## Acceptance Criteria

The v2 ships when:

1. **All 10 new section partials present** in `sections/`, each with required content slots, `data-pattern` + `data-variant`, top-comment mode-compatibility, and the default variant rendering correctly across all 5 modes × 9 moods.
2. **`sections/README.md` catalog updated** — 22 entries.
3. **`tokens/states.css` shipped** — interaction states, touch-targets, focus-visible rings. Reduced motion respected.
4. **Image-placeholder upgrade visible** — old flat-gray gone everywhere; new gradient+grain treatment with bottom-left label.
5. **Premium motion live** — scroll-progress hairline at top (graceful fallback in older browsers), smooth-scroll honors nav offset, reduced-motion respected.
6. **`templates/page-composed.html` `<head>` expanded** — full SEO/OG/Twitter/JSON-LD with Mustache placeholders.
7. **`templates/404.html`, `privacy.html`, `terms.html`, `meta.partial.html`** exist and render with the same `style.json` that drives the rest of the brand.
8. **SF Vibe Fest re-render** — 15 sections, all rendering cleanly at desktop AND mobile (320 / 375 / 768 / 1024 / 1440). No horizontal overflow. No mustache placeholder leaks. No console errors. Browser-verified at minimum desktop + mobile.
9. **Lovetel still renders identically** — the dusk-quiet adapter test is unaffected; polish layer is additive.
10. **Five existing showcase pages still render** — backwards compatibility.
11. **A11y baseline pass** — every interactive element keyboard-reachable with visible focus ring; touch-targets ≥44px; `prefers-reduced-motion` honored; semantic HTML (`<nav>`, `<main>`, `<details>`, `<form>` with labels).
12. **Pushed to all three remotes** — `git pushall` after explicit user authorization.

---

## Open Questions for Implementation

None outstanding at spec time. Anything that surfaces during implementation should be raised before merging.

---

## Related

- `~/.claude/skills/controlledchaos/docs/superpowers/specs/2026-05-20-section-library-and-style-guide-adapter-design.md` — the v1 spec this builds on.
- `~/.claude/skills/controlledchaos/AXIOMS.md` — design first principles.
- `~/.claude/skills/controlledchaos/DESIGN.md` — original system overview.
- `~/.claude/skills/controlledchaos/adapters/examples/sf-vibe-fest.json` — the canary brand's style.
- `~/.claude/skills/controlledchaos/adapters/examples/lovetel.json` — second adapter test, must stay unaffected.
- `~/.claude/projects/-Users-nayslayer/memory/project_controlledchaos_style_guides.md` — locked decision: style-guide source images are reference-only, never embedded.
