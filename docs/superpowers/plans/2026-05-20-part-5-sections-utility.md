# Part 5: Utility Sections (Tasks 16–17)

Prev: [part-4-sections-trust.md](2026-05-20-part-4-sections-trust.md). Next: [part-6-catalog-showcase-docs.md](2026-05-20-part-6-catalog-showcase-docs.md).

Two final sections: `cta-banner` (inline + dismissible sticky-bottom) and `comparison-table` (vs-competitors matrix).

---

## Task 16: `cta-banner` section

**Files:**
- Create: `sections/cta-banner.html`
- Modify: `sections/sections.css` (append after `/* end-integration-logos */`)

- [ ] **Step 1: Write `sections/cta-banner.html`**

```html
<!-- cc-section: cta-banner
     best:  all
     ok:    -
     avoid: -
     variants: inline (default — between sections, hairline above + below), sticky-bottom (fixed at bottom, dismissible), divider (decorative orange hairline between sections)
     placeholders: heading, supporting (optional), action_text, action_href, dismissible (bool, defaults true for sticky-bottom), banner_key (string for localStorage dismissal key, defaults 'cc-banner-default') -->
<section class="cc-section cc-banner"
         data-pattern="cta-banner"
         data-variant="inline"
         data-key="{{banner_key}}">
  <div class="container --wide cc-banner__inner">
    <div class="cc-banner__copy">
      <h2 class="cc-banner__heading voice-monument">{{heading}}</h2>
      <p class="cc-banner__supporting voice-swiss">{{supporting}}</p>
    </div>
    <a class="cc-banner__action voice-swiss --label" href="{{action_href}}">
      {{action_text}}
      <span aria-hidden="true">→</span>
    </a>
    <button class="cc-banner__dismiss" type="button" aria-label="Dismiss" hidden>
      <span aria-hidden="true">×</span>
    </button>
  </div>
</section>

<script>
  (function () {
    document.querySelectorAll('.cc-banner[data-variant="sticky-bottom"]').forEach(function (banner) {
      var key = 'cc-banner-dismissed-' + (banner.dataset.key || 'default');
      if (localStorage.getItem(key) === '1') {
        banner.remove();
        return;
      }
      var dismiss = banner.querySelector('.cc-banner__dismiss');
      if (dismiss) {
        dismiss.hidden = false;
        dismiss.addEventListener('click', function () {
          localStorage.setItem(key, '1');
          banner.setAttribute('data-dismissing', '');
          setTimeout(function () { banner.remove(); }, 300);
        });
      }
    });
  })();
</script>
```

- [ ] **Step 2: Append cta-banner CSS**

```css

/* --- cta-banner -------------------------------------------- */
.cc-banner__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  flex-wrap: wrap;
}
.cc-banner__copy { flex: 1 1 280px; min-width: 0; }
.cc-banner__heading {
  font-size: var(--type-headline);
  font-style: italic;
  font-weight: 700;
  margin: 0 0 0.25rem;
  line-height: 1.2;
}
.cc-banner__supporting {
  margin: 0;
  color: var(--color-accent-2);
  font-size: var(--type-body);
}
.cc-banner__action {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.875rem 1.5rem;
  border: 1px solid var(--color-fg);
  color: var(--color-fg);
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  white-space: nowrap;
}
.cc-banner__action:hover { background: var(--color-fg); color: var(--color-bg); }
.cc-banner__dismiss {
  background: transparent;
  border: 0;
  width: 32px; height: 32px;
  font-size: 1.5rem;
  line-height: 1;
  color: var(--color-muted);
  cursor: pointer;
  transition: color var(--dur-swift);
}
.cc-banner__dismiss:hover { color: var(--color-fg); }

/* Sticky-bottom variant */
.cc-banner[data-variant="sticky-bottom"] {
  position: fixed;
  bottom: 0; left: 0; right: 0;
  z-index: 90;
  background: color-mix(in oklch, var(--color-bg) 70%, transparent);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-top: 1px solid var(--color-border);
  padding-block: 1rem;
  transform: translateY(0);
  transition: transform var(--dur-natural) var(--ease-out-expo);
  animation: cc-banner-rise var(--dur-natural) var(--ease-out-expo) 0.5s both;
}
.cc-banner[data-variant="sticky-bottom"][data-dismissing] {
  transform: translateY(100%);
}
@keyframes cc-banner-rise {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}
@media (prefers-reduced-motion: reduce) {
  .cc-banner[data-variant="sticky-bottom"] { animation: none; transition: none; }
}

/* Divider variant: decorative hairline, smaller padding */
.cc-banner[data-variant="divider"] {
  padding-block: 2rem;
  border-top: 1px solid var(--color-accent-1);
  border-bottom: 1px solid var(--color-accent-1);
}

@media (max-width: 720px) {
  .cc-banner__inner { flex-direction: column; align-items: flex-start; }
  .cc-banner__action { width: 100%; justify-content: center; }
  .cc-banner[data-variant="sticky-bottom"] { padding-block: 0.75rem; }
  .cc-banner[data-variant="sticky-bottom"] .cc-banner__heading { font-size: var(--type-body); }
  .cc-banner[data-variant="sticky-bottom"] .cc-banner__supporting { display: none; }
}
/* end-cta-banner */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/cta-banner.html sections/sections.css
git commit -m "Add cta-banner section partial (inline/sticky-bottom/divider variants)

Inline: hairline-bordered strip between sections. Sticky-bottom:
fixed at viewport bottom with backdrop-blur, rises in after 500ms,
dismissible via X button that writes to localStorage (per-page key
via data-key). Once dismissed, stays gone. Reduced-motion zeros
the rise animation. Divider variant: orange hairlines as decorative
section break.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 17: `comparison-table` section

**Files:**
- Create: `sections/comparison-table.html`
- Modify: `sections/sections.css` (append after `/* end-cta-banner */`)

- [ ] **Step 1: Write `sections/comparison-table.html`**

```html
<!-- cc-section: comparison-table
     best:  swiss, gallery
     ok:    -
     avoid: brutalist, kinetic
     variants: vs-2 (default — you vs 1 competitor), vs-3 (you vs 2), vs-many (you vs N, horizontal scroll on mobile)
     placeholders: heading, eyebrow (optional), columns[] (name, is_you (bool)), rows[] (feature, values[] aligned with columns — each value is "✓", "—", or a short string) -->
<section class="cc-section cc-compare"
         data-pattern="comparison-table"
         data-variant="vs-2">
  <div class="container --wide flow motion-reveal">
    <span class="cc-compare__eyebrow voice-swiss --label">{{eyebrow}}</span>
    <h2 class="cc-compare__heading voice-monument --display">{{heading}}</h2>
    <div class="cc-compare__scroll">
      <table class="cc-compare__table">
        <thead>
          <tr>
            <th scope="col" class="cc-compare__feature-col"></th>
            {{#columns}}
            <th scope="col" class="cc-compare__col {{#is_you}}cc-compare__col--you{{/is_you}}">
              <span class="voice-monument">{{name}}</span>
            </th>
            {{/columns}}
          </tr>
        </thead>
        <tbody>
          {{#rows}}
          <tr>
            <th scope="row" class="cc-compare__feature voice-swiss">{{feature}}</th>
            {{#values}}
            <td class="cc-compare__cell">{{.}}</td>
            {{/values}}
          </tr>
          {{/rows}}
        </tbody>
      </table>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append comparison-table CSS**

```css

/* --- comparison-table -------------------------------------- */
.cc-compare__eyebrow {
  display: block;
  color: var(--color-accent-1);
  font-family: var(--font-brutalist);
  letter-spacing: 0.18em;
  margin-bottom: 1rem;
}
.cc-compare__heading { margin: 0 0 3rem; max-width: 28ch; line-height: 1.1; }
.cc-compare__scroll { overflow-x: auto; }
.cc-compare__table {
  width: 100%;
  border-collapse: collapse;
  font-size: var(--type-body);
}
.cc-compare__table th,
.cc-compare__table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid var(--color-border);
  vertical-align: top;
}
.cc-compare__col {
  font-family: var(--font-monument);
  font-size: var(--type-subhead);
  font-style: italic;
  font-weight: 400;
  color: var(--color-fg);
  text-align: left;
}
.cc-compare__col--you {
  background: color-mix(in oklch, var(--color-accent-1) 6%, transparent);
  border-top: 2px solid var(--color-accent-1);
}
.cc-compare__feature-col { width: 30%; }
.cc-compare__feature {
  font-weight: 500;
  color: var(--color-fg);
  position: sticky;
  left: 0;
  background: var(--color-bg);
  z-index: 1;
}
.cc-compare__cell {
  color: var(--color-accent-2);
  font-family: var(--font-swiss);
}

/* Highlight cells in the "you" column */
.cc-compare__table tbody td:nth-child(2):nth-last-child(1) { /* edge case */ }

/* Use data attribute on table to mark which column index is "you"
   (for vs-2 it's typically 1; agent sets via <table data-you-col="N"> if non-default) */
.cc-compare__table[data-you-col="1"] td:nth-child(2),
.cc-compare__table:not([data-you-col]) td:nth-child(2) {
  background: color-mix(in oklch, var(--color-accent-1) 4%, transparent);
  color: var(--color-fg);
}
.cc-compare__table[data-you-col="2"] td:nth-child(3) {
  background: color-mix(in oklch, var(--color-accent-1) 4%, transparent);
  color: var(--color-fg);
}

/* vs-many variant: horizontal scroll with sticky first column */
.cc-compare[data-variant="vs-many"] .cc-compare__table { min-width: 700px; }
@media (max-width: 720px) {
  .cc-compare__table { min-width: 560px; }
  .cc-compare__feature { font-size: var(--type-body-small); }
}
/* end-comparison-table */
```

- [ ] **Step 3: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add sections/comparison-table.html sections/sections.css
git commit -m "Add comparison-table section partial (vs-2/vs-3/vs-many variants)

Semantic <table> with <thead>/<tbody>/<th scope> for a11y. 'You'
column highlighted with accent-tinted background + accent top
border. First column (feature names) is position:sticky so it
stays visible during horizontal scroll on narrow viewports.
vs-many forces 700px min-width to enable the scroll affordance.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

End of Part 5. All 10 new sections complete. Continue to [part-6-catalog-showcase-docs.md](2026-05-20-part-6-catalog-showcase-docs.md).
