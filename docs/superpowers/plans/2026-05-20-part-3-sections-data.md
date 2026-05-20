# Part 3: Data/Structural Sections (Tasks 9–12)

Prev: [part-2-sections-text.md](2026-05-20-part-2-sections-text.md). Next: [part-4-sections-visual.md](2026-05-20-part-4-sections-visual.md).

Authoring contract recap: top-comment for best/ok/avoid + variants; `class="cc-section cc-<name>"`, `data-pattern`, `data-variant`; Mustache placeholders; append CSS to `sections/sections.css`; commit per section.

---

## Task 9: `stat-grid` section

**Files:**
- Create: `sections/stat-grid.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/stat-grid.html`**

```html
<!-- cc-section: stat-grid
     best:  swiss, brutalist, kinetic
     ok:    immersive
     avoid: gallery (numbers fight white-cube quiet)
     variants: default, bleed, vertical
     placeholders: heading, stats[] (num, label, optional caption) -->
<section class="cc-section cc-stat-grid"
         data-pattern="stat-grid"
         data-variant="default">
  <div class="container --wide flow">
    <h2 class="cc-stat-grid__heading voice-monument --display">{{heading}}</h2>
    <div class="cc-stat-grid__items">
      {{#stats}}
      <div class="cc-stat">
        <span class="cc-stat__num voice-monument --massive">{{num}}</span>
        <span class="cc-stat__label voice-swiss --label">{{label}}</span>
        <span class="cc-stat__caption voice-swiss">{{caption}}</span>
      </div>
      {{/stats}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- stat-grid ---------------------------------------------- */
.cc-stat-grid__heading { margin: 0 0 3rem; max-width: 28ch; }
.cc-stat-grid__items {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: clamp(2rem, 4vw, 4rem);
}
.cc-stat { display: flex; flex-direction: column; gap: 0.5rem; }
.cc-stat__num {
  line-height: 0.9;
  letter-spacing: -0.04em;
  color: var(--color-fg);
}
.cc-stat__label {
  color: var(--color-discord);
  text-transform: uppercase;
  letter-spacing: 0.15em;
}
.cc-stat__caption { color: var(--color-muted); font-size: var(--type-body-small); max-width: 30ch; }
.cc-stat-grid[data-variant="bleed"] .container { max-width: none; padding-inline: 0; }
.cc-stat-grid[data-variant="bleed"] .cc-stat-grid__items { padding-inline: 4vw; border-top: 1px solid var(--color-border); padding-top: 2rem; }
.cc-stat-grid[data-variant="vertical"] .cc-stat-grid__items {
  grid-template-columns: 1fr;
  border-top: 1px solid var(--color-border);
}
.cc-stat-grid[data-variant="vertical"] .cc-stat {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 2rem;
  padding: 2rem 0;
  border-bottom: 1px solid var(--color-border);
  align-items: baseline;
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/stat-grid.html sections/sections.css
git commit -m "Add stat-grid section partial (default/bleed/vertical variants)"
```

---

## Task 10: `process` section

**Files:**
- Create: `sections/process.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/process.html`**

```html
<!-- cc-section: process
     best:  swiss, immersive
     ok:    brutalist, kinetic
     avoid: gallery (sequence feels businesslike for white cube)
     variants: numbered, timeline, vertical
     placeholders: heading, steps[] (title, body) -->
<section class="cc-section cc-process"
         data-pattern="process"
         data-variant="numbered">
  <div class="container --regular flow">
    <h2 class="cc-process__heading voice-editorial --display">{{heading}}</h2>
    <ol class="cc-process__steps">
      {{#steps}}
      <li class="cc-process__step">
        <h3 class="cc-process__step-title voice-monument">{{title}}</h3>
        <p class="cc-process__step-body voice-swiss">{{body}}</p>
      </li>
      {{/steps}}
    </ol>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- process ------------------------------------------------ */
.cc-process__heading { margin: 0 0 3rem; max-width: 28ch; }
.cc-process__steps { list-style: none; padding: 0; margin: 0; counter-reset: cc-process; display: grid; gap: 3rem; }
.cc-process__step {
  counter-increment: cc-process;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 2rem;
  align-items: start;
}
.cc-process__step::before {
  content: counter(cc-process, decimal-leading-zero);
  font-family: var(--font-monument);
  font-size: var(--type-display);
  line-height: 1;
  color: var(--color-discord);
  letter-spacing: -0.02em;
}
.cc-process__step-title { margin: 0 0 0.5rem; font-size: var(--type-headline); }
.cc-process__step-body { margin: 0; max-width: 50ch; color: var(--color-accent-2); }
.cc-process[data-variant="timeline"] .cc-process__steps {
  position: relative; padding-left: 3rem;
}
.cc-process[data-variant="timeline"] .cc-process__steps::before {
  content: ""; position: absolute; left: 1rem; top: 0; bottom: 0;
  border-left: 2px solid var(--color-border);
}
.cc-process[data-variant="timeline"] .cc-process__step { position: relative; }
.cc-process[data-variant="timeline"] .cc-process__step::before {
  position: absolute; left: -3rem; font-size: var(--type-headline);
}
.cc-process[data-variant="vertical"] .cc-process__step {
  grid-template-columns: 1fr;
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 2rem;
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/process.html sections/sections.css
git commit -m "Add process section partial (numbered/timeline/vertical variants)"
```

---

## Task 11: `footer` section

**Files:**
- Create: `sections/footer.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/footer.html`**

```html
<!-- cc-section: footer
     best:  all
     ok:    -
     avoid: -
     variants: minimal, editorial, bleed
     placeholders: copyright, links[] (label, href) -->
<footer class="cc-section cc-footer"
        data-pattern="footer"
        data-variant="minimal">
  <div class="container flex-between">
    <span class="cc-footer__copyright voice-swiss --label">{{copyright}}</span>
    <nav class="cc-footer__nav">
      {{#links}}
      <a class="cc-footer__link voice-swiss --label" href="{{href}}">{{label}}</a>
      {{/links}}
    </nav>
  </div>
</footer>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- footer ------------------------------------------------- */
.cc-footer { padding-block: clamp(2rem, 4vh, 4rem); border-top: 1px solid var(--color-border); }
.cc-footer .container { display: flex; align-items: center; justify-content: space-between; gap: 2rem; flex-wrap: wrap; }
.cc-footer__copyright { color: var(--color-muted); text-transform: uppercase; letter-spacing: 0.1em; }
.cc-footer__nav { display: flex; gap: 2rem; flex-wrap: wrap; }
.cc-footer__link {
  color: var(--color-fg); text-decoration: none;
  text-transform: uppercase; letter-spacing: 0.1em;
  transition: color var(--dur-swift);
}
.cc-footer__link:hover { color: var(--color-discord); }
.cc-footer[data-variant="editorial"] .container { flex-direction: column; align-items: flex-start; }
.cc-footer[data-variant="editorial"] .cc-footer__copyright {
  font-family: var(--font-editorial);
  font-size: var(--type-body);
  text-transform: none;
  letter-spacing: normal;
  color: var(--color-fg);
}
.cc-footer[data-variant="bleed"] .container { max-width: none; padding-inline: 4vw; }
```

- [ ] **Step 3: Commit**

```bash
git add sections/footer.html sections/sections.css
git commit -m "Add footer section partial (minimal/editorial/bleed variants)"
```

---

## Task 12: `logo-wall` section

**Files:**
- Create: `sections/logo-wall.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/logo-wall.html`**

```html
<!-- cc-section: logo-wall
     best:  swiss, brutalist
     ok:    gallery
     avoid: kinetic (motion fights monumentality)
     variants: grid, marquee, monolith
     placeholders: heading, logos[] (label, optional href) -->
<section class="cc-section cc-logo-wall"
         data-pattern="logo-wall"
         data-variant="grid">
  <div class="container --wide flow">
    <h2 class="cc-logo-wall__heading voice-swiss --label">{{heading}}</h2>
    <ul class="cc-logo-wall__items">
      {{#logos}}
      <li class="cc-logo-wall__item">
        <a href="{{href}}" class="voice-monument">{{label}}</a>
      </li>
      {{/logos}}
    </ul>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- logo-wall ---------------------------------------------- */
.cc-logo-wall__heading {
  text-transform: uppercase; letter-spacing: 0.2em;
  color: var(--color-muted); margin: 0 0 2rem;
  text-align: center;
}
.cc-logo-wall__items {
  list-style: none; padding: 0; margin: 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 2rem 3rem;
  align-items: center;
  justify-items: center;
}
.cc-logo-wall__item a {
  font-family: var(--font-monument);
  font-size: var(--type-headline);
  text-transform: uppercase; letter-spacing: -0.02em;
  color: var(--color-fg); text-decoration: none;
  opacity: 0.6;
  transition: opacity var(--dur-swift);
}
.cc-logo-wall__item a:hover { opacity: 1; }
.cc-logo-wall[data-variant="marquee"] .cc-logo-wall__items {
  display: flex; gap: 4rem;
  animation: marquee-scroll 30s linear infinite;
  width: max-content;
}
.cc-logo-wall[data-variant="marquee"] { overflow: hidden; }
.cc-logo-wall[data-variant="monolith"] .cc-logo-wall__items {
  grid-template-columns: 1fr;
  border-top: 1px solid var(--color-border);
}
.cc-logo-wall[data-variant="monolith"] .cc-logo-wall__item {
  padding: 2rem 0;
  border-bottom: 1px solid var(--color-border);
  width: 100%;
  justify-self: stretch;
  text-align: center;
}
.cc-logo-wall[data-variant="monolith"] .cc-logo-wall__item a { font-size: var(--type-display); }
```

- [ ] **Step 3: Commit**

```bash
git add sections/logo-wall.html sections/sections.css
git commit -m "Add logo-wall section partial (grid/marquee/monolith variants)"
```

---

End of Part 3. Continue to [part-4-sections-visual.md](2026-05-20-part-4-sections-visual.md).
