# Blog patterns research — Ramp, Cursor, Retool

**Date:** 2026-05-11
**Goal:** ground a restyle of `/resources` (currently aeonik + brandYellow + dark cards) into the new editorial style established on `/mission` and `/about` (Instrument Serif, IBM Plex Sans, AnimatedHighlight, 674px column, soft black/70).
**Method:** captured 11 full-page screenshots via headless chromium across 3 reference sites (index + at least one post each, plus Cursor's changelog).

References live in `./references/`. Each insight below cites the file you can open to verify.

---

## What each reference does well

### Cursor — `cursor-blog-index.png`, `cursor-post-third-era.png`, `cursor-post-cursor-3.png`
The closest fit to what we want. Single source of truth for "editorial" on a product site.

- **Index**: 4-up featured grid at top (each card = cover image + date + author + title + short excerpt). Below: a flat, dense, *tabular* list of every other post — date column on the left, title column, author column. No pagination — search box instead. Filter chips: Engineering / Research / Product / Ideas. After the post list: customer stories logo strip, press tiles, videos, then a "Get started" CTA.
- **Post (`/blog/third-era`)**: Topic chip ("Blog / Ideas") + dateline + bold title in a sans display face. Author + read time in a single row under the title. Cover image full-bleed inside the column. TOC card collapses inline (not a sticky rail). Body is centered, **~720px column**, generous line height, sans throughout. **Drop cap on the first paragraph** ("W"). Inline charts/screenshots with soft frame. End: "Related posts" 2-3 cards in a row.
- **Changelog (`cursor-changelog.png` + `-entry.png`)**: same chrome, but date-prefixed list with one screenshot per entry. Each entry has its own anchor URL but they all live on one scrollable page. Individual entry pages exist (`/changelog/05-11-26`) for sharing — same layout as a blog post.
- **Visuals:** very neutral palette. Light gray surfaces, near-black text, only product UI mocks bring color. No yellow accent. No harsh borders — surfaces are differentiated by background tint alone.

### Ramp — `ramp-blog-index.png`, `ramp-post-1.png`, `ramp-post-2.png`
The "magazine" extreme. Most signal for **index density** and **sticky reader chrome**.

- **Index**: one large hero post at top, then *eleven* category clusters down the page (Ramp news, Business credit cards, Expense management, Alternatives, AR, Accounting, Procurement, Treasury, Recently published). Each cluster is a row of 3 small cards with cover image + 1-line title + tiny meta. Reads like a print newspaper section page.
- **Post**: sticky **left rail** with TOC + author card + share buttons. Wide center column. Ramp keeps the body in a sans serif but uses bold callouts and yellow CTA cards inline. "Don't miss these" related row at the bottom.
- **Takeaway:** the sticky TOC is the move for long posts. Ramp's category clustering is *too dense* for us — we have one post.

### Retool / cache. — `retool-blog-index.png`, `retool-post-1.png`, `retool-post-2.png`
Strong opinion. Dark theme. Almost too strong — useful as a foil.

- **Index**: nav-as-section-chooser (BUILD & LEARN, READS & REPORTS, SHOP TALK, RELEASES, NEWSROOM). Mixed-size tile grid feels Bloomberg-ish. Yellow accents (we already have something similar — brandYellow).
- **Post**: sticky left TOC, wide column, dark background, white text, yellow links. Each post is filed under a "collection" chip at the top.
- **Takeaway:** the *only* thing worth borrowing here is "filed under [collection]" chip + cover positioned full-bleed at top. Everything else (dark bg, dense nav-as-sections) clashes with the mission/about direction.

---

## Common patterns worth adopting

| Pattern | Cursor | Ramp | Retool | Adopt? |
|---|---|---|---|---|
| Category/topic chip above title | ✓ | — | ✓ | **yes** |
| Author photo + name + date + read time row | ✓ | ✓ | ✓ | **yes** |
| Cover image full-width inside reading column | ✓ | ✓ | ✓ | **yes** |
| Narrow centered column (≤ 720px) | ✓ | — | — | **yes** — match mission's 674px |
| Drop-cap first paragraph | ✓ | — | — | **yes, subtle** |
| Sticky left rail (TOC + author) | — | ✓ | ✓ | **defer** — only 1 post, low payoff. Inline TOC instead. |
| Related posts grid at footer | ✓ | ✓ | ✓ | **yes** |
| Category filter chips on index | ✓ | — | ✓ | **yes** (we already have categories) |
| Dense cluster-by-topic index | — | ✓ | — | no — overkill for our volume |
| Tabular post list under featured | ✓ | — | — | **yes, eventually** — once we have >6 posts |
| Search input on index | ✓ | — | — | defer |
| Dark background | — | — | ✓ | no — mission/about are soft, want continuity |

---

## Recommended scaffold

Treat `/resources` as an editorial column. Three intents:

1. **Match the new editorial language.** Instrument Serif headings, IBM Plex Sans body, `text-black/70 dark:text-light-80`, AnimatedHighlight for inline emphasis, SectionDivider gradient bars between major shifts. No `brandYellow` chrome — use `highlight-cyan` / `highlight-pink` / `highlight-gold` accents already in the design system.
2. **Keep authoring dead-simple.** A new post is still: drop an `.mdx` into `src/content/blog/`, fill frontmatter, done. The MDX components map handles styling. Authors can opt-in to `<Highlight variant="cyan">term</Highlight>` for inline emphasis inside MDX since `next-mdx-remote` supports JSX-in-MDX.
3. **Plan for growth without over-engineering today.** Featured 2-up grid + all-posts grid below is enough until we hit ~8 posts. Then we can introduce Cursor's tabular fallback.

### Component-by-component changes

| Component | Today | Target |
|---|---|---|
| `BlogHero` | aeonik "Resources" | Instrument Serif `Field notes` (italic emphasis like mission). Sub-line in Instrument Sans. SectionDivider underneath. |
| `BlogCategories` | grid of dark cards, no filter UI | Real filter chip row at top (`All`, `Product`, `AI`, `Student`, `Engineering`…), plus the grid. Restyled cards. |
| `BlogPostCard` | dark `bg-light-5` border-`light-10` rounded card, hover scales image, font-aeonik | Image on top (no chrome around it, just `rounded-lg`), category chip + read time row, Instrument Serif title, IBM Plex Sans excerpt, author + relative date row. Hover: subtle title color shift to `highlight-cyan` (or gold in dark mode), image opacity, no scale. |
| `BlogPostHeader` | aeonik title, brandYellow chip outline | Centered. Category chip with subtle border. Instrument Serif title (clamp 32–56px). Instrument Sans excerpt. Author row centered too — photo + name + date + read time, all small caps `text-black/50 dark:text-white/50`. |
| `BlogPostImage` | bare `rounded-lg` image | Same, but wrapped to 674px column on small screens / 900px on lg. Optional caption support if frontmatter has `coverCaption`. |
| `BlogPostContent` | aeonik headings, brandYellow code/links/blockquote, dark `prose-invert` | Rewrite MDX map. H1/H2/H3 Instrument Serif (italic allowed). Body IBM Plex Sans, `text-black/70 dark:text-light-80`, `leading-relaxed`. Blockquote: pink/cyan gradient left bar (matches SectionDivider). Code: `bg-black/5 dark:bg-white/10`, no yellow. Links: `highlight-cyan` underline-on-hover. First-paragraph drop cap via `:first-of-type::first-letter` CSS. Images: no harsh border, soft drop shadow. Tables: minimal, no inner borders, only top/bottom rule. |
| `BlogPostFooter` | tags + share/bookmark fake buttons + "All Resources" + "Download" | Tags row only (chip style matching the index filters). Single back link "← Back to field notes". 2-up related posts grid (auto-derived from same category, fallback to most recent). Subtle hr divider, no `bg-light-5` box. |

### New: inline `Highlight` in MDX

Expose `AnimatedHighlight` (already imported in mission) inside `BlogPostContent`'s MDXComponents map as a named import. Authors write:

```mdx
We need to <Highlight variant="cyan">quiet the noise</Highlight> and get back our time.
```

No code changes needed in the renderer beyond adding `Highlight: AnimatedHighlight` to the components map.

### Frontmatter (no change needed)

The current schema is already good. Just refine defaults in `src/lib/blog.ts`:

```yaml
---
title: "..."
excerpt: "..."          # used on cards + meta description
date: "2026-05-11"
author: "Vasudev Menon"
authorImage: "..."      # optional, falls back to initial
category: "product"     # one of: product, ai, engineering, student, company
tags: ["..."]
coverImage: "..."       # optional
featured: false         # opt-in to the 2-up hero grid
readTime: 6             # optional, auto-calculated
---
```

### Index page composition

```
BlogHero (editorial title)
  ↓
SectionDivider (pink)
  ↓
[Featured row]  — up to 2 cards, larger, side-by-side. Hidden if no featured posts.
  ↓
[Filter chips] — All / Product / AI / Engineering / Student / Company
  ↓
[All-posts grid] — 3-col on lg, 2-col md, 1-col sm
  ↓
SectionDivider (cyan)
  ↓
[Newsletter / "Get the newsletter" CTA] — optional, can skip for v1
```

### Post page composition

```
Breadcrumb (Home › Resources › Title) — keep current
  ↓
BlogPostHeader (chip, title, excerpt, author row) — centered, 674px max
  ↓
BlogPostImage (rounded, soft shadow)
  ↓
BlogPostContent (mdx, 674px column, drop cap on first <p>)
  ↓
SectionDivider (cyan)
  ↓
BlogPostFooter (tags, back link, related 2-up)
```

---

## Open questions before implementation

1. **Naming**: keep "Resources" or rebrand to "Field notes" / "Journal" / "Dispatches"? The mission page sets a *voice* (we, ephemeral knowledge, coordination tax); "Resources" feels generic next to that. **Recommendation:** "Field notes" — short, editorial, matches voice. Marketing call.
2. **Category taxonomy**: today the one post is `student`. Real categories will probably be `product`, `engineering`, `ai`, `company`. Worth defining now before more posts are written. **Recommendation:** rename `student` → keep as a topic tag, set category to `ai` for the existing post; introduce `product`, `engineering`, `company` as new options.
3. **Featured flag**: nothing is currently featured. Without features the index becomes a flat grid — fine. Want to nominate the existing post as featured for visual interest, or leave for the next post?
4. **Newsletter capture**: any newsletter we'd send people to? If yes, footer block; if not, skip.
5. **Existing post**: the AI homework one is heavy SEO content, *not* editorial. Restyling will improve it but it'll still feel out-of-place. Want to add the new editorial-tone post alongside it as the new "default" reading experience?

---

## Proposed new post

Suggested topic for the example post (please redirect if you have something specific in mind):

- **Title:** *Notes from the coordination tax* — short reflective post on a small product behavior. ~600-900 words.
- **Category:** `product`
- **Featured:** true (so the redesigned hero has something to show)
- **Author:** TBD — Vasudev / Pim / someone on the team
- **Voice:** mirrors the mission page (short paragraphs, AnimatedHighlight on 2-3 phrases, one section divider, no SEO-flavor tables).

Will draft once you greenlight the topic/author. **Holding off on code until approval.**

---

## Files captured

| File | What it shows |
|---|---|
| `references/ramp-blog-index.png` | Ramp blog index — dense category clusters |
| `references/ramp-post-1.png` | Ramp essay — "Build for the long run" |
| `references/ramp-post-2.png` | Ramp announcement — "Midday joining Ramp" |
| `references/cursor-blog-index.png` | Cursor blog index — featured grid + tabular list |
| `references/cursor-post-third-era.png` | Cursor essay — "The third era of AI software" |
| `references/cursor-post-cursor-3.png` | Cursor product post — "Meet the new Cursor" |
| `references/cursor-changelog.png` | Cursor changelog index — date-prefixed list |
| `references/cursor-changelog-entry.png` | Cursor changelog single entry — same chrome as blog |
| `references/retool-blog-index.png` | Retool cache. index — magazine grid, dark |
| `references/retool-post-1.png` | Retool essay — "Code is free, now what?" |
| `references/retool-post-2.png` | Retool essay — "Engineering mindset onboarding" |
