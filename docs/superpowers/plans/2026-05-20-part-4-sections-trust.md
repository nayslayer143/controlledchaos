# Part 4: Trust + Social Proof Sections (Tasks 12–15)

Prev: [part-3-sections-conversion.md](2026-05-20-part-3-sections-conversion.md). Next: [part-5-sections-utility.md](2026-05-20-part-5-sections-utility.md).

Four sections that build trust and answer objections: `faq`, `testimonial`, `signup-form`, `integration-logos`. Same authoring contract; append CSS after the previous `/* end-* */` marker each time.

---

## Task 12: `faq` section

**Files:**
- Create: `sections/faq.html`
- Modify: `sections/sections.css` (append after `/* end-pricing */`)

- [ ] **Step 1: Write `sections/faq.html`**

```html
<!-- cc-section: faq
     best:  swiss, gallery, immersive
     ok:    -
     avoid: kinetic
     variants: accordion (default — one-at-a-time toggle), static (all visible), numbered
     placeholders: heading, items[] (question, answer) -->
<section class="cc-section cc-faq"
         data-pattern="faq"
         data-variant="accordion">
  <div class="container --regular flow motion-reveal">
    <h2 class="cc-faq__heading voice-monument --display">{{heading}}</h2>
    <div class="cc-faq__items">
      {{#items}}
      <details class="cc-faq__item">
        <summary class="cc-faq__question voice-monument">
          {{question}}
          <span class="cc-faq__marker" aria-hidden="true">+</span>
        </summary>
        <div class="cc-faq__answer">
          <p class="voice-swiss">{{answer}}</p>
        </div>
      </details>
      {{/items}}
    </div>
  </div>
</section>

<script>
  (function () {
    // Accordion-variant behavior: close siblings when one opens.
    // Static and numbered variants skip this (multiple can stay open).
    document.querySelectorAll('.cc-faq[data-variant="accordion"]').forEach(function (faq) {
      faq.querySelectorAll('.cc-faq__item').forEach(function (item) {
        item.addEventListener('toggle', function () {
          if (!item.open) return;
          faq.querySelectorAll('.cc-faq__item').forEach(function (sibling) {
            if (sibling !== item) sibling.open = false;
          });
        });
      });
    });
  })();
</script>
```

- [ ] **Step 2: Append faq CSS**

```css

/* --- faq --------------------------------------------------- */
.cc-faq__heading { margin: 0 0 3rem; max-width: 28ch; line-height: 1.1; }
.cc-faq__items { border-top: 1px solid var(--color-border); }
.cc-faq__item { border-bottom: 1px solid var(--color-border); }
.cc-faq__question {
  list-style: none;
  cursor: pointer;
  padding: 1.5rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 2rem;
  font-size: var(--type-headline);
  font-style: italic;
  line-height: 1.2;
  transition: color var(--dur-swift);
}
.cc-faq__question::-webkit-details-marker { display: none; }
.cc-faq__question:hover { color: var(--color-accent-1); }
.cc-faq__marker {
  flex: 0 0 auto;
  font-family: var(--font-monument);
  font-size: var(--type-headline);
  font-style: normal;
  color: var(--color-accent-1);
  transition: transform var(--dur-swift);
  display: inline-block;
  width: 1.5ch; text-align: center;
}
.cc-faq__item[open] .cc-faq__marker { transform: rotate(45deg); }
.cc-faq__answer {
  padding-bottom: 1.5rem;
  max-width: 65ch;
}
.cc-faq__answer p {
  font-size: var(--type-body);
  line-height: 1.65;
  color: var(--color-accent-2);
  margin: 0;
}
.cc-faq[data-variant="static"] .cc-faq__marker { display: none; }
.cc-faq[data-variant="static"] .cc-faq__item {
  /* render answer always visible */
}
.cc-faq[data-variant="static"] .cc-faq__answer { display: block !important; }
.cc-faq[data-variant="numbered"] .cc-faq__items { counter-reset: cc-faq; border-top: 0; }
.cc-faq[data-variant="numbered"] .cc-faq__item { counter-increment: cc-faq; }
.cc-faq[data-variant="numbered"] .cc-faq__question::before {
  content: counter(cc-faq, decimal-leading-zero);
  font-family: var(--font-brutalist);
  font-size: var(--type-caption);
  color: var(--color-accent-1);
  letter-spacing: 0.15em;
  margin-right: 1rem;
  font-style: normal;
}
@media (prefers-reduced-motion: reduce) {
  .cc-faq__marker { transition: none; }
}
/* end-faq */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/faq.html sections/sections.css
git commit -m "Add faq section partial (accordion/static/numbered variants)

Native <details>/<summary> — keyboard-accessible without ARIA
ceremony. Accordion variant closes siblings via 8 lines of JS.
Static keeps everything open. Numbered prefixes each question
with a brutalist counter. Marker rotates 45° on [open].

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 13: `testimonial` section

**Files:**
- Create: `sections/testimonial.html`
- Modify: `sections/sections.css` (append after `/* end-faq */`)

- [ ] **Step 1: Write `sections/testimonial.html`**

```html
<!-- cc-section: testimonial
     best:  all
     ok:    -
     avoid: -
     variants: card (default — single quote), grid (3 side-by-side), featured (large single with company logo)
     placeholders: testimonials[] (quote, author, role, company, avatar_slot (optional, used for monogram), monogram (string, optional — defaults to first letter of author)) -->
<section class="cc-section cc-testimonial"
         data-pattern="testimonial"
         data-variant="card">
  <div class="container --regular motion-reveal">
    <div class="cc-testimonial__items motion-stagger">
      {{#testimonials}}
      <figure class="cc-quote-card">
        <blockquote class="cc-quote-card__quote">
          <p class="voice-editorial --display">{{quote}}</p>
        </blockquote>
        <figcaption class="cc-quote-card__author">
          <span class="cc-quote-card__avatar" aria-hidden="true">{{monogram}}</span>
          <span class="cc-quote-card__meta">
            <span class="cc-quote-card__name voice-monument">{{author}}</span>
            <span class="cc-quote-card__role voice-swiss">{{role}} · {{company}}</span>
          </span>
        </figcaption>
      </figure>
      {{/testimonials}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append testimonial CSS**

```css

/* --- testimonial ------------------------------------------- */
.cc-testimonial__items {
  display: grid;
  gap: clamp(2rem, 4vw, 3rem);
  grid-template-columns: 1fr;
}
.cc-testimonial[data-variant="grid"] .cc-testimonial__items {
  grid-template-columns: repeat(3, 1fr);
}
@media (max-width: 960px) {
  .cc-testimonial[data-variant="grid"] .cc-testimonial__items { grid-template-columns: 1fr; }
}
.cc-quote-card {
  border-top: 1px solid var(--color-border);
  padding-top: 2rem;
  margin: 0;
  min-width: 0;
  transition: border-color var(--dur-swift);
}
.cc-quote-card:hover { border-color: var(--color-accent-2); }
.cc-quote-card__quote { margin: 0 0 2rem; position: relative; padding-top: 1.5rem; }
.cc-quote-card__quote::before {
  content: "\201C";
  position: absolute;
  top: -0.2em; left: -0.1em;
  font-family: var(--font-monument);
  font-size: 3em;
  line-height: 1;
  color: var(--color-accent-1);
  font-style: normal;
}
.cc-quote-card__quote p {
  margin: 0;
  font-size: clamp(1.25rem, 2vw, 1.75rem);
  line-height: 1.4;
  font-style: italic;
  color: var(--color-fg);
}
.cc-quote-card__author { display: flex; align-items: center; gap: 1rem; }
.cc-quote-card__avatar {
  width: 56px; height: 56px;
  flex: 0 0 56px;
  border-radius: 50%;
  background:
    radial-gradient(circle at 30% 30%,
      color-mix(in oklch, var(--color-accent-1) 30%, var(--color-surface)),
      color-mix(in oklch, var(--color-accent-2) 15%, var(--color-surface)));
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-monument);
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--color-accent-2);
  letter-spacing: -0.02em;
}
.cc-quote-card__meta { display: flex; flex-direction: column; gap: 0.125rem; }
.cc-quote-card__name { font-size: var(--type-body); font-weight: 700; }
.cc-quote-card__role {
  font-size: var(--type-body-small);
  color: var(--color-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

/* Featured variant: larger quote, centered */
.cc-testimonial[data-variant="featured"] .cc-testimonial__items { max-width: 60ch; margin: 0 auto; }
.cc-testimonial[data-variant="featured"] .cc-quote-card__quote p {
  font-size: clamp(1.75rem, 3vw, 2.75rem);
  line-height: 1.25;
}
/* end-testimonial */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/testimonial.html sections/sections.css
git commit -m "Add testimonial section partial (card/grid/featured variants)

Editorial italic quote with decorative opening quote mark in
accent color. Monogram avatar via radial-gradient placeholder
(NO photo dependency — agent supplies a 1-2 char monogram).
Grid variant goes 3-up at desktop, 1-up under 960px. Featured
variant centers and enlarges the quote.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 14: `signup-form` section

**Files:**
- Create: `sections/signup-form.html`
- Modify: `sections/sections.css` (append after `/* end-testimonial */`)

- [ ] **Step 1: Write `sections/signup-form.html`**

```html
<!-- cc-section: signup-form
     best:  all
     ok:    -
     avoid: brutalist
     variants: inline (default — label+input+button on a row), stacked (vertical), framed (in a card with heading + supporting)
     placeholders: heading (optional), supporting (optional), placeholder_text, button_text, action_href, field_name (default 'email'), input_id (default 'cc-signup-email') -->
<section class="cc-section cc-signup"
         data-pattern="signup-form"
         data-variant="inline">
  <div class="container --regular flow motion-reveal">
    <h2 class="cc-signup__heading voice-monument --display">{{heading}}</h2>
    <p class="cc-signup__supporting voice-swiss">{{supporting}}</p>
    <form class="cc-signup__form" method="POST" action="{{action_href}}" aria-describedby="cc-signup-help">
      <label class="visually-hidden" for="{{input_id}}">{{placeholder_text}}</label>
      <input type="email"
             id="{{input_id}}"
             name="{{field_name}}"
             autocomplete="email"
             required
             placeholder="{{placeholder_text}}"
             class="cc-signup__input">
      <button type="submit" class="cc-signup__button voice-swiss --label">{{button_text}}</button>
    </form>
    <small id="cc-signup-help" class="cc-signup__help voice-swiss">
      No spam. Unsubscribe anytime.
    </small>
  </div>
</section>
```

- [ ] **Step 2: Append signup-form CSS**

```css

/* --- signup-form ------------------------------------------- */
.cc-signup__heading { margin: 0 0 1rem; max-width: 22ch; line-height: 1.1; }
.cc-signup__supporting { max-width: 50ch; color: var(--color-accent-2); margin-bottom: 1.5rem; }
.cc-signup__form {
  display: flex;
  gap: 0.5rem;
  max-width: 480px;
  align-items: stretch;
}
.cc-signup__input {
  flex: 1;
  /* base input styles (border, padding, focus) come from tokens/states.css */
}
.cc-signup__button {
  padding: 0 1.5rem;
  border: 1px solid var(--color-fg);
  background: var(--color-fg);
  color: var(--color-bg);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  cursor: pointer;
  white-space: nowrap;
}
.cc-signup__button:hover { background: var(--color-accent-1); border-color: var(--color-accent-1); }
.cc-signup__help {
  display: block;
  margin-top: 0.75rem;
  color: var(--color-muted);
  font-size: var(--type-body-small);
}

/* Stacked variant */
.cc-signup[data-variant="stacked"] .cc-signup__form { flex-direction: column; max-width: 360px; }
.cc-signup[data-variant="stacked"] .cc-signup__button { padding: 0.875rem 1.5rem; }

/* Framed variant: form in a card */
.cc-signup[data-variant="framed"] .cc-signup__form,
.cc-signup[data-variant="framed"] .cc-signup__heading,
.cc-signup[data-variant="framed"] .cc-signup__supporting,
.cc-signup[data-variant="framed"] .cc-signup__help {
  /* container will be a card via the wrapper class below */
}
.cc-signup[data-variant="framed"] > .container {
  padding: clamp(2rem, 4vw, 3rem);
  border: 1px solid var(--color-border);
  background: var(--color-surface);
  max-width: 56ch;
}

@media (max-width: 600px) {
  .cc-signup__form { flex-direction: column; }
  .cc-signup__button { padding: 0.875rem 1.5rem; }
}
/* end-signup-form */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/signup-form.html sections/sections.css
git commit -m "Add signup-form section partial (inline/stacked/framed variants)

Accessible <form> with visually-hidden <label>, autocomplete=email,
required validation. Input/button states inherit from tokens/
states.css. aria-describedby wires help text to the input for
screen readers. Stacks to vertical under 600px regardless of
variant.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 15: `integration-logos` section

**Files:**
- Create: `sections/integration-logos.html`
- Modify: `sections/sections.css` (append after `/* end-signup-form */`)

- [ ] **Step 1: Write `sections/integration-logos.html`**

```html
<!-- cc-section: integration-logos
     best:  swiss, gallery
     ok:    -
     avoid: kinetic
     variants: grid (default — hairline-bordered cells), inline (single horizontal row), categorized (grouped under category labels)
     placeholders: heading, logos[] (label, href (optional), category (optional, used by categorized variant))
     note: text-only "logos" — agent supplies labels. CC does not ship third-party logo SVGs. -->
<section class="cc-section cc-integrations"
         data-pattern="integration-logos"
         data-variant="grid">
  <div class="container --wide flow motion-reveal">
    <h2 class="cc-integrations__heading voice-swiss --label">{{heading}}</h2>
    <ul class="cc-integrations__items">
      {{#logos}}
      <li class="cc-integrations__item" {{#category}}data-category="{{category}}"{{/category}}>
        {{#href}}<a href="{{href}}" class="voice-monument">{{label}}</a>{{/href}}
        {{^href}}<span class="voice-monument">{{label}}</span>{{/href}}
      </li>
      {{/logos}}
    </ul>
  </div>
</section>
```

- [ ] **Step 2: Append integration-logos CSS**

```css

/* --- integration-logos ------------------------------------- */
.cc-integrations__heading {
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 0.2em;
  color: var(--color-muted);
  margin: 0 0 2.5rem;
}
.cc-integrations__items {
  list-style: none; padding: 0; margin: 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  border-top: 1px solid var(--color-border);
  border-left: 1px solid var(--color-border);
}
.cc-integrations__item {
  border-right: 1px solid var(--color-border);
  border-bottom: 1px solid var(--color-border);
  padding: 2rem 1rem;
  display: flex; align-items: center; justify-content: center;
  min-height: 100px;
}
.cc-integrations__item a,
.cc-integrations__item span {
  font-family: var(--font-monument);
  font-size: var(--type-subhead);
  text-transform: uppercase;
  letter-spacing: -0.02em;
  color: var(--color-fg);
  text-decoration: none;
  opacity: 0.65;
  transition: opacity var(--dur-swift);
}
.cc-integrations__item a:hover { opacity: 1; }

/* Inline variant: single row, no grid */
.cc-integrations[data-variant="inline"] .cc-integrations__items {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem 3rem;
  border: 0;
}
.cc-integrations[data-variant="inline"] .cc-integrations__item {
  border: 0;
  padding: 0;
  min-height: 0;
}

/* Categorized variant: items are grouped client-side by data-category;
   simplest implementation is to keep grid layout but render category
   labels above the relevant runs. Agent inserts category breakers as
   <li class="cc-integrations__break">CATEGORY</li> elements between
   item groups. */
.cc-integrations__break {
  grid-column: 1 / -1;
  border-right: 0;
  border-bottom: 0;
  padding: 1.5rem 1rem 0.5rem;
  font-family: var(--font-brutalist);
  font-size: var(--type-caption);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  color: var(--color-accent-1);
}
/* end-integration-logos */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/integration-logos.html sections/sections.css
git commit -m "Add integration-logos section partial (grid/inline/categorized variants)

Auto-fit grid (150px min) with hairline-bordered cells — gridlines
visible between logos. Text-only labels (CC does not ship third-
party logo SVGs). Inline variant flows logos in a single wrapping
row. Categorized variant uses agent-inserted .cc-integrations__break
LI rows as category headers.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

End of Part 4. Continue to [part-5-sections-utility.md](2026-05-20-part-5-sections-utility.md).
