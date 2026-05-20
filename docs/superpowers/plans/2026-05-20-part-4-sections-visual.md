# Part 4: Visual/Motion Sections (Tasks 13–15)

Prev: [part-3-sections-data.md](2026-05-20-part-3-sections-data.md). Next: [part-5-catalog-adapter.md](2026-05-20-part-5-catalog-adapter.md).

These three sections are the most visually loaded. They use the `cc-img-placeholder` slot pattern from the scaffold — never embed images from a style-guide source board.

---

## Task 13: `gallery` section

**Files:**
- Create: `sections/gallery.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/gallery.html`**

```html
<!-- cc-section: gallery
     best:  gallery, immersive, kinetic
     ok:    swiss
     avoid: brutalist (image grids cool the volume)
     variants: rigid, asymmetric, mosaic
     placeholders: heading, items[] (slot_name, optional caption) -->
<section class="cc-section cc-gallery"
         data-pattern="gallery"
         data-variant="rigid">
  <div class="container --wide flow">
    <h2 class="cc-gallery__heading voice-swiss --label">{{heading}}</h2>
    <div class="cc-gallery__grid">
      {{#items}}
      <figure class="cc-gallery__item">
        <div class="cc-img-placeholder" data-slot="{{slot_name}}"></div>
        <figcaption class="cc-gallery__caption voice-swiss --label">{{caption}}</figcaption>
      </figure>
      {{/items}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- gallery ------------------------------------------------ */
.cc-gallery__heading {
  text-transform: uppercase; letter-spacing: 0.2em;
  color: var(--color-muted); margin: 0 0 2rem;
}
.cc-gallery__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: clamp(1rem, 2vw, 2rem);
}
@media (max-width: 720px) { .cc-gallery__grid { grid-template-columns: repeat(2, 1fr); } }
.cc-gallery__item { margin: 0; }
.cc-gallery__caption {
  margin-top: 0.75rem; color: var(--color-muted);
  text-transform: uppercase; letter-spacing: 0.1em;
}
.cc-gallery[data-variant="asymmetric"] .cc-gallery__grid {
  grid-template-columns: 2fr 1fr 1fr;
  grid-auto-rows: 200px;
}
.cc-gallery[data-variant="asymmetric"] .cc-gallery__item:nth-child(3n+1) {
  grid-row: span 2;
}
.cc-gallery[data-variant="asymmetric"] .cc-gallery__item:nth-child(3n+1) .cc-img-placeholder {
  aspect-ratio: auto; height: 100%;
}
.cc-gallery[data-variant="mosaic"] .cc-gallery__grid {
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 160px;
}
.cc-gallery[data-variant="mosaic"] .cc-gallery__item:nth-child(5n+1) { grid-column: span 2; grid-row: span 2; }
.cc-gallery[data-variant="mosaic"] .cc-gallery__item:nth-child(5n+4) { grid-column: span 2; }
.cc-gallery[data-variant="mosaic"] .cc-img-placeholder { aspect-ratio: auto; height: 100%; }
```

- [ ] **Step 3: Commit**

```bash
git add sections/gallery.html sections/sections.css
git commit -m "Add gallery section partial (rigid/asymmetric/mosaic variants)"
```

---

## Task 14: `full-bleed` section

**Files:**
- Create: `sections/full-bleed.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/full-bleed.html`**

```html
<!-- cc-section: full-bleed
     best:  immersive, brutalist, kinetic
     ok:    swiss
     avoid: -
     variants: type, image, color-field
     placeholders: type variant → content (string)
                  image variant → slot_name (string)
                  color-field variant → content (string), optional --bleed-color override -->
<section class="cc-section cc-bleed"
         data-pattern="full-bleed"
         data-variant="type">
  <!-- TYPE variant: massive bleeding text -->
  <div class="cc-bleed__content cc-bleed__content--type">
    <span class="voice-monument" data-bleed-text>{{content}}</span>
  </div>
  <!-- IMAGE variant: full-bleed image placeholder
       <div class="cc-bleed__content cc-bleed__content--image">
         <div class="cc-img-placeholder" data-slot="{{slot_name}}"></div>
       </div>
       (Swap with the type variant above when data-variant="image") -->
  <!-- COLOR-FIELD variant:
       <div class="cc-bleed__content cc-bleed__content--color">
         <span class="voice-monument --massive">{{content}}</span>
       </div> -->
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- full-bleed --------------------------------------------- */
.cc-bleed { padding-inline: 0; padding-block: 0; overflow: hidden; }
.cc-bleed__content {
  width: 100vw; min-height: 60vh;
  display: flex; align-items: center; justify-content: center;
  position: relative;
}
.cc-bleed__content--type {
  background: var(--color-bg);
  padding: 0 4vw;
}
.cc-bleed__content--type [data-bleed-text] {
  font-size: clamp(6rem, 22vw, 28rem);
  line-height: 0.9;
  letter-spacing: -0.04em;
  text-align: center;
  color: var(--color-fg);
  word-break: break-word;
  text-transform: uppercase;
}
.cc-bleed__content--image { min-height: 80vh; }
.cc-bleed__content--image .cc-img-placeholder {
  aspect-ratio: auto;
  height: 80vh; width: 100%;
  min-height: 0;
}
.cc-bleed__content--color {
  background: var(--bleed-color, var(--color-discord));
  color: var(--color-bg);
  min-height: 70vh;
}
.cc-bleed__content--color [class*="voice-"] {
  font-size: clamp(3rem, 10vw, 10rem);
  line-height: 0.95;
  text-align: center;
  padding: 0 4vw;
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/full-bleed.html sections/sections.css
git commit -m "Add full-bleed section partial (type/image/color-field variants)"
```

---

## Task 15: `marquee` section

**Files:**
- Create: `sections/marquee.html`
- Modify: `sections/sections.css` (append)

- [ ] **Step 1: Write `sections/marquee.html`**

```html
<!-- cc-section: marquee
     best:  kinetic, immersive, brutalist
     ok:    swiss
     avoid: gallery (kills the silence)
     variants: default, slow, vertical
     placeholders: items[] (text)
     motion: respects prefers-reduced-motion (kept static when set) -->
<section class="cc-section cc-marquee-section"
         data-pattern="marquee"
         data-variant="default">
  <div class="cc-marquee__track">
    <div class="cc-marquee__inner motion-marquee">
      {{#items}}
      <span class="cc-marquee__item voice-monument">{{text}}</span>
      <span class="cc-marquee__sep" aria-hidden="true">●</span>
      {{/items}}
      <!-- Duplicate the items[] block once so the marquee animation loops seamlessly. -->
      {{#items}}
      <span class="cc-marquee__item voice-monument">{{text}}</span>
      <span class="cc-marquee__sep" aria-hidden="true">●</span>
      {{/items}}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append CSS to `sections/sections.css`**

```css
/* --- marquee ------------------------------------------------ */
.cc-marquee-section { padding-block: clamp(2rem, 4vh, 4rem); padding-inline: 0; overflow: hidden; }
.cc-marquee__track { width: 100vw; }
.cc-marquee__inner {
  display: flex; align-items: center; gap: 3rem;
  width: max-content;
}
.cc-marquee__item {
  font-size: clamp(2rem, 6vw, 6rem);
  line-height: 1;
  text-transform: uppercase;
  letter-spacing: -0.02em;
  white-space: nowrap;
}
.cc-marquee__sep { color: var(--color-discord); font-size: 1rem; }
.cc-marquee-section[data-variant="slow"] .motion-marquee { animation-duration: 60s; }
.cc-marquee-section[data-variant="vertical"] { padding-inline: 4vw; padding-block: 0; height: 80vh; overflow: hidden; }
.cc-marquee-section[data-variant="vertical"] .cc-marquee__track { width: auto; height: 100%; }
.cc-marquee-section[data-variant="vertical"] .cc-marquee__inner {
  flex-direction: column; width: auto; height: max-content;
  animation: marquee-scroll-vertical 30s linear infinite;
}
@keyframes marquee-scroll-vertical { from { transform: translateY(0); } to { transform: translateY(-50%); } }
@media (prefers-reduced-motion: reduce) {
  .cc-marquee__inner, .cc-marquee-section[data-variant="vertical"] .cc-marquee__inner { animation: none; }
}
```

- [ ] **Step 3: Commit**

```bash
git add sections/marquee.html sections/sections.css
git commit -m "Add marquee section partial (default/slow/vertical variants)"
```

---

End of Part 4. All 12 sections complete. Continue to [part-5-catalog-adapter.md](2026-05-20-part-5-catalog-adapter.md).
