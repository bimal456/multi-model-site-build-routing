---
name: multi-model-site-build-routing
description: "Use when building a site with fixed multi-model routing."
version: 1.0.0
license: MIT
author: kushalarora
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [model-routing, multi-model, workflow, web-build, content]
---

# Multi-Model Site Build Routing

## When to Use

- User asks to build a site, product, or shipping pipeline and references the fixed routing (Claude/GLM/GPT/Gemini, by model name).
- User asks for a detailed scope of work, a content plan, or a QA audit and expects a specific model assigned per job.
- Any task where "which model does this stage?" has a standing answer - invoke the designated model, don't re-derive.

Fixed model assignment for any site/product build and shipping pipeline. Each stage has one designated model; the routing is a standing user rule and is NOT negotiable.

## Model Routing (fixed - user standing rule)

| Stage / Job | Designated model |
|---|---|
| Site structure & planning | **Claude Opus 5.5** |
| Content writing | **GLM 5.3** |
| Site build + technical/on-page SEO audit (Ahrefs) | **GPT-6.1 Sol** |
| Logo, favicon, text banners | **GPT Image 2.5 Flare** |
| Other site images | **Nano Banana 2** |
| Bulk geo variants | **Gemini 3.8 Flash** |

## Rules

1. **Assign each job to its designated model. Never let a non-designated model do another model's job** (e.g. structure/planning only by Claude Opus 5.5, content only by GLM 5.3, build + SEO audit only by GPT-6.1 Sol).
2. **If a designated model is unavailable, ASK the user before swapping.** Never substitute silently and never just skip the stage. Ask, propose the swap, wait for confirmation.
3. **Images split by type:** GPT Image 2.5 Flare handles only logo, favicon and text banners; Nano Banana 2 handles all other (non-logo, non-banner) site images. Do not collapse these into one.
4. **The Ahrefs on-page/technical SEO audit is part of the build stage** (GPT-6.1 Sol), not a separate job.
5. **Geo variants are batched to Gemini 3.8 Flash** ("bulk geo variants" = the bulk pass over all locales).

## Unavailability handling (step-by-step)

1. Detect the unavailable model (API error, quota, capability gap).
2. Stop - do NOT pick a fallback model yourself.
3. Ask the user: state which model is unavailable, which stage it owns, and propose a specific substitute.
4. Proceed only after the user picks or approves the swap.

## Master Prompt (full SpinGazette spec)

The routing one-liner and the complete master prompt below are the authoritative, copy-anywhere spec. Fill the `[GEO INPUTS]` at the top, hand it to the builder, do not skip any section. The routing table + rules + run order earlier in this skill are a structured summary of this same spec - keep them in sync if this section changes.

### Routing one-liner

> Use Claude Opus 5.5 for site structure and planning, GLM 5.3 for content writing, GPT-6.1 Sol for site build and technical/on-page SEO audit with Ahrefs, GPT Image 2.5 Flare for logo, favicon and text banners, Nano Banana 2 for other site images, and Gemini 3.8 Flash for bulk geo variants; ask me before swapping any model that is unavailable.

### MASTER PROMPT — full spec (verbatim)

```text
# MASTER PROMPT — Multi-Geo Casino Review/Affiliate Site (SpinGazette blueprint)

Copy this whole prompt, fill the [GEO INPUTS] at the top, and give it to the builder. It reproduces the exact site model, design, content depth, compliance and technical setup used for SpinGazette.com (NL), adaptable to any country. Do not skip any section.

---

## 0) GEO INPUTS (fill these first)

- **Target country / market:** [e.g. Germany]
- **Language (content language, all copy in this):** [e.g. German / Deutsch]
- **Currency + format:** [e.g. EUR, "€10", comma decimals "€0,20"]
- **Brand name + domain:** [e.g. SpinGazette.de or a fresh brandable .de/.com]
- **Primary regulator (authority):** [e.g. GGL / Gemeinsame Glücksspielbehörde]
- **Self-exclusion register (the local "Cruks" equivalent):** [e.g. OASIS] — reference as PLAIN TEXT, never link to the official register
- **Responsible-gambling helplines (official, for footer, nofollow):** [e.g. BZgA / check-dein-spiel.de, Spielsucht hotline]
- **Tax authority (only if a tax/kansspelbelasting-type guide is built):** [e.g. Bundeszentralamt für Steuern]
- **Legal age:** [e.g. 18+]
- **Licensed operators/brands to review (real, currently licensed in this geo):** [list 15-30, e.g. …] — if unknown, research first
- **Head keywords (commercial + informational):** [e.g. "online casino Deutschland", "beste online casino", "casino ohne OASIS", …] — confirm via research
- **Real popular slots for this market (high search volume):** [e.g. Gates of Olympus, Book of Dead, Sweet Bonanza, …]

> Rule: NEVER mention the agency/company name anywhere on the site.

---

## 1) OBJECTIVE & SITE TYPE

Build a **static HTML** casino review + affiliate site for the target geo: an editorial, brand-like "gazette" that tests and ranks **locally licensed** online casinos, plus supporting guides, payment-method pages, and slot reviews. Independent, honest, E-E-A-T-driven tone. Primary geo lives at the **root** (`/`); future geos go in subfolders (`/de/`, `/it/`) with hreflang.

---

## 2) SERP-FIRST (MANDATORY, do this before writing ANY page)

For every page, FIRST check the pages currently ranking in that geo for the target keyword. Match the **content type, structure, subheadings and search intent** Google is rewarding. Examples:
- Commercial keyword (e.g. "casino zonder cruks / casino ohne OASIS") -> listicle/comparison **hub**: ranked table + CTAs + per-item profiles, NOT an essay.
- Slot keyword (e.g. "gates of olympus") -> **slot review**: hero image, quick-facts panel, how-to-play, features, RTP/volatility, where-to-play, FAQ.
- Do not invent structure; mirror what ranks, then write better/humanised.
- **Word count is NOT fixed - the SERP sets it.** Check the length/depth of the top 3-5 ranking pages for that keyword and match or slightly exceed it. If they run 2,500+ words, go longer; if a query is served by short pages, do not pad. Depth follows intent, per page, per geo.
- **Voice & tone follow the competitors too.** Read how the top-ranking pages for that keyword speak (register, formality, first-person vs neutral, use of opinions, local idiom) and match that voice - while keeping it humanised and original to the brand, never copied. Tone can differ per market and per page type.

---

## 3) SITE ARCHITECTURE & URL STRUCTURE

Root = primary geo. Clean folder URLs with trailing slash.

- `/` — homepage
- `/casinos/` — casino ranking hub (comparison table + per-brand profiles)
- `/casinos/<brand>/` — individual brand review (one per licensed operator)
- `/casinos/<brand>/bonus/` — brand bonus sub-page (for major brands)
- `/spellen|/games/` — games hub  ·  `/…/<game-guide>/` (blackjack, roulette, live casino, slots/gokkasten)  ·  `/…/<real-slot>/` (slot reviews)
- `/bonus/` — bonus hub  ·  `/gidsen|/guides/` — guides hub + individual guides (bonus explained, no-deposit, what-is-[register], gambling-tax)
- `/betaalmethoden|/payments/` — payments hub + method pages (Paysafecard, Pay N Play, etc.)
- `/[casino-without-register]/` — the geo's "zonder Cruks / ohne OASIS" comparison hub (offshore/non-licensed angle, framed as background NOT recommendation)
- `/crypto-casino/` — crypto comparison  ·  `/[casino-without-registration]/` — no-registration hub
- `/auteurs|/authors/<name>/` — author page (E-E-A-T)
- Utility (footer only, NOT header): `/over-ons|about`, `/contact`, `/verantwoord-spelen|responsible-gambling`, `/voorwaarden|terms`, `/privacy` — deliver as real files with full content
- Technical: `robots.txt`, `sitemap.xml` (ALL URLs — no page missing), `404.html`, `.htaccess`

hreflang: self + x-default; when multiple geos exist, cross-reference. Canonical: non-www HTTPS, self-referential.

---

## 4) DESIGN SYSTEM (unique, brand-like, NOT AI-looking)

- **Theme:** dark premium (deep navy/blue + gold accent). Fonts: **Oswald** (headings/labels, uppercase) + **Inter** (body). Define CSS color tokens on `:root`.
- **Header:** logo + **dropdown mega-menu** nav (same on homepage AND inner pages — do not ship a simpler homepage nav) + **Login** and **Register** buttons (both -> `/play`). Utility pages NOT in header. Mobile hamburger that actually toggles the menu open.
- **Homepage sections:** hero (headline + hero image + "editor's choice" brand card with score) + trust stats + **top casino ranking** (cards with real logos, score, editorial one-liner, varied CTA) + **best bonuses** + **popular games** (clickable tiles linking to slot reviews) + **guides** section + footer.
- **Casino hub:** intro + **comparison table** (rank, logo, score, "what stands out", welcome bonus, CTA "Speel nu / Bezoeken" + Review link) + per-brand profiles.
- **Slot/brand review pages:** hero image (top) + **score box** (overall X/10 + 3 sub-score bars) + **quick-facts panel** (icon-style grid: provider, release, layout, RTP, volatility, min/max bet, max win, features, autoplay, jackpot) + prominent **CTA near top** (after intro).
- **Reusable blocks:** styled specs grid, pros/cons two-column box, numbered FAQ (badge counters), "similar items" grid with mini-specs, provider info box, disclosure bar.
- **Avoid AI-look:** no templated identical per-item scaffolding, no badge/pill/checkmark overload, no accordion-FAQ overuse, no letter-badge placeholder logos. Vary rhythm, add opinions and lived detail.

---

## 5) CONTENT RULES

- Language = geo language, throughout. **No em dashes** (hyphens fine).
- Humanised, editorial, opinionated; varied sentence rhythm; real specifics. Never reads AI-generated.
- **Voice & tone are set by the SERP, not by a default.** Study the top-ranking pages for each keyword and match their register/tone for that market and page type (formal vs casual, neutral vs first-person, level of opinion, local idiom) - staying original to the brand, never copied.
- **E-E-A-T:** named author with author page, byline (photo + "By [Name], editor-in-chief - Updated [date]"), reviewer line, Person + Organization schema.
- **Real facts only.** RTP as "around X%" (note operators vary). NO fake/invented bonus amounts; describe bonus type generically if exact terms unknown ("welcome bonus", "match + free spins").
- Use every word of any supplied content; build page by page, deliver together.
- **Word count is flexible and SERP-driven, not a fixed target.** As a baseline slot/brand reviews land ~1500-2000 words and hubs match listicle depth, but ALWAYS size each page to match or slightly beat the top-ranking pages for that keyword - increase where they are longer, do not pad where the intent is short.

---

## 6) CTA & LINK RULES (strict)

- Primary CTA = **Login/Register/"Speel nu"/"Bezoeken"** -> `/play` (affiliate redirect placeholder; keep placeholder in place).
- **Vary anchor text** (not always "Play Now"): soft anchors ("Check here", "Bekijk aanbod", "Naar [brand]").
- CTA placement: one **near the top** (after intro) AND one mid/lower. Buttons must be **large and prominent** (full-width in table action cells), not tiny.
- **Every CTA and every external link = `rel="nofollow"`.** Only internal main-site links are followed.
- **No irrelevant/spam external links.** The ONLY external links allowed: official regulator, tax authority (only on the tax guide), and responsible-gambling helplines (footer) — all nofollow.
- Reference the self-exclusion register as **plain text** (no link to it).
- Responsible-gambling page must have **NO play CTA** (no "Speel nu"/Login-to-play button in its content).

---

## 7) RESPONSIBLE GAMBLING & COMPLIANCE

- **No age-gate / 18+ popup** on any page. Instead: small **[age]+ badge in the footer** + one **"[age]+ only" disclosure line** on the homepage.
- Footer "Play responsibly" column: geo's official RG helplines (nofollow).
- Geo-appropriate legal framing: distinguish locally licensed (safe) vs offshore/non-registered; on the "without register" hub, frame as background info, not a recommendation, and score lower with a clear disclaimer.
- Affiliate disclosure line in footer/disclosure bar.

---

## 8) SLOT REVIEW PAGE SPEC (per real slot)

Pick REAL high-volume slots for the geo (Gates of Olympus, Book of Dead, Sweet Bonanza, Big Bass Bonanza, Starburst, etc.). Each page, in order:
  [SPEC GAP: source paste truncates here. Confirm the rest of section 8 + any later sections with the user before building slot pages; do not invent the missing spec.]
```

### Truncation note (from source)

The pasted master prompt ends mid-way through **section 8 (Slot Review Page Spec)**, at "Each page, in order:". The rest of section 8 and any sections after it (likely technical SEO/schema, deliverables, QA) are missing. Before building slot pages, ask the user for the remainder - do not reconstruct or invent the missing spec. This matches the same gap already flagged in the `multi-geo-casino-affiliate-site` skill.

## Run order (recommended)

1. **Claude Opus 5.5** - sitemap, IA, page structure, content plan (the blueprint everything else follows).
2. **GLM 5.3** - all written copy per the content plan (voice, tone, SEO-focused).
3. **GPT Image 2.5 Flare** - brand assets: logo, favicon, text banners.
4. **Nano Banana 2** - remaining site imagery (heroes, sections, article/listing images).
5. **GPT-6.1 Sol** - static build/assembly, on-page + technical SEO, Ahrefs audit, fixes.
6. **Gemini 3.8 Flash** - bulk geo/localization variants across locales (after the primary geo is stable).

## Notes
- This skill is model-vendor agnostic about *how* models are invoked (API, CLI delegate, provider router) - keep the routing table the single source of truth and let the surrounding pipeline handle transport.
- If the routing table ever changes, update the table in the skill and keep each stage's scope description in sync.
