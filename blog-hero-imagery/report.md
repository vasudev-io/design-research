# Blog hero imagery patterns

**Date:** 2026-05-11
**Goal:** decide what (if anything) goes above the title on a Highlight blog post, with a specific recommendation for the new "How to build agentic chat with Durable Objects" post.
**Method:** 14 above-the-fold screenshots from 7 SaaS blogs — Cursor, Ramp, Retool, Vercel, Resend, Linear, Anthropic.

References live in `./references/`.

---

## Two camps

After looking at the captures, every SaaS blog post falls into one of two approaches for its hero:

### Camp A — **No hero image**, the title *is* the hero
Used by: Vercel (every post), Cursor's text-heavy posts when no specific artwork exists.

The post opens with: huge bold title → author byline → straight into copy. On the index, posts get a small monogram glyph (Vercel uses a triangle for product, a `{f}` for engineering, an octagon for press — tiny abstract icons, not photos).

**Why it works:** signals "this is a serious piece of writing". No image-sourcing tax. Index page gets visual rhythm from glyphs, not full art.

**Where it fails:** if the post has weak copy or a generic title, the index looks dry.

### Camp B — **Strong hero**, image carries half the meaning
Used by: Cursor essays, Ramp, Retool/cache, Resend, Linear, Anthropic.

Image sits between title and body, full-width inside the reading column. The kind of image varies wildly:

| Style | Examples | When it fits |
|---|---|---|
| Abstract photo / texture | Cursor "third era" (white wire on dark bg) | thought-leadership essays, no obvious subject |
| Product UI mock (small/dark) | Linear "Now" index — tiny dark cards | product launches, changelog entries |
| Photo of person/team | Cursor 3 (interview), Ramp "midday joining" | announcements, profiles, customer stories |
| Bold typography poster | Resend launch-week (vintage poster) | brand moments, launches |
| Hand-drawn illustration | Anthropic newsroom (head + network) | research, ideas, technical-meets-human |
| Magazine illustration | Retool "Code is free" (3D collage) | provocative essays, opinion |

---

## What's worth borrowing

For Highlight's blog specifically (clean, light, sans, narrow column), the strongest fits are:

1. **Vercel's pure-text approach** — simplest, hardest to get wrong, scales as we add posts. On the index, attach a small monogram or category glyph instead of an image.
2. **Cursor's abstract textures** — used when the post is conceptual and there's no obvious subject. Photographic, often neutral palette, lives well on a light cream background.
3. **Linear's tiny product mocks** — small dark thumbnails of the actual product/diagram, framed in a card. Great for technical posts because the image IS the diagram.

What to **avoid**:
- Retool/cache style (loud magazine art) — clashes with the clean light direction we just shipped.
- Resend's massive type posters — fun but they need a brand-illustration system we don't have.
- Stock photos of laptops, code, abstract tech — every dev blog uses these. They read as filler.

---

## Recommendations for Highlight

**Default policy: Camp A (no hero image)**, with the option to add one when a specific post earns it.

This is the simplest path:
- Most posts go up faster — no image hunting.
- Index page gets rhythm from category chips + author photos already.
- Engineering posts especially don't *need* art at the top.
- Removes the "what if the image is bad" worry.

**When to add a hero image**:
- The post has a real, photogenic subject (a person, an event, a product launch).
- We have a strong custom illustration or diagram already (don't go searching for one).
- It's a launch/announcement and we want the visual punch.

If we do add an image, default to one of two styles:
1. **Custom diagram from the post itself**, framed in a soft card (Linear style). Especially for engineering posts where the architecture *is* the story.
2. **Abstract photo or render** in a neutral palette (Cursor third-era style). Stays calm, doesn't compete with the title.

---

## Specific recommendation: "How to build agentic chat with Durable Objects"

The post is a technical deep-dive with code, a diagram, and an architectural narrative. **Closest analog in the wild: Vercel's "A new programming model for durable execution"** (also durable-execution-themed, also tech-heavy) — and Vercel uses **no hero image at all**. Just title → author → text. See `references/vercel-durable-hero.png`.

That's the strongest signal: for this exact category of post, no image is the industry default and it works.

**Three options, ranked:**

### 1. **No hero image** (my recommendation — easiest, looks intentional)
Just title + author + read time + straight into the post. Already supported by our current `BlogPostImage` component returning `null` when there's no `coverImage` in frontmatter. The post is already wired this way — leave it.

Why this wins: matches Vercel exactly, ships immediately, looks confident. The diagram referenced in the post (Hono → Worker → ChatObject → SQLite) becomes the first image users see, in-context, which is more useful than a decorative cover.

### 2. **Architecture diagram as hero** (best if we have time for one custom asset)
Take the existing structure diagram the post already references (currently a TODO placeholder around line 60) and elevate it: clean it up in Figma, export at 1600×900 with generous white space, soft drop shadow, neutral palette. Frame it in a light gray card like Linear does on its blog cards.

Why: turns the educational asset into the cover. Doubles as the in-post diagram. No extra creative work beyond polishing what we need anyway. Direct and honest — "this is what we built".

Mockup recipe: white/light-gray background → minimal node boxes (Worker, ChatObject, SQLite) → connecting lines with subtle arrows → labels in IBM Plex Sans → no shadows, no color (or a single accent on the data path).

### 3. **Abstract texture/render** (most polish, most time)
A neutral abstract image conveying "durability" or "distributed compute" without being literal. Options:
- **Fiber-optic strands** (long-exposure photo style, dark navy or warm gray bg) — fits "globally distributed".
- **Stacked stone columns / basalt** — literal "durable", but might read too literal.
- **Subtle 3D render of nested boxes** (one large, many small) — visual metaphor for "single Durable Object instance with co-located storage".
- **Cursor "third-era" style** — single thin white wire on a dark muted backdrop, abstract motion.

Why: feels most like a destination editorial site. But requires sourcing or commissioning, and the post ships fine without it.

**My call: ship with option 1 today, replace with option 2 once someone has a free hour in Figma.** Don't block the post on imagery.

---

## Files captured

| File | What it shows |
|---|---|
| `references/vercel-durable-hero.png` | **The reference** — Vercel's "durable execution" post, no hero, our closest analog |
| `references/vercel-agentic-hero.png` | Vercel's "agentic infrastructure" — same treatment, no hero |
| `references/vercel-blog-index.png` | Vercel index — monogram glyphs replace cover images |
| `references/cursor-third-era-hero.png` | Cursor essay — abstract photo on light bg |
| `references/cursor-3-hero.png` | Cursor announcement — person photo as hero |
| `references/cursor-bench-hero.png` | Cursor product post — what their newer treatment looks like |
| `references/ramp-midday-hero.png` | Ramp announcement — full-bleed people photo |
| `references/ramp-trillion-hero.png` | Ramp opinion — magazine-style hero photo |
| `references/retool-code-free-hero.png` | Retool/cache — loud 3D collage style (foil) |
| `references/retool-engineering-hero.png` | Retool engineering post |
| `references/resend-blog-index.png` | Resend — vintage typography posters |
| `references/resend-ai-editor-hero.png` | Resend AI editor launch hero |
| `references/linear-blog-index.png` | Linear "Now" — tiny dark product-mock thumbnails |
| `references/anthropic-news-index.png` | Anthropic newsroom — hand-drawn illustration mosaic |
