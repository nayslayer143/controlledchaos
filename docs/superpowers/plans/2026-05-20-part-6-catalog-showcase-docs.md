# Part 6: Catalog + Showcase + Docs + Verify + Push (Tasks 18–23)

Prev: [part-5-sections-utility.md](2026-05-20-part-5-sections-utility.md). Final part.

The wrap-up: ship the icon reference, expand the section catalog, re-render SF Vibe Fest with the new components, update the three documentation files, verify backwards compatibility, and push to GitHub.

---

## Task 18: Icon reference snippets

**Files:**
- Create: `sections/icons.md`

- [ ] **Step 1: Write `sections/icons.md`**

```markdown
# Icon Reference — Inline SVG Snippets

Use these stroke-based icons inside `feature-grid` `{{{icon}}}` slots, `pricing` features,
and anywhere else a small symbol earns its room. All are 24x24, `stroke="currentColor"`,
`fill="none"`, `stroke-width="1.75"`, `stroke-linecap="round"`, `stroke-linejoin="round"`.

The agent picks the icon that matches the feature's meaning. If none fit, the agent
authors a new SVG in the same style.

## Curated set

### Direction / motion
```html
<svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
```
Arrow right — navigation, "next."

```html
<svg viewBox="0 0 24 24"><path d="M12 5v14M6 13l6 6 6-6"/></svg>
```
Arrow down — descent, deeper.

### Convergence
```html
<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M12 3v4M12 17v4M3 12h4M17 12h4M5.6 5.6l2.8 2.8M15.6 15.6l2.8 2.8M5.6 18.4l2.8-2.8M15.6 8.4l2.8-2.8"/></svg>
```
Hub — center with rays. "The room."

### Stack / layers
```html
<svg viewBox="0 0 24 24"><path d="M12 2l10 5-10 5L2 7l10-5zM2 12l10 5 10-5M2 17l10 5 10-5"/></svg>
```
Stack — three layers. Infrastructure / platform.

### Speed
```html
<svg viewBox="0 0 24 24"><path d="M13 2L3 14h7v8l10-12h-7V2z"/></svg>
```
Bolt — speed, instant.

### Trust
```html
<svg viewBox="0 0 24 24"><path d="M12 2L3 7v6c0 5 4 9 9 10 5-1 9-5 9-10V7l-9-5z"/><path d="M9 12l2 2 4-4"/></svg>
```
Shield-check — trust, security, verified.

### Network
```html
<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="2"/><circle cx="4" cy="6" r="2"/><circle cx="20" cy="6" r="2"/><circle cx="4" cy="18" r="2"/><circle cx="20" cy="18" r="2"/><path d="M6 7l5 4M18 7l-5 4M6 17l5-4M18 17l-5-4"/></svg>
```
Network — distributed nodes.

### Eye / vision
```html
<svg viewBox="0 0 24 24"><path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7S2 12 2 12z"/><circle cx="12" cy="12" r="3"/></svg>
```
Eye — vision, insight, the needle.

### Globe
```html
<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><path d="M2 12h20M12 2a15 15 0 010 20M12 2a15 15 0 000 20"/></svg>
```
Globe — global, distributed.

### Cards / canvas
```html
<svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M3 9h18M9 21V9"/></svg>
```
Canvas — board, dashboard, layout.

### Plus / add
```html
<svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
```
Plus — add, more.

### Sparkle / signal
```html
<svg viewBox="0 0 24 24"><path d="M12 3l2 6 6 2-6 2-2 6-2-6-6-2 6-2 2-6z"/></svg>
```
Sparkle — magic, signal, AI.

### Map pin
```html
<svg viewBox="0 0 24 24"><path d="M12 2c-4 0-7 3-7 7 0 5 7 13 7 13s7-8 7-13c0-4-3-7-7-7z"/><circle cx="12" cy="9" r="2.5"/></svg>
```
Pin — location, neighborhood, place.

## Usage

In `feature-grid.html` the `{{{icon}}}` placeholder uses triple-stash so the inline SVG isn't HTML-escaped. The agent pastes the SVG directly:

```html
<div class="cc-feature__icon" aria-hidden="true">
  <svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
</div>
```

The icon inherits `color: var(--color-accent-1)` from the wrapping `.cc-feature__icon`, and `currentColor` cascades into the SVG's `stroke`.

## Authoring more

If none of the curated 13 match, author a new one in the same style:
- 24x24 viewBox
- Single `<path>` or minimal primitives
- `stroke="currentColor" fill="none" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round"` (these are set globally in `sections.css` on `.cc-feature__icon svg`, so the SVG markup can omit them)
- No `<title>`, no `<defs>`, no styles — keep the markup minimal

Add the new SVG to this file under a sensible category so the next agent can find it.
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/icons.md
git commit -m "Add sections/icons.md — 13 reference inline SVG snippets

Stroke-based, 24x24, currentColor for color inheritance. Grouped
by intent (direction, convergence, stack, speed, trust, network,
eye, globe, canvas, plus, sparkle, pin). Authoring guidance for
adding new icons. Lets the agent fill feature-grid icon slots
without inventing inconsistent symbols.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 19: Update `sections/README.md` catalog (22 entries)

**Files:**
- Modify: `sections/README.md`

- [ ] **Step 1: Replace the existing catalog table with the 22-entry version**

Open `sections/README.md`. Find the `## Catalog` section. Replace the table under it with:

```markdown
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
| [full-bleed](full-bleed.html) | type, image, color-field | content / slot_name / content+--bleed-color | immersive, brutalist, kinetic | — |
| [marquee](marquee.html) | default, slow, vertical | items[] (text) | kinetic, immersive, brutalist | gallery |
| [nav](nav.html) | default, centered, minimal | brand, links[] (label, href), cta_text, cta_href | all | — |
| [feature-grid](feature-grid.html) | default, wide, compact | heading, eyebrow?, features[] (icon, title, body) | swiss, gallery, immersive | brutalist, kinetic |
| [product-showcase](product-showcase.html) | split, centered, callouts | heading, body, image_slot, callouts[] for callouts | gallery, immersive | brutalist |
| [pricing](pricing.html) | cards, table | heading, eyebrow?, tiers[] | swiss, gallery | brutalist |
| [faq](faq.html) | accordion, static, numbered | heading, items[] (question, answer) | swiss, gallery, immersive | kinetic |
| [testimonial](testimonial.html) | card, grid, featured | testimonials[] (quote, author, role, company, monogram?) | all | — |
| [signup-form](signup-form.html) | inline, stacked, framed | heading?, supporting?, placeholder_text, button_text, action_href | all | brutalist |
| [integration-logos](integration-logos.html) | grid, inline, categorized | heading, logos[] (label, href?, category?) | swiss, gallery | kinetic |
| [cta-banner](cta-banner.html) | inline, sticky-bottom, divider | heading, supporting?, action_text, action_href, dismissible?, banner_key? | all | — |
| [comparison-table](comparison-table.html) | vs-2, vs-3, vs-many | heading, eyebrow?, columns[], rows[] | swiss, gallery | brutalist, kinetic |
```

After the table, add this note about icons:

```markdown
## Icons

`feature-grid` and `pricing` use inline SVG icons. Reference set (13 stroke-based icons) lives in [icons.md](icons.md). The agent picks the icon that matches the feature meaning, or authors a new one in the same style and adds it to the reference file.
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/README.md
git commit -m "Update sections/README.md catalog — 22 entries

Existing 12 sections + 10 new (nav, feature-grid, product-showcase,
pricing, faq, testimonial, signup-form, integration-logos,
cta-banner, comparison-table). Each row links to the partial and
lists variants, required placeholders, best/avoid modes. Plus an
Icons section pointing at icons.md for feature-grid/pricing SVGs.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 20: SF Vibe Fest re-render to 15 sections

**Files:**
- Modify: `showcase/test-sf-vibe-fest.html` (full rewrite using the new section library)

This is the proof point. Every new section gets exercised against the Fog City Futurism brand. Honor the spec's constraints: no fake testimonial bios (attribution: "Host Committee"), no real pricing (use "Apply" CTAs), skip comparison-table (festival has no direct competitor).

- [ ] **Step 1: Open `showcase/test-sf-vibe-fest.html` in your editor**

The existing file has the v1 11-section layout. We're going to replace the entire `<body>` content and update the `<head>` to use the new SEO meta stack.

- [ ] **Step 2: Replace `<head>` to add SEO meta + `tokens/states.css` to link chain**

Within `<head>`, ensure the following are present (some may already exist; add what's missing):

- `<title>SF VIBE FEST 2027 — The Future Gets a Neighborhood</title>`
- `<meta name="description" content="A seven-day citywide convergence of capital, code, culture, music, art, and civic imagination. San Francisco, October 25–31, 2027.">`
- `<meta property="og:title" content="SF VIBE FEST 2027">`
- `<meta property="og:description" content="The future gets a neighborhood. October 25–31, 2027.">`
- `<meta property="og:type" content="event">`
- `<meta name="theme-color" content="#f5efe0">`
- Add `<link rel="stylesheet" href="../tokens/states.css">` between `textures.css` and `sections/sections.css`

- [ ] **Step 3: Replace entire `<body>` with the 15-section layout**

```html
<body>

  <!-- 1. NAV -->
  <nav class="cc-section cc-nav" data-pattern="nav" data-variant="default" aria-label="Primary">
    <div class="container --wide cc-nav__inner">
      <a class="cc-nav__brand voice-monument" href="#">SF VIBE FEST</a>
      <button class="cc-nav__toggle" type="button" aria-label="Open menu" aria-expanded="false" aria-controls="cc-nav-menu">
        <span aria-hidden="true"></span><span aria-hidden="true"></span><span aria-hidden="true"></span>
      </button>
      <div class="cc-nav__menu" id="cc-nav-menu">
        <ul class="cc-nav__links">
          <li><a class="cc-nav__link voice-swiss --label" href="#program">Program</a></li>
          <li><a class="cc-nav__link voice-swiss --label" href="#sponsors">Sponsors</a></li>
          <li><a class="cc-nav__link voice-swiss --label" href="#civic">Civic</a></li>
        </ul>
        <a class="cc-nav__cta voice-swiss --label" href="mailto:jordan@shiny.new">Apply</a>
      </div>
    </div>
  </nav>

  <!-- (nav drawer JS — copy from sections/nav.html) -->
  <script>
    (function(){var nav=document.querySelector('.cc-nav');if(!nav)return;var t=nav.querySelector('.cc-nav__toggle'),m=nav.querySelector('.cc-nav__menu');document.documentElement.style.setProperty('--nav-height',(nav.offsetHeight||64)+'px');t.addEventListener('click',function(){var o=nav.toggleAttribute('data-open');t.setAttribute('aria-expanded',o?'true':'false');t.setAttribute('aria-label',o?'Close menu':'Open menu');});nav.addEventListener('keydown',function(e){if(e.key==='Escape'&&nav.hasAttribute('data-open')){nav.removeAttribute('data-open');t.setAttribute('aria-expanded','false');t.focus();}});m.addEventListener('click',function(e){if(e.target.closest('a')&&nav.hasAttribute('data-open')){nav.removeAttribute('data-open');t.setAttribute('aria-expanded','false');}});var s=function(){if(window.scrollY>4)nav.setAttribute('data-scrolled','');else nav.removeAttribute('data-scrolled');};window.addEventListener('scroll',s,{passive:true});s();})();
  </script>

  <!-- 2. HERO (unchanged from v1) -->
  <section class="cc-section cc-hero" data-pattern="hero" data-variant="editorial">
    <div class="container --regular flow motion-reveal">
      <span class="cc-hero__eyebrow voice-swiss --label">SF VIBE FEST · OCT 25–31, 2027 · SAN FRANCISCO</span>
      <h1 class="cc-hero__heading voice-monument --massive">The future gets a neighborhood</h1>
      <p class="cc-hero__subhead voice-swiss">A seven-day citywide convergence of capital, code, culture, music, art, and civic imagination. Anchored at Moscone. Distributed across museums, waterfronts, civic plazas, cultural institutions, startup spaces, and seven neighborhood block parties.</p>
    </div>
  </section>

  <!-- 3. MANIFESTO (unchanged) -->
  <section class="cc-section cc-manifesto" data-pattern="manifesto" data-variant="default">
    <div class="container --regular flow motion-reveal">
      <h2 class="cc-manifesto__heading voice-editorial --display">San Francisco has always been where the future comes to get a body.</h2>
      <div class="cc-manifesto__body motion-stagger">
        <p class="voice-swiss">Before the future becomes normal, it comes here strange. Before it becomes infrastructure, it comes here as a scene. Before it becomes an industry, it comes here as music, code, protest, poetry, sex, food, risk, argument, light, fog, and people brave enough to live a little ahead of schedule.</p>
        <p class="voice-swiss">The Beats came here to rewrite consciousness. The psychedelic generation came here to turn music into a portal. Queer liberation changed the moral vocabulary of the world from these streets. Punk, rave, Burning Man, <em>Wired</em>, open source, biotech, mobile, social, climate, crypto, and AI all passed through this same charged geography.</p>
        <p class="voice-swiss">Now the next wave is here. Artificial intelligence is not just another technology cycle — it is the new nervous system of capital, culture, media, labor, health, education, cities, and power. And once again, <em class="io">San Francisco is where the wire is thickest.</em></p>
      </div>
    </div>
  </section>

  <!-- 4. STAT-GRID #1 — proof cards (unchanged) -->
  <section class="cc-section cc-stat-grid" data-pattern="stat-grid" data-variant="default">
    <div class="container --wide flow motion-reveal">
      <h2 class="cc-stat-grid__heading voice-monument --display">Culture is everywhere. Decision gravity is not.</h2>
      <div class="cc-stat-grid__items motion-stagger">
        <div class="cc-stat"><span class="cc-stat__num voice-monument">$211B</span><span class="cc-stat__label voice-swiss --label">global AI VC, 2025</span><span class="cc-stat__caption voice-swiss">85% surge year-over-year.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument">60%</span><span class="cc-stat__label voice-swiss --label">captured by the Bay Area</span><span class="cc-stat__caption voice-swiss">$126B of the global pool.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument">$113B</span><span class="cc-stat__label voice-swiss --label">to 92 companies</span><span class="cc-stat__caption voice-swiss">The narrow window the future passes through.</span></div>
        <div class="cc-stat"><span class="cc-stat__num voice-monument">$1.7B</span><span class="cc-stat__label voice-swiss --label">SF nonprofit arts output</span><span class="cc-stat__caption voice-swiss">36,828 full-time-equivalent jobs.</span></div>
      </div>
    </div>
  </section>

  <!-- 5. FEATURE-GRID (NEW) -->
  <section class="cc-section cc-feature-grid" data-pattern="feature-grid" data-variant="default" id="program">
    <div class="container --wide flow motion-reveal">
      <span class="cc-feature-grid__eyebrow voice-swiss --label">PROGRAM</span>
      <h2 class="cc-feature-grid__heading voice-monument --display">One festival. Three economic engines. Seven nights.</h2>
      <div class="cc-feature-grid__items motion-stagger">
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M12 3v4M12 17v4M3 12h4M17 12h4M5.6 5.6l2.8 2.8M15.6 15.6l2.8 2.8M5.6 18.4l2.8-2.8M15.6 8.4l2.8-2.8"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Capital Summit</h3>
          <p class="cc-feature__body voice-swiss">Curated Tue–Thu core for LPs, family offices, GPs, sovereign funds, foundations, and select founders. Application-based. White-glove.</p>
        </div>
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><path d="M12 2l10 5-10 5L2 7l10-5zM2 12l10 5 10-5M2 17l10 5 10-5"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Core Conference</h3>
          <p class="cc-feature__body voice-swiss">Seven days of programming for founders, operators, brands, creators, civic leaders, designers, artists, media, and technologists.</p>
        </div>
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><path d="M12 2c-4 0-7 3-7 7 0 5 7 13 7 13s7-8 7-13c0-4-3-7-7-7z"/><circle cx="12" cy="9" r="2.5"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Civic Festival</h3>
          <p class="cc-feature__body voice-swiss">Public block parties, museum nights, art, music, markets, food, and neighborhood activations. Free where it counts.</p>
        </div>
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="2"/><circle cx="4" cy="6" r="2"/><circle cx="20" cy="6" r="2"/><circle cx="4" cy="18" r="2"/><circle cx="20" cy="18" r="2"/><path d="M6 7l5 4M18 7l-5 4M6 17l5-4M18 17l-5-4"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Neighborhood Nights</h3>
          <p class="cc-feature__body voice-swiss">Each evening belongs to a different district. Co-produced with local merchants, artists, venues, cultural districts, nonprofits, chefs, musicians.</p>
        </div>
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7S2 12 2 12z"/><circle cx="12" cy="12" r="3"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Civic Return</h3>
          <p class="cc-feature__body voice-swiss">Annual report on hotel nights, vendor revenue, artist compensation, neighborhood grants, and small-business participation. The festival leaves the city stronger.</p>
        </div>
        <div class="cc-feature">
          <div class="cc-feature__icon" aria-hidden="true"><svg viewBox="0 0 24 24"><path d="M12 3l2 6 6 2-6 2-2 6-2-6-6-2 6-2 2-6z"/></svg></div>
          <h3 class="cc-feature__title voice-monument">Fog City Futurism</h3>
          <p class="cc-feature__body voice-swiss">A premium visual identity blending San Francisco's cultural past with its technological future. Editorial, civic, electric.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- 6. PRODUCT-SHOWCASE (NEW) — annotated "city as campus" map -->
  <section class="cc-section cc-showcase" data-pattern="product-showcase" data-variant="callouts">
    <div class="container --wide">
      <div class="cc-showcase__grid motion-reveal">
        <div class="cc-showcase__copy">
          <h2 class="cc-showcase__heading voice-monument --display">The city is the campus.</h2>
          <p class="cc-showcase__body voice-swiss">SF VIBE FEST is not a convention trapped inside a convention center. For one week, San Francisco becomes a living map of the future. The route is the story; the movement through the city is the mechanism.</p>
        </div>
        <div class="cc-showcase__frame">
          <div class="cc-showcase__chrome" aria-hidden="true"><span></span><span></span><span></span></div>
          <div class="cc-img-placeholder cc-showcase__image" data-slot="city-map"></div>
          <button class="cc-showcase__callout" style="left:40%;top:30%" aria-label="Moscone Center"><span class="cc-showcase__callout-num">1</span><span class="cc-showcase__callout-label">Moscone</span></button>
          <button class="cc-showcase__callout" style="left:62%;top:35%" aria-label="Embarcadero"><span class="cc-showcase__callout-num">2</span><span class="cc-showcase__callout-label">Embarcadero</span></button>
          <button class="cc-showcase__callout" style="left:70%;top:65%" aria-label="Dogpatch"><span class="cc-showcase__callout-num">3</span><span class="cc-showcase__callout-label">Dogpatch</span></button>
          <button class="cc-showcase__callout" style="left:45%;top:55%" aria-label="Mission"><span class="cc-showcase__callout-num">4</span><span class="cc-showcase__callout-label">Mission</span></button>
          <button class="cc-showcase__callout" style="left:30%;top:50%" aria-label="Castro"><span class="cc-showcase__callout-num">5</span><span class="cc-showcase__callout-label">Castro</span></button>
        </div>
      </div>
    </div>
  </section>

  <!-- 7. QUOTE (unchanged) -->
  <section class="cc-section cc-quote" data-pattern="quote" data-variant="gutter">
    <div class="container --regular motion-reveal">
      <blockquote class="cc-quote__body">
        <p class="cc-quote__text voice-editorial --display">If you aren't in the room, you are 500ms behind the future.</p>
        <cite class="cc-quote__attribution voice-swiss --label">— The Eye of the Needle thesis · SF VIBE FEST 2027</cite>
      </blockquote>
    </div>
  </section>

  <!-- 8. PROCESS (unchanged) - keep existing 7-day timeline -->
  [Keep the existing process timeline section from the current file unchanged.]

  <!-- 9. TESTIMONIAL (NEW) — host-committee placeholder -->
  <section class="cc-section cc-testimonial" data-pattern="testimonial" data-variant="featured">
    <div class="container --regular motion-reveal">
      <div class="cc-testimonial__items">
        <figure class="cc-quote-card">
          <blockquote class="cc-quote-card__quote">
            <p class="voice-editorial --display">The festival proves the city can host its own future without losing its soul — premium where the rooms demand it, public where the streets need it, local at every block party.</p>
          </blockquote>
          <figcaption class="cc-quote-card__author">
            <span class="cc-quote-card__avatar" aria-hidden="true">HC</span>
            <span class="cc-quote-card__meta">
              <span class="cc-quote-card__name voice-monument">Host Committee</span>
              <span class="cc-quote-card__role voice-swiss">SF Vibe Fest 2027 · Founding Member</span>
            </span>
          </figcaption>
        </figure>
      </div>
    </div>
  </section>

  <!-- 10. PRICING (NEW) — three application tiers, NO dollar amounts -->
  <section class="cc-section cc-pricing" data-pattern="pricing" data-variant="cards" id="sponsors">
    <div class="container --wide flow motion-reveal">
      <span class="cc-pricing__eyebrow voice-swiss --label">ACCESS</span>
      <h2 class="cc-pricing__heading voice-monument --display">By invitation. By application. By neighborhood.</h2>
      <div class="cc-pricing__tiers motion-stagger">
        <div class="cc-tier">
          <h3 class="cc-tier__name voice-monument">Public Festival</h3>
          <p class="cc-tier__price"><span class="cc-tier__amount voice-monument">Free</span><span class="cc-tier__period voice-brutalist">where it counts</span></p>
          <ul class="cc-tier__features">
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Seven neighborhood block parties</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Public art + museum nights</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Live music + markets</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Civic forums + city programming</li>
          </ul>
          <a class="cc-tier__cta voice-swiss --label" href="mailto:jordan@shiny.new">Stay in touch</a>
        </div>
        <div class="cc-tier cc-tier--recommended">
          <span class="cc-tier__badge voice-swiss --label">Most requested</span>
          <h3 class="cc-tier__name voice-monument">Core Conference</h3>
          <p class="cc-tier__price"><span class="cc-tier__amount voice-monument">Apply</span><span class="cc-tier__period voice-brutalist">7-day pass</span></p>
          <ul class="cc-tier__features">
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Full mainstage + breakout access</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Expo + founder demos</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Sponsor houses + cultural salons</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Museum-night patron access</li>
          </ul>
          <a class="cc-tier__cta voice-swiss --label" href="mailto:jordan@shiny.new">Apply</a>
        </div>
        <div class="cc-tier">
          <h3 class="cc-tier__name voice-monument">Capital Summit</h3>
          <p class="cc-tier__price"><span class="cc-tier__amount voice-monument">By invitation</span><span class="cc-tier__period voice-brutalist">Tue–Thu</span></p>
          <ul class="cc-tier__features">
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>LP / family-office / GP roundtables</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Allocator breakfasts + founder matching</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>SHACK15 + Embarcadero private salons</li>
            <li class="voice-swiss"><svg class="cc-tier__check" viewBox="0 0 24 24"><path d="M5 12l4 4 10-10"/></svg>Everything in Core Conference</li>
          </ul>
          <a class="cc-tier__cta voice-swiss --label" href="mailto:jordan@shiny.new">Request invitation</a>
        </div>
      </div>
    </div>
  </section>

  <!-- 11. INTEGRATION-LOGOS (NEW) — venues as labels -->
  <section class="cc-section cc-integrations" data-pattern="integration-logos" data-variant="grid">
    <div class="container --wide flow motion-reveal">
      <h2 class="cc-integrations__heading voice-swiss --label">VENUE ECOSYSTEM</h2>
      <ul class="cc-integrations__items">
        <li class="cc-integrations__item"><span class="voice-monument">Moscone</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">SFMOMA</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">SHACK15</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">Exploratorium</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">Ferry Building</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">Power Station</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">Fort Mason</span></li>
        <li class="cc-integrations__item"><span class="voice-monument">Frontier Tower</span></li>
      </ul>
    </div>
  </section>

  <!-- 12. FULL-BLEED (unchanged) -->
  <section class="cc-section cc-bleed" data-pattern="full-bleed" data-variant="type">
    <div class="cc-bleed__content cc-bleed__content--type motion-reveal">
      <span class="voice-monument" data-bleed-text>The future does not need another convention. It needs a place to become human.</span>
    </div>
  </section>

  <!-- 13. FAQ (NEW) -->
  <section class="cc-section cc-faq" data-pattern="faq" data-variant="accordion" id="civic">
    <div class="container --regular flow motion-reveal">
      <h2 class="cc-faq__heading voice-monument --display">Questions, briefly.</h2>
      <div class="cc-faq__items">
        <details class="cc-faq__item">
          <summary class="cc-faq__question voice-monument">How do I get a pass?<span class="cc-faq__marker" aria-hidden="true">+</span></summary>
          <div class="cc-faq__answer"><p class="voice-swiss">Core Conference and Capital Summit are application-based. Email jordan@shiny.new with your name, role, and what you'd contribute. The public Civic Festival is free and open — no pass required.</p></div>
        </details>
        <details class="cc-faq__item">
          <summary class="cc-faq__question voice-monument">Is the festival public or private?<span class="cc-faq__marker" aria-hidden="true">+</span></summary>
          <div class="cc-faq__answer"><p class="voice-swiss">Both. The public sees the festival — block parties, museum nights, music, food, public art. The business is the room — the Capital Summit and curated tracks nested inside. Premium access funds public wonder.</p></div>
        </details>
        <details class="cc-faq__item">
          <summary class="cc-faq__question voice-monument">What's the civic return?<span class="cc-faq__marker" aria-hidden="true">+</span></summary>
          <div class="cc-faq__answer"><p class="voice-swiss">Every year we publish a Civic Return Report: hotel nights, local vendor revenue, artist compensation, small-business participation, neighborhood grants, public attendance, city tax impact, and more. The festival must leave the city stronger than it found it.</p></div>
        </details>
        <details class="cc-faq__item">
          <summary class="cc-faq__question voice-monument">When do block parties happen?<span class="cc-faq__marker" aria-hidden="true">+</span></summary>
          <div class="cc-faq__answer"><p class="voice-swiss">Every night of the festival. Each evening belongs to a different neighborhood (SoMa Mon, Embarcadero Tue, Mission Wed, Dogpatch Thu, Fillmore Fri, Castro Sat, Bayview Sun). All free, all public, all co-produced with local merchants, artists, and venues.</p></div>
        </details>
      </div>
    </div>
  </section>

  <!-- FAQ accordion JS -->
  <script>
    (function(){document.querySelectorAll('.cc-faq[data-variant="accordion"]').forEach(function(f){f.querySelectorAll('.cc-faq__item').forEach(function(i){i.addEventListener('toggle',function(){if(!i.open)return;f.querySelectorAll('.cc-faq__item').forEach(function(s){if(s!==i)s.open=false;});});});});})();
  </script>

  <!-- 14. CTA-BANNER (NEW) — sticky bottom -->
  <section class="cc-section cc-banner" data-pattern="cta-banner" data-variant="sticky-bottom" data-key="sf-vibe-fest-apply">
    <div class="container --wide cc-banner__inner">
      <div class="cc-banner__copy">
        <h2 class="cc-banner__heading voice-monument">Once a year, the room opens.</h2>
        <p class="cc-banner__supporting voice-swiss">Applications, founding partnerships, and host-committee invitations are open.</p>
      </div>
      <a class="cc-banner__action voice-swiss --label" href="mailto:jordan@shiny.new">jordan@shiny.new<span aria-hidden="true">→</span></a>
      <button class="cc-banner__dismiss" type="button" aria-label="Dismiss" hidden><span aria-hidden="true">×</span></button>
    </div>
  </section>

  <!-- CTA-BANNER dismiss JS -->
  <script>
    (function(){document.querySelectorAll('.cc-banner[data-variant="sticky-bottom"]').forEach(function(b){var k='cc-banner-dismissed-'+(b.dataset.key||'default');if(localStorage.getItem(k)==='1'){b.remove();return;}var d=b.querySelector('.cc-banner__dismiss');if(d){d.hidden=false;d.addEventListener('click',function(){localStorage.setItem(k,'1');b.setAttribute('data-dismissing','');setTimeout(function(){b.remove();},300);});}});})();
  </script>

  <!-- 15. FOOTER (unchanged) -->
  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="editorial">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">SF VIBE FEST 2027 · The Future Gets a Neighborhood · Built by SHINY and Omega MegaCorp</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="#about">about</a>
        <a class="cc-footer__link voice-swiss --label" href="#sponsors">sponsors</a>
        <a class="cc-footer__link voice-swiss --label" href="#civic">civic return</a>
        <a class="cc-footer__link voice-swiss --label" href="mailto:jordan@shiny.new">contact</a>
      </nav>
    </div>
  </footer>

  <!-- IntersectionObserver for motion-reveal / motion-stagger (unchanged from v1) -->
  <script>
    (function(){var io=new IntersectionObserver(function(es){es.forEach(function(e){if(e.isIntersecting){e.target.classList.add('--visible');io.unobserve(e.target);}});},{threshold:0.1,rootMargin:'0px 0px -10% 0px'});document.querySelectorAll('.motion-reveal,.motion-stagger').forEach(function(el){io.observe(el);});})();
  </script>

</body>
```

- [ ] **Step 4: Verify the page loads and all section count is right**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
# Start the static server if not already running
pgrep -f "http.server 8770" >/dev/null || (python3 -m http.server 8770 > /tmp/cc-srv.log 2>&1 &)
sleep 1
# Count unique data-pattern values
curl -sS http://localhost:8770/showcase/test-sf-vibe-fest.html | grep -oE 'data-pattern="[^"]+"' | sort -u
# Count mustache leaks
echo "Mustache leaks: $(curl -sS http://localhost:8770/showcase/test-sf-vibe-fest.html | grep -cE '\{\{[^}]+\}\}')"
```

Expected: 14 unique data-pattern values (15 sections but two stat-grids share the same pattern), 0 mustache leaks.

- [ ] **Step 5: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add showcase/test-sf-vibe-fest.html
git commit -m "Re-render SF Vibe Fest to 15 sections using v2 library

Adds nav (sticky + drawer), feature-grid (6 program pillars),
product-showcase (city-as-campus callout map), testimonial
(Host Committee placeholder), pricing (3 application tiers, no
\$ amounts), integration-logos (venue ecosystem), faq (4 common
questions), cta-banner (sticky-bottom, dismissible). Skips
comparison-table by design — the festival has no direct
competitor.

Honors v2 style guide constraints: no fake bios, no real
pricing, sentence case headings.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 21: Documentation updates

**Files:**
- Modify: `SKILL.md`
- Modify: `README.md`
- Modify: `CLAUDE.md`

- [ ] **Step 1: Update `SKILL.md` triggers + add SMB-baseline note**

In `SKILL.md`, find the `triggers:` line and append (comma-separated):

```
add a nav, add a feature grid, add a feature-grid, add a product showcase, add a product-showcase, add pricing, add an faq, add a faq, add a testimonial, add a signup form, add a signup-form, add integration logos, add an integration-logos, add a cta banner, add a cta-banner, add a comparison table, add a comparison-table, SMB tech landing page, SaaS landing page, out of the box landing page
```

Then find the `## Two Workflows` section and add this note immediately after the existing table:

```markdown
**SMB-tech baseline:** The default polish baseline is *Editorial-Generous* (Stripe / Anthropic / Cursor-marketing DNA). The interaction-states layer in `tokens/states.css` makes every interactive element feel shipping-grade out of the box. Section variants are tuned for this baseline.
```

- [ ] **Step 2: Update `README.md` — extend section catalog table to 22 + add polish + scaffolding tables**

In `README.md`, find the existing "Section Library" table (12 rows) and replace its body with the 22-row version from `sections/README.md`. Then add three new sections immediately after:

```markdown
## Polish Layer (`tokens/states.css`)

Every interactive element gets shipping-grade craft out of the box.

| Behavior | Element types | How |
|---|---|---|
| Hover lift | buttons, CTAs, nav-CTA | `transform: translateY(-2px)` at `var(--dur-swift)` |
| Active press | buttons, CTAs | `transform: translateY(1px)` at `var(--dur-instant)` |
| Focus ring | every focusable | `outline: 2px solid var(--color-discord)` via `:focus-visible` |
| Disabled | buttons, inputs | `opacity: 0.4` + `cursor: not-allowed` + `pointer-events: none` |
| Loading | buttons with `[aria-busy]` or `[data-loading]` | spinner pseudo-element |
| Touch targets | buttons, links, inputs, nav links | `min-height: 44px` |
| Input states | text/email/textarea/select | hover/focus/invalid/disabled |
| Reduced motion | universal | `prefers-reduced-motion: reduce` zeros transforms + spinner |

## Premium Motion

Added to `sections.css`:

- **Scroll-progress hairline** at top of viewport via `animation-timeline: scroll()` (Chrome/Edge; graceful absence on Firefox stable).
- **Smooth-scroll with sticky-nav offset** via `scroll-padding-top: calc(var(--nav-height, 0) + 1rem)`.
- **Image-placeholder upgrade** — `cc-img-placeholder` now uses radial-gradient + linear-gradient + SVG grain overlay + bottom-left attr() label.

All halt under `prefers-reduced-motion`.

## Template Scaffolding

Deploy-ready defaults in `templates/`:

| File | Purpose |
|---|---|
| `page-composed.html` | Full SEO/OG/Twitter/JSON-LD `<head>` with Mustache placeholders for `{{seo.*}}` and `{{org.*}}` |
| `404.html` | Style-driven 404 — hero + cta + footer, same `<link>` chain as the main page |
| `privacy.html` | 8-section privacy-policy scaffold (lorem-replaceable) |
| `terms.html` | 12-section terms-of-service scaffold |
| `meta.partial.html` | Standalone copy-pasteable `<head>` meta stack for non-CC pages |
```

- [ ] **Step 3: Update `CLAUDE.md` — add one bullet**

In `CLAUDE.md`, under the "How to Use This System" numbered list, append:

```markdown
8. **SMB-tech baseline is opt-in via class adoption.** The polish layer in `tokens/states.css` activates universally when included in the `<link>` chain. Section partials use the *Editorial-Generous* variant defaults; choose other variants per brand context.
```

- [ ] **Step 4: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add SKILL.md README.md CLAUDE.md
git commit -m "Update docs — triggers, section catalog (22), polish/scaffolding tables

SKILL.md: triggers extended with all 10 new section names; Two
Workflows section adds SMB-tech baseline note.
README.md: section table grows from 12 to 22 entries; Polish Layer
+ Premium Motion + Template Scaffolding tables added.
CLAUDE.md: one bullet (#8) noting tokens/states.css is opt-in via
class adoption.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 22: Backwards-compatibility verification

No new files. Verification gate before push.

- [ ] **Step 1: Open the existing showcase pages + Lovetel + SF Vibe Fest in a browser**

```bash
# Ensure static server is running
pgrep -f "http.server 8770" >/dev/null || (cd /Users/nayslayer/.claude/skills/controlledchaos && python3 -m http.server 8770 > /tmp/cc-srv.log 2>&1 &)
sleep 1
for p in showcase/swiss.html showcase/brutalist.html showcase/immersive.html showcase/kinetic.html showcase/gallery.html showcase/test-lovetel.html showcase/test-sf-vibe-fest.html showcase/sections-demo.html showcase/style-driven.html; do
  curl -sS -o /dev/null -w "  $p: %{http_code}\n" "http://localhost:8770/$p"
done
```

Expected: every page returns 200.

- [ ] **Step 2: Check Lovetel for unintended visual changes**

Open `http://localhost:8770/showcase/test-lovetel.html` in a browser. Confirm:
- Background is dusk warm-dark (#0e0a06)
- Hero italic Cormorant Garamond
- Quote section has the cyan gutter
- No new interaction-state weirdness on buttons (they should still hover-lift but match Lovetel's calm tempo)
- No horizontal overflow

If any visual changed unexpectedly, the polish layer may have over-reached. Diagnose by checking computed styles in DevTools.

- [ ] **Step 3: Verify SF Vibe Fest at desktop + narrow viewports**

Open `http://localhost:8770/showcase/test-sf-vibe-fest.html`. At desktop:
- Sticky nav appears with backdrop blur on scroll
- All 15 sections render
- Scroll-progress hairline visible at top of viewport
- Sticky-bottom CTA banner rises after 500ms
- Dismissing it removes it and stores localStorage flag

At narrow viewport (Chrome DevTools Device Mode → iPhone 14 Pro):
- Nav collapses to hamburger; tapping opens drawer
- All sections stack to 1-column
- Pricing cards stack vertically
- Stat numbers don't overflow
- No horizontal scroll

- [ ] **Step 4: Verify schema still validates for both adapter examples**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
python3 -c "
import json
schema = json.load(open('adapters/style-schema.json'))
for f in ['adapters/examples/minimal.json','adapters/examples/dusk.json','adapters/examples/signal.json','adapters/examples/lovetel.json','adapters/examples/sf-vibe-fest.json']:
    data = json.load(open(f))
    # Basic check: required fields present
    assert 'name' in data, f + ': missing name'
    assert 'palette' in data or 'mood_id' in data, f + ': missing palette or mood_id'
    print(f, 'OK')
"
```

Expected: every example logs OK.

- [ ] **Step 5: No commit needed unless something was fixed**

If a regression was discovered and patched, commit it with a `Fix <specific issue>` message. Otherwise this task is a no-op — skip the commit.

---

## Task 23: Push to GitHub

- [ ] **Step 1: Confirm clean working tree + commit log**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git status -s
git log --oneline -25
```

Expected: clean tree, ~22 commits ahead of last push (Tasks 1-22 each committed at least once).

- [ ] **Step 2: Ask the user for explicit push authorization**

Per global memory: do NOT push without explicit user authorization.

If the user already said "push it" or "keep github updated" in the current session, proceed. Otherwise stop and ask.

- [ ] **Step 3: Push to all configured remotes**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git pushall 2>&1 | tail -10
```

Expected: push to `origin` (https://github.com/nayslayer143/controlledchaos.git) succeeds. `gitlab` and `gitea` remotes may not exist for this skill repo — the `|| true` in the alias swallows those errors gracefully.

- [ ] **Step 4: Confirm on GitHub**

```bash
gh repo view nayslayer143/controlledchaos --json updatedAt,defaultBranchRef
```

Expected: `defaultBranchRef.name === "main"`, `updatedAt` is within the last minute.

---

## Plan complete

All 23 tasks shipped:

1. ✅ `tokens/states.css` — interaction states
2. ✅ `sections.css` polish — image-placeholder + premium motion
3. ✅ `page-composed.html` head — SEO/OG/JSON-LD + states.css in link chain
4. ✅ `meta.partial.html`
5. ✅ `404.html`
6. ✅ `privacy.html`
7. ✅ `terms.html`
8. ✅ `nav` section
9. ✅ `feature-grid` section
10. ✅ `product-showcase` section
11. ✅ `pricing` section
12. ✅ `faq` section
13. ✅ `testimonial` section
14. ✅ `signup-form` section
15. ✅ `integration-logos` section
16. ✅ `cta-banner` section
17. ✅ `comparison-table` section
18. ✅ `icons.md` reference SVGs
19. ✅ `sections/README.md` catalog (22 entries)
20. ✅ SF Vibe Fest 15-section re-render
21. ✅ Docs updates (SKILL/README/CLAUDE)
22. ✅ Backwards-compat verification
23. ✅ Push to GitHub

The skill now produces SMB-tech-grade landing pages out of the box for any brand whose vibe lives in a `style.json`. SF Vibe Fest is the canary; Lovetel remains the dusk-quiet anchor.
