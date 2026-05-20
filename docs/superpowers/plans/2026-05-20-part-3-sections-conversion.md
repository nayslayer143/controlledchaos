# Part 3: Conversion-Critical Sections (Tasks 8–11)

Prev: [part-2-templates.md](2026-05-20-part-2-templates.md). Next: [part-4-sections-trust.md](2026-05-20-part-4-sections-trust.md).

Four sections SMB tech pages can't go to market without: `nav`, `feature-grid`, `product-showcase`, `pricing`. Each follows the same authoring contract as the existing 12: top-comment with best/ok/avoid + variants, `data-pattern` + `data-variant`, Mustache placeholders, CSS appended to `sections/sections.css`.

**Common conventions across all 10 new sections:**
- Variants `default` is listed first in the top-comment and is the value applied as the partial's initial `data-variant`.
- All component CSS targets `.cc-<name>` selectors. Per-variant overrides use `[data-variant="<name>"]`.
- New CSS gets appended after the existing `/* end-marquee */` marker in `sections/sections.css`. Each new block ends with `/* end-<name> */` for future-safe Edit anchoring.

---

## Task 8: `nav` section

**Files:**
- Create: `sections/nav.html`
- Modify: `sections/sections.css` (append after `/* end-marquee */`)

- [ ] **Step 1: Write `sections/nav.html`**

```html
<!-- cc-section: nav
     best:  all (with per-mode density tuning)
     ok:    -
     avoid: -
     variants: default (logo left + links right), centered (logo center), minimal (logo + 1 CTA)
     placeholders: brand, links[] (label, href), cta_text, cta_href
     motion: sets --nav-height via inline style; uses 8 lines of JS for mobile drawer + scroll-state -->
<nav class="cc-section cc-nav"
     data-pattern="nav"
     data-variant="default"
     aria-label="Primary">
  <div class="container --wide cc-nav__inner">
    <a class="cc-nav__brand voice-monument" href="/">{{brand}}</a>

    <button class="cc-nav__toggle" type="button"
            aria-label="Open menu"
            aria-expanded="false"
            aria-controls="cc-nav-menu">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </button>

    <div class="cc-nav__menu" id="cc-nav-menu">
      <ul class="cc-nav__links">
        {{#links}}
        <li><a class="cc-nav__link voice-swiss --label" href="{{href}}">{{label}}</a></li>
        {{/links}}
      </ul>
      <a class="cc-nav__cta voice-swiss --label" href="{{cta_href}}">{{cta_text}}</a>
    </div>
  </div>
</nav>

<script>
  (function () {
    var nav = document.currentScript.previousElementSibling;
    if (!nav || !nav.classList.contains('cc-nav')) return;
    var toggle = nav.querySelector('.cc-nav__toggle');
    var menu = nav.querySelector('.cc-nav__menu');

    // Set --nav-height for scroll-padding-top in tokens
    var h = nav.offsetHeight || 64;
    document.documentElement.style.setProperty('--nav-height', h + 'px');

    // Mobile drawer toggle
    toggle.addEventListener('click', function () {
      var open = nav.toggleAttribute('data-open');
      toggle.setAttribute('aria-expanded', open ? 'true' : 'false');
      toggle.setAttribute('aria-label', open ? 'Close menu' : 'Open menu');
    });

    // Close on Escape or link click
    nav.addEventListener('keydown', function (e) {
      if (e.key === 'Escape' && nav.hasAttribute('data-open')) {
        nav.removeAttribute('data-open');
        toggle.setAttribute('aria-expanded', 'false');
        toggle.focus();
      }
    });
    menu.addEventListener('click', function (e) {
      if (e.target.closest('a') && nav.hasAttribute('data-open')) {
        nav.removeAttribute('data-open');
        toggle.setAttribute('aria-expanded', 'false');
      }
    });

    // Scrolled-state for backdrop blur
    var onScroll = function () {
      if (window.scrollY > 4) nav.setAttribute('data-scrolled', '');
      else nav.removeAttribute('data-scrolled');
    };
    window.addEventListener('scroll', onScroll, { passive: true });
    onScroll();
  })();
</script>
```

- [ ] **Step 2: Append nav CSS to `sections/sections.css` (after `/* end-marquee */`)**

```css

/* --- nav ---------------------------------------------------- */
.cc-nav {
  position: sticky;
  top: 0;
  z-index: 100;
  padding-block: 0.75rem;
  background: color-mix(in oklch, var(--color-bg) 90%, transparent);
  transition: background var(--dur-swift), border-color var(--dur-swift);
  border-bottom: 1px solid transparent;
}
.cc-nav[data-scrolled] {
  background: color-mix(in oklch, var(--color-bg) 70%, transparent);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-bottom-color: var(--color-border);
}
.cc-nav__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  min-height: 44px;
}
.cc-nav__brand {
  font-size: var(--type-subhead);
  font-weight: 700;
  letter-spacing: -0.01em;
  color: var(--color-fg);
  text-decoration: none;
}
.cc-nav__menu {
  display: flex;
  align-items: center;
  gap: 2rem;
}
.cc-nav__links {
  display: flex;
  gap: 2rem;
  list-style: none;
  margin: 0; padding: 0;
}
.cc-nav__link {
  color: var(--color-fg);
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  padding: 0.5rem 0;
  position: relative;
}
.cc-nav__link::after {
  content: ""; position: absolute; left: 0; right: 0; bottom: 0;
  height: 1px; background: var(--color-fg);
  transform: scaleX(0); transform-origin: left;
  transition: transform var(--dur-swift);
}
.cc-nav__link:hover::after { transform: scaleX(1); }
.cc-nav__cta {
  display: inline-flex; align-items: center;
  padding: 0.625rem 1.25rem;
  border: 1px solid var(--color-fg);
  color: var(--color-fg);
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}
.cc-nav__cta:hover {
  background: var(--color-fg);
  color: var(--color-bg);
}
.cc-nav__toggle {
  display: none;
  background: transparent;
  border: 0;
  padding: 0.5rem;
  cursor: pointer;
  flex-direction: column;
  justify-content: space-around;
  width: 44px; height: 44px;
}
.cc-nav__toggle span {
  display: block;
  height: 2px;
  width: 22px;
  background: var(--color-fg);
  margin: 2px 0;
  transition: transform var(--dur-swift), opacity var(--dur-swift);
}
.cc-nav[data-open] .cc-nav__toggle span:nth-child(1) { transform: translateY(6px) rotate(45deg); }
.cc-nav[data-open] .cc-nav__toggle span:nth-child(2) { opacity: 0; }
.cc-nav[data-open] .cc-nav__toggle span:nth-child(3) { transform: translateY(-6px) rotate(-45deg); }

/* Centered variant: logo center, links flanking */
.cc-nav[data-variant="centered"] .cc-nav__inner {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
}
.cc-nav[data-variant="centered"] .cc-nav__brand { grid-column: 2; }
.cc-nav[data-variant="centered"] .cc-nav__menu { grid-column: 3; justify-self: end; }

/* Minimal variant: logo + 1 CTA only */
.cc-nav[data-variant="minimal"] .cc-nav__links { display: none; }

/* Mobile: hamburger drawer */
@media (max-width: 720px) {
  .cc-nav__toggle { display: flex; }
  .cc-nav__menu {
    position: fixed;
    inset: var(--nav-height, 64px) 0 0 0;
    background: var(--color-bg);
    flex-direction: column;
    align-items: flex-start;
    padding: 2rem;
    gap: 1.5rem;
    transform: translateX(100%);
    transition: transform var(--dur-natural) var(--ease-out-expo);
    overflow-y: auto;
  }
  .cc-nav[data-open] .cc-nav__menu { transform: translateX(0); }
  .cc-nav__links { flex-direction: column; gap: 1.5rem; }
  .cc-nav__link { font-size: var(--type-headline); padding: 0.75rem 0; }
  .cc-nav__cta { width: 100%; justify-content: center; padding: 1rem; }
}
@media (prefers-reduced-motion: reduce) {
  .cc-nav__menu { transition: none; }
}
/* end-nav */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/nav.html sections/sections.css
git commit -m "Add nav section partial (default/centered/minimal variants)

Sticky top bar with backdrop-blur scrolled state. Mobile hamburger
collapses to a full-height right-drawer overlay. Sets --nav-height
CSS var on load so smooth-scroll padding works. Honors Escape +
link-click + reduced-motion.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 9: `feature-grid` section

**Files:**
- Create: `sections/feature-grid.html`
- Modify: `sections/sections.css` (append after `/* end-nav */`)

- [ ] **Step 1: Write `sections/feature-grid.html`**

```html
<!-- cc-section: feature-grid
     best:  swiss, gallery, immersive
     ok:    -
     avoid: brutalist (loud fights feature-density), kinetic (motion fights the read)
     variants: default (3-col), wide (2-col, longer body), compact (4-col)
     placeholders: heading, eyebrow (optional), features[] (icon (raw SVG), title, body)
     icons: agent fills inline SVG from sections/icons.md curated set -->
<section class="cc-section cc-feature-grid"
         data-pattern="feature-grid"
         data-variant="default">
  <div class="container --wide flow motion-reveal">
    <span class="cc-feature-grid__eyebrow voice-swiss --label">{{eyebrow}}</span>
    <h2 class="cc-feature-grid__heading voice-monument --display">{{heading}}</h2>
    <div class="cc-feature-grid__items motion-stagger">
      {{#features}}
      <div class="cc-feature">
        <div class="cc-feature__icon" aria-hidden="true">{{{icon}}}</div>
        <h3 class="cc-feature__title voice-monument">{{title}}</h3>
        <p class="cc-feature__body voice-swiss">{{body}}</p>
      </div>
      {{/features}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append feature-grid CSS to `sections/sections.css`**

```css

/* --- feature-grid ------------------------------------------ */
.cc-feature-grid__eyebrow {
  display: block;
  color: var(--color-accent-1);
  font-family: var(--font-brutalist);
  letter-spacing: 0.18em;
  margin-bottom: 1rem;
}
.cc-feature-grid__heading {
  max-width: 28ch;
  margin: 0 0 3rem;
  line-height: 1.1;
}
.cc-feature-grid__items {
  display: grid;
  gap: clamp(2rem, 4vw, 4rem);
  grid-template-columns: repeat(3, 1fr);
}
.cc-feature-grid[data-variant="wide"] .cc-feature-grid__items { grid-template-columns: repeat(2, 1fr); }
.cc-feature-grid[data-variant="compact"] .cc-feature-grid__items { grid-template-columns: repeat(4, 1fr); }
@media (max-width: 960px) {
  .cc-feature-grid[data-variant="compact"] .cc-feature-grid__items { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 720px) {
  .cc-feature-grid__items,
  .cc-feature-grid[data-variant="wide"] .cc-feature-grid__items,
  .cc-feature-grid[data-variant="compact"] .cc-feature-grid__items { grid-template-columns: 1fr; }
}
.cc-feature {
  min-width: 0;
  padding-top: 1.5rem;
  border-top: 1px solid var(--color-border);
  transition: transform var(--dur-swift);
}
.cc-feature:hover { transform: translateY(-2px); }
.cc-feature__icon {
  width: 32px; height: 32px;
  color: var(--color-accent-1);
  margin-bottom: 1.25rem;
  transition: color var(--dur-swift);
}
.cc-feature:hover .cc-feature__icon { color: var(--color-discord); }
.cc-feature__icon svg { width: 100%; height: 100%; stroke: currentColor; fill: none; stroke-width: 1.75; stroke-linecap: round; stroke-linejoin: round; }
.cc-feature__title {
  font-size: var(--type-headline);
  margin: 0 0 0.5rem;
  line-height: 1.2;
}
.cc-feature__body {
  font-size: var(--type-body);
  color: var(--color-accent-2);
  max-width: 38ch;
  margin: 0;
}
.cc-feature-grid[data-variant="wide"] .cc-feature__body { max-width: 50ch; }
/* end-feature-grid */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/feature-grid.html sections/sections.css
git commit -m "Add feature-grid section partial (default/wide/compact variants)

3/2/4-column responsive grid. Icon slot accepts inline SVG (agent
fills from sections/icons.md). Hairline top border per card, hover
lift, icon color shift to discord on hover. Collapses to 2-col at
960px, 1-col at 720px.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 10: `product-showcase` section

**Files:**
- Create: `sections/product-showcase.html`
- Modify: `sections/sections.css` (append after `/* end-feature-grid */`)

- [ ] **Step 1: Write `sections/product-showcase.html`**

```html
<!-- cc-section: product-showcase
     best:  gallery, immersive
     ok:    swiss
     avoid: brutalist
     variants: split (default — image left, copy right), centered (copy above, full-width image below), callouts (image with numbered annotation pills)
     placeholders: heading, body, image_slot, callouts[] (num, label, x_pct, y_pct) for callouts variant -->
<section class="cc-section cc-showcase"
         data-pattern="product-showcase"
         data-variant="split">
  <div class="container --wide">
    <div class="cc-showcase__grid motion-reveal">
      <div class="cc-showcase__copy">
        <h2 class="cc-showcase__heading voice-monument --display">{{heading}}</h2>
        <p class="cc-showcase__body voice-swiss">{{body}}</p>
      </div>
      <div class="cc-showcase__frame">
        <div class="cc-showcase__chrome" aria-hidden="true">
          <span></span><span></span><span></span>
        </div>
        <div class="cc-img-placeholder cc-showcase__image" data-slot="{{image_slot}}"></div>
        {{#callouts}}
        <button class="cc-showcase__callout"
                style="left:{{x_pct}}%;top:{{y_pct}}%"
                aria-label="{{label}}">
          <span class="cc-showcase__callout-num">{{num}}</span>
          <span class="cc-showcase__callout-label">{{label}}</span>
        </button>
        {{/callouts}}
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append product-showcase CSS**

```css

/* --- product-showcase -------------------------------------- */
.cc-showcase__grid {
  display: grid;
  grid-template-columns: minmax(0, 0.8fr) minmax(0, 1.2fr);
  gap: clamp(2rem, 5vw, 6rem);
  align-items: center;
}
@media (max-width: 960px) { .cc-showcase__grid { grid-template-columns: 1fr; } }
.cc-showcase__heading { margin: 0 0 1.5rem; max-width: 22ch; line-height: 1.1; }
.cc-showcase__body { font-size: var(--type-body); color: var(--color-accent-2); max-width: 48ch; }
.cc-showcase__frame {
  position: relative;
  border-radius: 8px 8px 0 0;
  background: color-mix(in oklch, var(--color-fg) 10%, var(--color-bg));
  overflow: hidden;
  box-shadow:
    0 1px 0 color-mix(in oklch, var(--color-fg) 15%, transparent),
    0 24px 60px -20px color-mix(in oklch, var(--color-fg) 25%, transparent);
}
.cc-showcase__chrome {
  display: flex;
  gap: 0.5rem;
  padding: 0.625rem 1rem;
  background: color-mix(in oklch, var(--color-fg) 6%, var(--color-bg));
  border-bottom: 1px solid var(--color-border);
}
.cc-showcase__chrome span {
  width: 10px; height: 10px;
  border-radius: 50%;
  background: color-mix(in oklch, var(--color-fg) 20%, transparent);
}
.cc-showcase__chrome span:nth-child(1) { background: #ee5e3a; }
.cc-showcase__chrome span:nth-child(2) { background: #e8a647; }
.cc-showcase__chrome span:nth-child(3) { background: #3aa896; }
.cc-showcase__image {
  aspect-ratio: 16 / 10;
  min-height: 0;
  border-radius: 0;
  margin: 0;
}

/* Centered variant: copy above, image full-width below */
.cc-showcase[data-variant="centered"] .cc-showcase__grid {
  grid-template-columns: 1fr;
  text-align: center;
}
.cc-showcase[data-variant="centered"] .cc-showcase__copy { max-width: 60ch; margin: 0 auto 3rem; }
.cc-showcase[data-variant="centered"] .cc-showcase__heading { margin-left: auto; margin-right: auto; }
.cc-showcase[data-variant="centered"] .cc-showcase__body { margin-left: auto; margin-right: auto; }

/* Callouts variant: numbered pills with absolute positioning */
.cc-showcase__callout {
  position: absolute;
  background: var(--color-accent-1);
  color: var(--color-bg);
  border: 0;
  border-radius: 999px;
  padding: 0.375rem 0.875rem 0.375rem 0.375rem;
  display: inline-flex; align-items: center; gap: 0.5rem;
  font-family: var(--font-brutalist);
  font-size: var(--type-caption);
  cursor: pointer;
  transform: translate(-50%, -50%);
}
.cc-showcase__callout-num {
  display: inline-flex; align-items: center; justify-content: center;
  width: 22px; height: 22px;
  border-radius: 50%;
  background: var(--color-bg);
  color: var(--color-accent-1);
  font-weight: 700;
}
.cc-showcase__callout-label { text-transform: uppercase; letter-spacing: 0.1em; }
/* end-product-showcase */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/product-showcase.html sections/sections.css
git commit -m "Add product-showcase section partial (split/centered/callouts variants)

Browser-chrome-framed mockup placeholder (rounded top, traffic-light
dots in international-orange/amber/bay-green). Image area uses the
upgraded gradient+grain cc-img-placeholder. Callouts variant adds
absolute-positioned numbered pills (x_pct/y_pct from agent).
Collapses to single column at 960px.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 11: `pricing` section

**Files:**
- Create: `sections/pricing.html`
- Modify: `sections/sections.css` (append after `/* end-product-showcase */`)

- [ ] **Step 1: Write `sections/pricing.html`**

```html
<!-- cc-section: pricing
     best:  swiss, gallery
     ok:    -
     avoid: brutalist
     variants: cards (default — each tier in its own card), table (full comparison matrix)
     placeholders: heading, eyebrow (optional), tiers[] (name, price, period, features[] (string), cta_text, cta_href, recommended (bool)) -->
<section class="cc-section cc-pricing"
         data-pattern="pricing"
         data-variant="cards">
  <div class="container --wide flow motion-reveal">
    <span class="cc-pricing__eyebrow voice-swiss --label">{{eyebrow}}</span>
    <h2 class="cc-pricing__heading voice-monument --display">{{heading}}</h2>
    <div class="cc-pricing__tiers motion-stagger">
      {{#tiers}}
      <div class="cc-tier {{#recommended}}cc-tier--recommended{{/recommended}}">
        {{#recommended}}<span class="cc-tier__badge voice-swiss --label">Recommended</span>{{/recommended}}
        <h3 class="cc-tier__name voice-monument">{{name}}</h3>
        <p class="cc-tier__price">
          <span class="cc-tier__amount voice-monument">{{price}}</span>
          <span class="cc-tier__period voice-brutalist">{{period}}</span>
        </p>
        <ul class="cc-tier__features">
          {{#features}}
          <li class="voice-swiss">
            <svg class="cc-tier__check" viewBox="0 0 24 24" aria-hidden="true"><path d="M5 12l4 4 10-10"/></svg>
            {{.}}
          </li>
          {{/features}}
        </ul>
        <a class="cc-tier__cta voice-swiss --label" href="{{cta_href}}">{{cta_text}}</a>
      </div>
      {{/tiers}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append pricing CSS**

```css

/* --- pricing ----------------------------------------------- */
.cc-pricing__eyebrow {
  display: block;
  color: var(--color-accent-1);
  font-family: var(--font-brutalist);
  letter-spacing: 0.18em;
  margin-bottom: 1rem;
}
.cc-pricing__heading { margin: 0 0 3rem; max-width: 28ch; line-height: 1.1; }
.cc-pricing__tiers {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}
.cc-tier {
  position: relative;
  padding: 2rem;
  border: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  min-width: 0;
  transition: transform var(--dur-swift), border-color var(--dur-swift);
}
.cc-tier:hover { transform: translateY(-2px); border-color: var(--color-accent-2); }
.cc-tier--recommended {
  border-color: var(--color-accent-1);
  border-width: 2px;
}
.cc-tier__badge {
  position: absolute;
  top: -0.625rem; left: 1.5rem;
  background: var(--color-accent-1);
  color: var(--color-bg);
  padding: 0.25rem 0.75rem;
  font-family: var(--font-brutalist);
  letter-spacing: 0.15em;
  text-transform: uppercase;
}
.cc-tier__name {
  font-size: var(--type-headline);
  margin: 0 0 1.5rem;
  letter-spacing: -0.01em;
}
.cc-tier__price { margin: 0 0 1.5rem; display: flex; align-items: baseline; gap: 0.5rem; }
.cc-tier__amount {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  letter-spacing: -0.03em;
  line-height: 1;
}
.cc-tier__period { color: var(--color-muted); font-size: var(--type-body-small); }
.cc-tier__features { list-style: none; padding: 0; margin: 0 0 2rem; display: grid; gap: 0.75rem; }
.cc-tier__features li {
  display: flex; align-items: flex-start; gap: 0.625rem;
  font-size: var(--type-body);
  color: var(--color-accent-2);
}
.cc-tier__check {
  width: 18px; height: 18px;
  flex: 0 0 18px;
  margin-top: 0.25em;
  stroke: var(--color-accent-1);
  fill: none;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}
.cc-tier__cta {
  margin-top: auto;
  padding: 0.875rem 1.25rem;
  border: 1px solid var(--color-fg);
  color: var(--color-fg);
  text-decoration: none;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  transition: background var(--dur-swift), color var(--dur-swift);
}
.cc-tier__cta:hover { background: var(--color-fg); color: var(--color-bg); }
.cc-tier--recommended .cc-tier__cta {
  background: var(--color-accent-1);
  border-color: var(--color-accent-1);
  color: var(--color-bg);
}
.cc-tier--recommended .cc-tier__cta:hover {
  background: var(--color-fg);
  border-color: var(--color-fg);
}

/* Table variant: render as comparison matrix (features as rows, tiers as columns).
   The agent uses different markup for this variant — see comparison-table partial
   if you need a competitor matrix instead. */
.cc-pricing[data-variant="table"] .cc-pricing__tiers {
  grid-template-columns: 1fr;
  gap: 0;
}
.cc-pricing[data-variant="table"] .cc-tier {
  display: grid;
  grid-template-columns: 1.2fr 1fr 1fr;
  align-items: start;
  border: 0;
  border-top: 1px solid var(--color-border);
  padding: 1.5rem 0;
}
.cc-pricing[data-variant="table"] .cc-tier:hover { transform: none; }
/* end-pricing */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/pricing.html sections/sections.css
git commit -m "Add pricing section partial (cards/table variants)

Auto-fit tier grid (240px min cards). Recommended tier elevates
with accent border + Recommended badge + filled CTA. Custom SVG
checkmarks per feature. Hover lift on cards. Table variant
restructures into a single-column comparison.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

End of Part 3. Continue to [part-4-sections-trust.md](2026-05-20-part-4-sections-trust.md).
