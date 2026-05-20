# Part 2: Templates (Tasks 4–7)

Prev: [part-1-foundation.md](2026-05-20-part-1-foundation.md). Next: [part-3-sections-conversion.md](2026-05-20-part-3-sections-conversion.md).

Four small template files that make the skill "deploy ready." All four reuse the same `<link>` chain as `page-composed.html` so any style.json that drives the main page also drives 404 / privacy / terms.

---

## Task 4: `templates/meta.partial.html`

A standalone copy-pasteable snippet of just the `<head>` meta stack — useful for brands building outside the `page-composed.html` scaffold but who still want CC's SEO/OG defaults.

**Files:**
- Create: `templates/meta.partial.html`

- [ ] **Step 1: Write `templates/meta.partial.html`**

```html
<!--
  controlledchaos · meta.partial.html
  ====================================
  Copy these <meta> tags into the <head> of any page that should
  inherit CC's SEO/OG/Twitter/JSON-LD defaults. Mustache placeholders
  ({{...}}) are filled inline by the agent from the page brief.

  Schema stays vibe-only; this partial carries page-level meta only.
-->

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

<!-- Favicon -->
<link rel="icon" type="image/svg+xml" href="/favicon.svg">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">

<!-- JSON-LD Organization -->
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
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add templates/meta.partial.html
git commit -m "Add templates/meta.partial.html — standalone SEO/OG meta snippet

Copy-pasteable <head> stack for brands building outside the
page-composed.html scaffold. Same Mustache placeholders.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 5: `templates/404.html`

Style-driven 404 page. Uses the same `<link>` chain as `page-composed.html`, so whichever `style.json` drives the brand also styles the 404.

**Files:**
- Create: `templates/404.html`

- [ ] **Step 1: Write `templates/404.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="signal" data-forces="immersive">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{seo.title}} · 404</title>
  <meta name="description" content="Page not found.">
  <meta name="theme-color" content="{{seo.theme_color}}">

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../tokens/states.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <!-- STYLE-GUIDE-OVERRIDES (filled by adapter when style.json drives) -->
  <style id="style-overrides"></style>

  <style>
    body {
      font-family: var(--font-swiss);
      background: var(--color-bg);
      color: var(--color-fg);
      margin: 0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    main { flex: 1; display: flex; flex-direction: column; justify-content: center; }
    .cc-404__code {
      font-family: var(--font-monument);
      font-size: clamp(4rem, 14vw, 12rem);
      line-height: 0.9;
      letter-spacing: -0.04em;
      color: var(--color-discord);
      margin: 0 0 1rem;
    }
  </style>
</head>
<body>
  <main>
    <section class="cc-section cc-hero" data-pattern="hero" data-variant="editorial">
      <div class="container --regular flow">
        <span class="cc-hero__eyebrow voice-swiss --label">404 · PAGE NOT FOUND</span>
        <p class="cc-404__code">404</p>
        <h1 class="cc-hero__heading voice-monument --massive">The room is dark.</h1>
        <p class="cc-hero__subhead voice-swiss">The page you wanted didn't make it past the door. The link may be old, mistyped, or moved. Head back to the surface and try again.</p>
      </div>
    </section>

    <section class="cc-section cc-cta" data-pattern="cta" data-variant="default">
      <div class="container --regular flow">
        <a class="cc-cta__action voice-swiss --label" href="/">
          ← Back to home
          <span class="cc-cta__arrow" aria-hidden="true">→</span>
        </a>
      </div>
    </section>
  </main>

  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="minimal">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">{{org.name}} · {{seo.year}}</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="/">Home</a>
      </nav>
    </div>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add templates/404.html
git commit -m "Add templates/404.html — style-driven 404 page

Reuses the full <link> chain so whichever style.json drives the
brand also styles the 404. Three sections: hero (editorial variant
with massive discord-colored '404' numeric), CTA back to home,
minimal footer. Brand-tunable via the same overrides as the main
page.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 6: `templates/privacy.html`

Editorial privacy-policy scaffold. Brand replaces lorem with real policy.

**Files:**
- Create: `templates/privacy.html`

- [ ] **Step 1: Write `templates/privacy.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="signal" data-forces="swiss">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{org.name}} · Privacy Policy</title>
  <meta name="description" content="Privacy policy for {{org.name}}.">
  <meta name="theme-color" content="{{seo.theme_color}}">

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../tokens/states.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <style id="style-overrides"></style>
  <style>
    body { font-family: var(--font-swiss); background: var(--color-bg); color: var(--color-fg); margin: 0; }
    .cc-policy { max-width: 70ch; padding-block: clamp(4rem, 8vh, 8rem); }
    .cc-policy h2 { font-family: var(--font-editorial); font-style: italic; margin-top: 3rem; }
    .cc-policy h3 { font-family: var(--font-monument); font-size: var(--type-headline); margin-top: 2rem; }
    .cc-policy p, .cc-policy li { font-size: var(--type-body); line-height: 1.65; max-width: 65ch; }
    .cc-policy ul { padding-left: 1.5rem; }
    .cc-policy a { color: var(--color-discord); }
  </style>
</head>
<body>
  <main class="container --regular cc-policy">
    <span class="voice-swiss --label" style="color: var(--color-muted); letter-spacing: 0.15em;">PRIVACY POLICY · LAST UPDATED {{policy.updated_date}}</span>
    <h1 class="voice-monument --display" style="margin: 1rem 0 2rem; text-transform: none;">Privacy policy</h1>

    <p>This is a placeholder privacy policy for <strong>{{org.name}}</strong>. Replace this content with your real policy before publishing. The structure below is a starting point.</p>

    <h2>1. What we collect</h2>
    <p>Describe the personal data your service collects: account info, usage data, cookies, payment data, third-party integrations.</p>
    <ul>
      <li>Account information (e.g., name, email, password hash)</li>
      <li>Usage data (pages viewed, actions taken, device info)</li>
      <li>Communications (support emails, feedback)</li>
    </ul>

    <h2>2. How we use it</h2>
    <p>Explain the legitimate-interest, contractual, consent-based, and legal bases for processing.</p>

    <h2>3. Who we share it with</h2>
    <p>List sub-processors (hosting, analytics, payments, support). Link to their privacy policies.</p>

    <h2>4. How long we keep it</h2>
    <p>Retention windows by data type. When account data is deleted on request.</p>

    <h2>5. Your rights</h2>
    <p>Access, rectification, deletion, portability, objection. How to contact the data protection officer at <a href="mailto:{{org.privacy_email}}">{{org.privacy_email}}</a>.</p>

    <h2>6. Cookies</h2>
    <p>What cookies the site uses. Which are essential vs analytics vs marketing. How to opt out.</p>

    <h2>7. Changes</h2>
    <p>Notification policy when the policy changes.</p>

    <h2>8. Contact</h2>
    <p>How to reach the privacy team. Physical mailing address if required by jurisdiction.</p>
  </main>

  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="minimal">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">{{org.name}} · {{seo.year}}</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="/">Home</a>
        <a class="cc-footer__link voice-swiss --label" href="/terms">Terms</a>
      </nav>
    </div>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add templates/privacy.html
git commit -m "Add templates/privacy.html — editorial privacy-policy scaffold

8-section structure with placeholder lorem the brand replaces with
real policy. Uses the swiss force preset for legal-document
clarity. Same <link> chain so style.json overrides reach it.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

## Task 7: `templates/terms.html`

Editorial terms-of-service scaffold. Mirror structure of privacy.html.

**Files:**
- Create: `templates/terms.html`

- [ ] **Step 1: Write `templates/terms.html`**

```html
<!DOCTYPE html>
<html lang="en" data-mood="signal" data-forces="swiss">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{org.name}} · Terms of Service</title>
  <meta name="description" content="Terms of service for {{org.name}}.">
  <meta name="theme-color" content="{{seo.theme_color}}">

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Inter:wght@400;500;700&family=Space+Mono:wght@400;700&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="../tokens/layout.css">
  <link rel="stylesheet" href="../tokens/typography.css">
  <link rel="stylesheet" href="../tokens/color.css">
  <link rel="stylesheet" href="../tokens/motion.css">
  <link rel="stylesheet" href="../tokens/forces.css">
  <link rel="stylesheet" href="../tokens/textures.css">
  <link rel="stylesheet" href="../tokens/states.css">
  <link rel="stylesheet" href="../sections/sections.css">

  <style id="style-overrides"></style>
  <style>
    body { font-family: var(--font-swiss); background: var(--color-bg); color: var(--color-fg); margin: 0; }
    .cc-policy { max-width: 70ch; padding-block: clamp(4rem, 8vh, 8rem); }
    .cc-policy h2 { font-family: var(--font-editorial); font-style: italic; margin-top: 3rem; }
    .cc-policy h3 { font-family: var(--font-monument); font-size: var(--type-headline); margin-top: 2rem; }
    .cc-policy p, .cc-policy li { font-size: var(--type-body); line-height: 1.65; max-width: 65ch; }
    .cc-policy ul { padding-left: 1.5rem; }
    .cc-policy a { color: var(--color-discord); }
  </style>
</head>
<body>
  <main class="container --regular cc-policy">
    <span class="voice-swiss --label" style="color: var(--color-muted); letter-spacing: 0.15em;">TERMS OF SERVICE · LAST UPDATED {{policy.updated_date}}</span>
    <h1 class="voice-monument --display" style="margin: 1rem 0 2rem; text-transform: none;">Terms of service</h1>

    <p>This is a placeholder terms-of-service document for <strong>{{org.name}}</strong>. Replace this content with your real terms before publishing.</p>

    <h2>1. Acceptance</h2>
    <p>By accessing or using {{org.name}}, you agree to be bound by these terms.</p>

    <h2>2. The service</h2>
    <p>What the service does. What it does not do. Eligibility (age, jurisdiction, capacity).</p>

    <h2>3. Accounts</h2>
    <p>Account creation, security, accuracy of information, responsibility for activity under your account.</p>

    <h2>4. Acceptable use</h2>
    <p>What users may do. What is prohibited (abuse, spam, IP infringement, illegal content, reverse engineering, automated scraping, etc.).</p>

    <h2>5. Content</h2>
    <p>User-generated content: ownership, license granted to {{org.name}}, removal policy. {{org.name}}'s content: copyright, trademark, permitted uses.</p>

    <h2>6. Payments</h2>
    <p>If applicable: pricing, billing cycle, refunds, taxes, late payments, plan changes.</p>

    <h2>7. Termination</h2>
    <p>How users can terminate. How {{org.name}} can terminate. What survives termination (sections 5, 8, 9, 10).</p>

    <h2>8. Disclaimers</h2>
    <p>Service provided "as is." No warranty of fitness or non-infringement. Third-party content disclaimers.</p>

    <h2>9. Limitation of liability</h2>
    <p>To the extent permitted by law: limits on direct and indirect damages. Cap on liability.</p>

    <h2>10. Governing law &amp; disputes</h2>
    <p>Governing jurisdiction. Dispute resolution (arbitration / venue / class-action waiver if applicable).</p>

    <h2>11. Changes</h2>
    <p>How material changes are notified.</p>

    <h2>12. Contact</h2>
    <p>How to reach legal at <a href="mailto:{{org.legal_email}}">{{org.legal_email}}</a>.</p>
  </main>

  <footer class="cc-section cc-footer" data-pattern="footer" data-variant="minimal">
    <div class="container flex-between">
      <span class="cc-footer__copyright voice-swiss --label">{{org.name}} · {{seo.year}}</span>
      <nav class="cc-footer__nav">
        <a class="cc-footer__link voice-swiss --label" href="/">Home</a>
        <a class="cc-footer__link voice-swiss --label" href="/privacy">Privacy</a>
      </nav>
    </div>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Commit**

```bash
cd /Users/nayslayer/.claude/skills/controlledchaos
git add templates/terms.html
git commit -m "Add templates/terms.html — editorial terms-of-service scaffold

12-section structure mirroring privacy.html. Same <link> chain.
Brand replaces placeholder text with real terms.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

End of Part 2. Continue to [part-3-sections-conversion.md](2026-05-20-part-3-sections-conversion.md).
