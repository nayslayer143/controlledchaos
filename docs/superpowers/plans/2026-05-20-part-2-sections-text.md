# Part 2: Text-Heavy Sections (Tasks 4–8)

Prev: [part-1-foundation.md](2026-05-20-part-1-foundation.md). Next: [part-3-sections-data.md](2026-05-20-part-3-sections-data.md).

Each task in this part follows the same shape: write the partial HTML, append the section's CSS to `sections/sections.css`, commit. No new dependencies are introduced — every section composes from existing CC tokens + the base in `sections/sections.css`.

**General authoring contract for every section (applies to all 12):**
- Top-comment lists best / ok / avoid modes + variants
- Root element carries `class="cc-section cc-<name>"`, `data-pattern="<name>"`, `data-variant="<default-variant>"`
- Placeholders use Mustache: `{{scalar}}` for substitution, `{{#items}}...{{/items}}` for repeats
- The default variant is the first one named in the variants comment
- Image slots use `<div class="cc-img-placeholder" data-slot="<name>"></div>` — NEVER fetch from a style-guide source board (see project memory)

---

## Task 4: `hero` section

**Files:**
- Create: `sections/hero.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/hero.html`**

```html
<!-- cc-section: hero
     best:  all
     ok:    -
     avoid: -
     variants: monument, editorial, bleed
     placeholders: heading, subhead, (optional) eyebrow -->
<section class="cc-section cc-hero"
         data-pattern="hero"
         data-variant="monument">
  <div class="container --regular flow">
    <span class="cc-hero__eyebrow voice-swiss --label">{{eyebrow}}</span>
    <h1 class="cc-hero__heading voice-monument --massive">{{heading}}</h1>
    <p class="cc-hero__subhead voice-swiss">{{subhead}}</p>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- hero --------------------------------------------------- */
.cc-hero {
  padding-block: clamp(6rem, 18vh, 14rem);
}
.cc-hero__eyebrow {
  display: block;
  margin-bottom: 1.5rem;
  color: var(--color-discord);
}
.cc-hero__heading {
  margin: 0 0 1.5rem;
  line-height: 0.95;
  letter-spacing: -0.02em;
}
.cc-hero__subhead {
  max-width: 50ch;
  color: var(--color-accent-2);
}
.cc-hero[data-variant="editorial"] .cc-hero__heading {
  font-family: var(--font-editorial);
  letter-spacing: -0.01em;
}
.cc-hero[data-variant="editorial"] .cc-hero__heading::after {
  content: ".";
  color: var(--color-discord);
}
.cc-hero[data-variant="bleed"] .container { max-width: none; padding-inline: 4vw; }
.cc-hero[data-variant="bleed"] .cc-hero__heading { font-size: clamp(4rem, 14vw, 14rem); }
```

- [ ] **Step 3: Commit**

```bash
git add sections/hero.html sections/sections.css
git commit -m "Add hero section partial (monument/editorial/bleed variants)"
```

---

## Task 5: `manifesto` section

**Files:**
- Create: `sections/manifesto.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/manifesto.html`**

```html
<!-- cc-section: manifesto
     best:  swiss, brutalist, immersive
     ok:    kinetic
     avoid: gallery (too loud for the white cube)
     variants: default, numbered
     placeholders: heading, paragraphs[] -->
<section class="cc-section cc-manifesto"
         data-pattern="manifesto"
         data-variant="default">
  <div class="container --regular flow">
    <h2 class="cc-manifesto__heading voice-editorial --display">{{heading}}</h2>
    <div class="cc-manifesto__body">
      {{#paragraphs}}
      <p class="voice-swiss">{{text}}</p>
      {{/paragraphs}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- manifesto ---------------------------------------------- */
.cc-manifesto__heading {
  max-width: 22ch;
  margin: 0 0 3rem;
}
.cc-manifesto__body {
  display: grid;
  gap: 1.5rem;
  max-width: 60ch;
  font-size: var(--type-body);
}
.cc-manifesto[data-variant="numbered"] .cc-manifesto__body {
  counter-reset: cc-manifesto-p;
}
.cc-manifesto[data-variant="numbered"] .cc-manifesto__body > p {
  counter-increment: cc-manifesto-p;
  padding-left: 4rem;
  position: relative;
}
.cc-manifesto[data-variant="numbered"] .cc-manifesto__body > p::before {
  content: counter(cc-manifesto-p, decimal-leading-zero);
  position: absolute; left: 0; top: 0;
  font-family: var(--font-brutalist);
  font-size: var(--type-caption);
  color: var(--color-discord);
  letter-spacing: 0.1em;
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/manifesto.html sections/sections.css
git commit -m "Add manifesto section partial (default/numbered variants)"
```

---

## Task 6: `quote` section

**Files:**
- Create: `sections/quote.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/quote.html`**

```html
<!-- cc-section: quote
     best:  all
     ok:    -
     avoid: -
     variants: default, monument, gutter
     placeholders: quote, attribution, (optional) source -->
<section class="cc-section cc-quote"
         data-pattern="quote"
         data-variant="default">
  <div class="container --regular">
    <blockquote class="cc-quote__body">
      <p class="cc-quote__text voice-editorial --display">{{quote}}</p>
      <cite class="cc-quote__attribution voice-swiss --label">
        — {{attribution}}<span class="cc-quote__source">{{source}}</span>
      </cite>
    </blockquote>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- quote -------------------------------------------------- */
.cc-quote__body { margin: 0; }
.cc-quote__text {
  margin: 0 0 2rem;
  max-width: 28ch;
  line-height: 1.1;
  font-style: italic;
  position: relative;
}
.cc-quote__text::before {
  content: "\201C";
  position: absolute; left: -0.5em; top: -0.1em;
  color: var(--color-discord);
  font-size: 1.4em;
  font-style: normal;
}
.cc-quote__attribution { color: var(--color-muted); display: block; }
.cc-quote__source { display: block; margin-top: 0.25em; color: var(--color-accent-2); font-style: italic; }
.cc-quote[data-variant="monument"] .cc-quote__text {
  font-family: var(--font-monument);
  font-style: normal;
  text-transform: uppercase;
  font-size: var(--type-massive);
  letter-spacing: -0.02em;
}
.cc-quote[data-variant="gutter"] .cc-quote__text {
  padding-left: 4rem;
  border-left: 4px solid var(--color-discord);
}
.cc-quote[data-variant="gutter"] .cc-quote__text::before { display: none; }
```

- [ ] **Step 3: Commit**

```bash
git add sections/quote.html sections/sections.css
git commit -m "Add quote section partial (default/monument/gutter variants)"
```

---

## Task 7: `two-column` section

**Files:**
- Create: `sections/two-column.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/two-column.html`**

```html
<!-- cc-section: two-column
     best:  swiss, gallery
     ok:    immersive, editorial moods
     avoid: kinetic (steals motion budget)
     variants: editorial, dialogue, before-after
     placeholders: left{heading,body}, right{heading,body} -->
<section class="cc-section cc-two-col"
         data-pattern="two-column"
         data-variant="editorial">
  <div class="container --wide">
    <div class="cc-two-col__grid">
      <div class="cc-two-col__col">
        <h3 class="cc-two-col__heading voice-monument">{{left.heading}}</h3>
        <p class="cc-two-col__body voice-swiss">{{left.body}}</p>
      </div>
      <div class="cc-two-col__col">
        <h3 class="cc-two-col__heading voice-monument">{{right.heading}}</h3>
        <p class="cc-two-col__body voice-swiss">{{right.body}}</p>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- two-column --------------------------------------------- */
.cc-two-col__grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: clamp(2rem, 5vw, 6rem);
}
@media (max-width: 720px) { .cc-two-col__grid { grid-template-columns: 1fr; } }
.cc-two-col__heading { margin: 0 0 1rem; font-size: var(--type-headline); }
.cc-two-col__body { margin: 0; max-width: 50ch; font-size: var(--type-body); }
.cc-two-col[data-variant="dialogue"] .cc-two-col__col:nth-child(2) { padding-top: 6rem; }
.cc-two-col[data-variant="before-after"] .cc-two-col__col:nth-child(1)::before {
  content: "Before";
  display: block; margin-bottom: 0.5rem;
  font-family: var(--font-brutalist); font-size: var(--type-caption);
  text-transform: uppercase; letter-spacing: 0.15em;
  color: var(--color-muted);
}
.cc-two-col[data-variant="before-after"] .cc-two-col__col:nth-child(2)::before {
  content: "After";
  display: block; margin-bottom: 0.5rem;
  font-family: var(--font-brutalist); font-size: var(--type-caption);
  text-transform: uppercase; letter-spacing: 0.15em;
  color: var(--color-discord);
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/two-column.html sections/sections.css
git commit -m "Add two-column section partial (editorial/dialogue/before-after variants)"
```

---

## Task 8: `cta` section

**Files:**
- Create: `sections/cta.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/cta.html`**

```html
<!-- cc-section: cta
     best:  all
     ok:    -
     avoid: -
     variants: default, discord, monument
     placeholders: heading, action_text, action_href, (optional) supporting -->
<section class="cc-section cc-cta"
         data-pattern="cta"
         data-variant="default">
  <div class="container --regular flow">
    <h2 class="cc-cta__heading voice-monument --display">{{heading}}</h2>
    <p class="cc-cta__supporting voice-swiss">{{supporting}}</p>
    <a class="cc-cta__action voice-swiss --label" href="{{action_href}}">
      {{action_text}}
      <span class="cc-cta__arrow" aria-hidden="true">→</span>
    </a>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- cta ---------------------------------------------------- */
.cc-cta { text-align: left; }
.cc-cta__heading { margin: 0 0 1rem; max-width: 22ch; }
.cc-cta__supporting { max-width: 50ch; color: var(--color-accent-2); margin-bottom: 2rem; }
.cc-cta__action {
  display: inline-flex; align-items: center; gap: 0.75rem;
  padding: 1rem 1.5rem;
  border: 2px solid var(--color-fg);
  color: var(--color-fg); text-decoration: none;
  text-transform: uppercase; letter-spacing: 0.1em;
  transition: transform var(--dur-swift) var(--ease-spring), background var(--dur-swift);
}
.cc-cta__action:hover { transform: translateY(-2px); background: var(--color-fg); color: var(--color-bg); }
.cc-cta__arrow { transition: transform var(--dur-swift) var(--ease-spring); }
.cc-cta__action:hover .cc-cta__arrow { transform: translateX(4px); }
.cc-cta[data-variant="discord"] .cc-cta__action {
  border-color: var(--color-discord); color: var(--color-discord);
}
.cc-cta[data-variant="discord"] .cc-cta__action:hover { background: var(--color-discord); color: var(--color-bg); }
.cc-cta[data-variant="monument"] .cc-cta__heading {
  font-size: var(--type-massive); line-height: 0.9;
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/cta.html sections/sections.css
git commit -m "Add cta section partial (default/discord/monument variants)"
```

---

End of Part 2. Continue to [part-3-sections-data.md](2026-05-20-part-3-sections-data.md).
