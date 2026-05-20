# Section Library

Reusable HTML partials. Each section is a single `.html` file with Mustache
placeholders the agent fills via string substitution. No runtime, no JS.

## Authoring contract

Every partial:
- Begins with a top-comment listing best/ok/avoid modes + variants + placeholders
- Root element carries `class="cc-section cc-<name>"`, `data-pattern="<name>"`, `data-variant="<default>"`
- Uses Mustache: `{{scalar}}` for substitution, `{{#items}}…{{/items}}` for repeats
- Image slots use `<div class="cc-img-placeholder" data-slot="<name>"></div>`
- Section-specific CSS lives in `sections/sections.css`

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
| [full-bleed](full-bleed.html) | type, image, color-field | type → content; image → slot_name; color-field → content, optional --bleed-color | immersive, brutalist, kinetic | — |
| [marquee](marquee.html) | default, slow, vertical | items[] (text) | kinetic, immersive, brutalist | gallery |

## Image policy

Sections with image slots (`hero.bleed`, `gallery`, `full-bleed.image`, `logo-wall`) use
placeholder / agent-sourced / user-supplied content. **Never** pull images from a
style-guide source board — style-guide imagery is reference-only.

## Use from a template

Read `templates/page-composed.html` for a complete scaffold. The recommended order
for a typical page is:

    hero → manifesto / stat-grid / quote / two-column / process / gallery / full-bleed / marquee
         → cta → footer

For style.json-driven authoring, honor `dos[]` / `donts[]` when picking sections and
variants — see `adapters/from-style-json.md`.
