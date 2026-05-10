# Chika Chika Cool — Website Source

Marketing copy, HTML reference implementation, and image asset specifications for the Chika Chika Cool product website (chikachikacool.com / wekvnz.mimipet1233.workers.dev).

**Maintained by:** MIMIPET Co., Ltd.

---

## 📁 What's in this repo

```
.
├── content.json     # Single source of truth — all website copy, structured by section
├── index.html       # Reference implementation — vanilla HTML/CSS rendering all sections
├── styles.css       # Stylesheet matching the brand's earth-tone palette
├── README.md        # This file
└── assets/          # Images (see "Image asset specifications" below)
```

---

## 🚀 Quick start

### Option A — Use as-is (vanilla HTML)

```bash
# Just open it
open index.html
# Or serve locally
python3 -m http.server 8000
```

Drop the `index.html`, `styles.css`, and `assets/` folder onto any static host (Cloudflare Pages, GitHub Pages, Netlify, Vercel, S3). Done.

### Option B — Use `content.json` with your existing framework

The current site at `wekvnz.mimipet1233.workers.dev` runs on Cloudflare Workers. If your codebase uses a templating engine, framework (React, Vue, Svelte, Astro), or static site generator, **import `content.json` and render from it**. That way, when marketing wants to change copy, they edit the JSON — not the code.

---

## 🧱 The single-source-of-truth model

The most important file in this repo is **`content.json`**. Every piece of copy on the website lives there. The `index.html` is a *reference* that shows how it should render — but in production, your code should render *from the JSON*, not duplicate the strings.

### Why?

- **Marketing edits stay out of code review.** A copy change is a one-line JSON edit, not a PR full of HTML diffs.
- **Translation-ready.** When you launch the Korean or Arabic version, copy `content.json` to `content.ko.json` / `content.ar.json` — same shape, different strings.
- **Less drift.** If the same tagline appears in the hero AND meta tags, you don't update it in two places.

### Example consumption (React)

```jsx
import content from './content.json';

export default function Hero() {
  const { headline, subheadline, primaryCta, secondaryCta, image, trustStrip } = content.hero;
  return (
    <section id={content.hero.id} className="hero">
      <h1>{headline}</h1>
      <p>{subheadline}</p>
      <a href={primaryCta.href} className="btn btn-primary">{primaryCta.label}</a>
      <a href={secondaryCta.href} className="btn btn-ghost">{secondaryCta.label}</a>
      <img src={image.src} alt={image.alt} />
      <div className="trust-strip">
        {trustStrip.map((item, i) => <div key={i}>{item.label}</div>)}
      </div>
    </section>
  );
}
```

### Example consumption (Cloudflare Worker, server-side render)

```js
import content from './content.json';

export default {
  async fetch(request) {
    const html = `
      <h1>${content.hero.headline}</h1>
      <p>${content.hero.subheadline}</p>
      ${content.faq.items.map(({ q, a }) =>
        `<details><summary>${q}</summary><p>${a}</p></details>`
      ).join('')}
    `;
    return new Response(html, { headers: { 'content-type': 'text/html' } });
  }
};
```

---

## 📐 Section reference

The site has **13 sections** in this order. Each has a corresponding key in `content.json`.

| # | Section | JSON key | Anchor | Notes |
|---|---------|----------|--------|-------|
| 1 | Hero | `hero` | `#hero` | Headline + subheadline + 2 CTAs + trust strip |
| 2 | Brand heritage | `heritage` | `#heritage` | Story of the human-patent origin |
| 3 | Why Chika Chika Cool | `why` | `#why` | 4 differentiator cards |
| 4 | Product lineup | `products` | `#products` | 100ml + 1L, side-by-side |
| 5 | Seven herbs | `ingredients` | `#ingredients` | 7-card herb grid |
| 6 | The science | `science` | `#science` | 12-hr extraction + 3-step process |
| 7 | Care performance | `performance` | `#performance` | Radar chart (5 axes) — *not in reference HTML, see below* |
| 8 | Trust by numbers | `trust` | `#trust` | 5 stat cards |
| 9 | International track record | `globalPresence` | `#global-presence` | Country grid: 6 pet markets + UAE camel supply |
| 10 | How to use | `howToUse` | `#how-to-use` | 4-step application guide |
| 11 | Use cases | `useCases` | `#use-cases` | Tabbed: pets / equine / camel |
| 12 | FAQ | `faq` | `#faq` | 10 expandable questions |
| 13 | Contact | `contact` | `#contact` | Retail + B2B + form |

### Note on the radar chart (section 7)

The `performance` data in `content.json` is structured for a 5-axis radar chart (the same chart from the existing product catalog, page 3). The reference `index.html` does *not* render this — radar charts need a chart library. Recommended options:

- **Vanilla / lightweight:** [Chart.js](https://www.chartjs.org/docs/latest/charts/radar.html) — `<canvas>`-based, ~70KB
- **React:** [Recharts](https://recharts.org/en-US/api/RadarChart) — already a common React choice
- **No dependency:** Hand-roll an SVG radar chart from the 5 values in `content.performance.axes`. Probably 50 lines of code and stays crisp on all screens.

---

## 🖼 Image asset specifications

All images go in `/assets/`. Filenames in `content.json` reference these paths. The image quality of your existing catalog photography is **excellent** — reuse those photos directly. Avoid AI-generated or generic stock.

### Required assets

| Filename | Section | Source recommendation | Spec |
|----------|---------|----------------------|------|
| `hero-split.jpg` | Hero | Composite the dog photo + camel photo from your two catalogs | 1600×900, JPG ≤200KB |
| `og-image.jpg` | Meta | Same composite, tighter crop | 1200×630, JPG |
| `heritage-extraction.jpg` | Heritage | Extraction shot, herb still life, or warm lab photo | 1200×800, JPG |
| `product-100ml.jpg` | Products | Green/red label bottle pair from pet catalog page 1 | 1200×900, JPG |
| `product-1l.jpg` | Products | 1L bottle lineup from large-animal catalog page 1 | 1200×900, JPG |
| `herb-clove.jpg` | Ingredients | From pet catalog page 2 (the styled herb-on-wood photos) | 800×800, JPG |
| `herb-licorice.jpg` | Ingredients | From pet catalog page 2 | 800×800, JPG |
| `herb-elm.jpg` | Ingredients | New — slippery elm bark (or substitute herb image) | 800×800, JPG |
| `herb-cnidium.jpg` | Ingredients | From pet catalog page 2 (cnidium flower) | 800×800, JPG |
| `herb-mint.jpg` | Ingredients | From pet catalog page 2 (mint leaves) | 800×800, JPG |
| `herb-prickly-ash.jpg` | Ingredients | From pet catalog page 2 (prickly ash berry) | 800×800, JPG |
| `herb-grapefruit.jpg` | Ingredients | From pet catalog page 2 (grapefruit seed) | 800×800, JPG |
| `howto-1-familiarize.jpg` | How to use | From pet catalog page 4 (dog sniffing bottle) | 800×600, JPG |
| `howto-2-water.jpg` | How to use | From pet catalog page 4 (chihuahua at water bowl) | 800×600, JPG |
| `howto-3-spray.jpg` | How to use | From pet catalog page 4 (chocolate lab) | 800×600, JPG |
| `howto-4-sensitive.jpg` | How to use | From pet catalog page 4 (girl with calm dog) | 800×600, JPG |
| `usecase-pets.jpg` | Use cases | Bichon photo from pet catalog | 1200×900, JPG |
| `usecase-equine.jpg` | Use cases | Horse photo from large-animal catalog | 1200×900, JPG |
| `usecase-camel.jpg` | Use cases | Racing camel photo from large-animal catalog | 1200×900, JPG |

### Image optimization

Before committing, run images through:

```bash
# Install once
brew install imagemagick   # macOS
# or: apt install imagemagick

# Resize + compress
mogrify -resize 1600x -quality 82 -strip *.jpg

# Generate WebP versions (optional but recommended — ~30% smaller)
for f in *.jpg; do cwebp -q 82 "$f" -o "${f%.jpg}.webp"; done
```

For Cloudflare Workers / Pages, you can also rely on Cloudflare Image Resizing in production and just upload originals.

---

## 🎨 Design tokens

CSS variables are defined in `styles.css` and match the brand's catalog palette. If you're rebuilding in Tailwind or another framework, port these:

```css
--color-olive:        #6b7444;   /* primary brand */
--color-olive-dark:   #4a5230;
--color-olive-darker: #2c3e2d;   /* dark backgrounds, headers */
--color-leaf:         #8aa56c;   /* accent / highlights */
--color-cream:        #f7f4ec;   /* primary surface */
--color-cream-warm:   #f1ede1;
--color-paper:        #fbf9f3;   /* page background */
--color-text:         #2c2c2a;
--color-text-soft:    #5f5e5a;
--color-text-muted:   #888780;
--color-border:       #d8d4c5;
--color-accent:       #b8763a;   /* warm accent / hover */
```

**Typography:**
- Display / headings — serif (recommend Playfair Display or Cormorant Garamond)
- Body — system sans-serif stack

**Don't deviate from this palette.** It's already working on the existing site and matches the catalog photography.

---

## 🌐 Internationalization (when ready)

Adding Korean or Arabic later:

1. Copy `content.json` → `content.ko.json`, translate the strings (keep keys identical)
2. Add a locale switcher to the nav
3. At render time, load the appropriate JSON based on URL prefix (`/ko/`, `/ar/`) or a cookie

The shape of the data is identical across languages, so no template changes are needed.

---

## 🔍 SEO checklist

The `content.meta` block has the title and description. When you wire up SSR or a build step, make sure these end up in:

- `<title>` tag
- `<meta name="description">`
- `<meta property="og:title">` / `og:description` / `og:image`
- `<meta name="twitter:card" content="summary_large_image">`

Add a `sitemap.xml` and `robots.txt` at the root. Submit the sitemap to Google Search Console once live.

---

## ✅ Pre-launch checklist

- [ ] All images in `/assets/` are present and optimized (<200KB each, WebP fallbacks)
- [ ] `content.json` has been proofread (no `[placeholder]` text remaining)
- [ ] Naver Smart Store link in `products.items[0].buyHref` points to the live product page
- [ ] Instagram link is correct (`https://www.instagram.com/chikachikacool/`)
- [ ] Email addresses (`contact@`, `sales@`) are set up and monitored
- [ ] Phone number (`+82-10-4043-9775`) is correct
- [ ] Contact form `<form>` submits to a real endpoint (email, Formspree, or Worker handler)
- [ ] Mobile layout tested at 375px, 768px, and 1024px
- [ ] Lighthouse score: Performance > 85, Accessibility > 95, Best Practices > 90, SEO > 95
- [ ] Open Graph preview tested (paste URL into Slack / iMessage to verify)
- [ ] Privacy policy and terms pages exist (linked from footer)
- [ ] Analytics installed (Cloudflare Web Analytics is free and privacy-friendly)

---

## 📞 Questions

- Marketing / copy: sales@mimipet.co.kr
- Technical: whoever owns the GitHub repo

---

*Generated for the Chika Chika Cool website refresh. All product claims and marketing copy are pre-approved by MIMIPET Co., Ltd.*
