---
name: redbus-design-language
description: >-
  Audits and documents the visual and UX design language of redbus.in (colors,
  type, layout, components, patterns, voice) across **all scoped URLs**, including
  bus home, **redRail** (`/railways`), **train search** (inventory + passenger
  steps on one route), and **payment shells** (`/railways/paymentDetails` and
  analogous paths). Use when the user mentions redBus, redbus.in, redRail,
  railways, IRCTC, train booking, design system, tokens, Stitch, DESIGN.md, or
  prototyping in the redBus style. **Default:** generate and audit **mWeb (mobile
  web)** only; **native app** and **desktop** are out of scope unless the user
  explicitly asks for them.
---

# redBus design language

## Purpose

Produce an **evidence-based design-language spec** from [redbus.in](https://www.redbus.in/) so new UI (code, Figma, [Google Stitch](https://stitch.withgoogle.com/), etc.) stays consistent with observable patterns. **Generated UX** defaults to **mWeb only** (mobile browser). See [Viewport default: mWeb](#viewport-default-mweb).

## Before you start

1. **Prefer primary sources:** live site, screenshots the user provides, or inspect CSS in browser DevTools (computed styles, font-family, hex/rgb).
2. **Do not invent tokens:** if a value is unknown, mark `TBD — verify on site` rather than guessing.
3. **Trademarks:** document patterns for **internal/educational** use. For public shipping, the user needs their own brand assets and must not imply endorsement by redBus or MakeMyTrip.

## When invoked

1. Confirm scope: **marketing home only**, **a vertical** (e.g. trains / redRail), **full booking funnel**, or **explicit URL list**.
2. If the user gives **no URLs**, propose a sensible default set: site home + (for trains) the **redRail canonical trio** in [Traverse all pages](#traverse-all-pages-generic-procedure) — still confirm before a long crawl.
3. Ask for **screenshots or URLs** when live fetch is unreliable; offer **Stitch** handoff via **[STITCH.md](STITCH.md)** ([reference.md](reference.md)).

## Traverse all pages (generic procedure)

Use this for **any** redBus surface so **`DESIGN.md` + `STITCH.md`** stay aligned for [Google Stitch](https://stitch.withgoogle.com/) and future screens.

### Viewport default: mWeb

**Unless the user asks otherwise**, all **generated UX** (Stitch, specs, prompts) and **primary evidence** target **mWeb** — the responsive site in a **mobile browser** (~360–430 px width), touch-first, typical mobile nav (hamburger / bottom bar per product). **Do not** produce native app chrome or desktop layouts unless the user explicitly requests **app** and/or **desktop**.

| Target | When to use |
|--------|-------------|
| **mWeb** (default) | Always for audits and Stitch: DevTools device mode or phone browser; Stitch **`MOBILE`** + prompt must say **“mobile web”** / **“mWeb”** — **not** native app. |
| **App** | Only if the user explicitly asks for native iOS/Android UI. |
| **Desktop** | Only if the user explicitly asks for desktop web. |

**Rules:** Tokens (colors, type, radii) stay the same if you later add other viewports; layout rules in `STITCH.md` prioritize **mWeb**. One **mWeb** screen per logical flow step unless the user expands scope.

### 1. Build the URL list

- **User-supplied:** every path the user names (exact query strings included).
- **Discover more:** global header/footer links, vertical switchers (Bus / Train / Hotels), sitemaps, and “obvious” funnel templates (`/search`, `/railways/search`, payment paths — see [reference.md](reference.md)).
- **Normalize:** record full HTTPS URL, page title, and **audit date**.

### 2. Capture evidence per URL

For **each** URL (and each **logical screen** below):

| Method | What to extract |
|--------|------------------|
| HTML + inlined CSS | Unique hex/rgb, `font-face`, radii, shadows, max-width (when fetch works). |
| DevTools | Computed styles on representative nodes (primary button, H1, card, input). |
| Screenshots | **Default:** **mWeb** capture per logical step — label `…-mweb.png` (or `…-mobile-web.png`). Add **app** / **desktop** captures only if the user requests those viewports. |

Merge into one token draft; **dedupe** colors but keep a note if a value is **step-specific**.

### 3. SPA / multi-step routes (same URL, multiple UIs)

Many funnels change **client-side step** without changing the path (e.g. train search: results list → passenger / IRCTC details).

**Rule:** Treat each step as a separate **logical screen** in the spec and evidence table:

- Name them clearly, e.g. `https://www.redbus.in/railways/search?…` **· step: inventory** and **· step: customer_info** (or vendor-neutral: `step: results`, `step: traveler_details`).
- **Do not** claim the design language is complete from a single load or a single screenshot.
- Require **one capture per step** (screenshot set, or DOM/export per step). If the user only provides one, mark the other `TBD — capture after advancing flow`.

### 4. Payment and authenticated shells

Paths like [`/railways/paymentDetails`](https://www.redbus.in/railways/paymentDetails) may return **minimal HTML** until session/booking context exists.

**Generalize** this pattern to **any** payment or checkout URL across products:

- Still **list the URL** in evidence.
- Document **visible chrome** (nav, title, layout shell) from whatever loads without a session.
- Mark payment instruments, price breakdown, and CTAs as **`TBD`** until captured with a real or test booking.
- In Stitch, instruct: “Match global header + train funnel tokens; payment body TBD from reference screenshot.”

### 5. Reference: redRail (trains) — canonical audit URLs

Use when auditing **trains / redRail / IRCTC** (replace `doj` with a valid journey date when reproducing search):

| # | URL | Capture |
|---|-----|--------|
| 1 | `https://www.redbus.in/railways` | Landing: hero (“Train Ticket Booking”), IRCTC partner / trust, promo code, From–To–Date, date chips (e.g. Tomorrow, Day After), Free Cancellation row, **Search Trains** CTA, routes, testimonials, long-form SEO, FAQs, footer. |
| 2 | `https://www.redbus.in/railways/search?src=NDLS&dst=HWH&doj=YYYYMMDD&srcName=Delhi%20-%20All%20Stations&dstName=Kolkata%20-%20All%20Stations&fcOpted=false` | **Two logical screens:** (a) **Inventory** — train list, classes, fares, availability; (b) **Customer info** — IRCTC login, passengers, add-ons. Same path, different step — **two rows** in evidence. |
| 3 | `https://www.redbus.in/railways/paymentDetails` | Payment step template; session-dependent body — follow §4 above. Reuse the **same audit rules** for other verticals’ payment URLs. |

## Audit workflow

Work in this order: foundations → components → patterns → voice.

### 1. Information architecture (from observable chrome)

Capture how the product frames itself (nav, promos, footers). **Bus home** includes bus/train/hotels, bookings/help/account, search-first hero, trust, offers, FAQs, SEO lists. **redRail landing** ([`/railways`](https://www.redbus.in/railways)) mirrors **search-first** layout with **train-specific** copy (IRCTC authorised partner, redRail, Free Cancellation, Search Trains). Use **layout priority**: primary search and conversion above the fold; legal and SEO below.

### 2. Foundations

| Area | Record |
|------|--------|
| Color | Primary brand red, neutrals, surfaces, borders, link color, semantic (success/warning/error) if visible |
| Typography | Families, scale (display/H1/body/small/caption), weights, line-height habits |
| Spacing & grid | Section padding, card gutters, max width, vertical rhythm |
| Shape | Input/button radii, card corners, shadows |
| Iconography | Style (outline/filled), size steps |
| Motion | Transitions used (hover, focus); default to minimal if none observed |

### 3. Components

Inventory recurring UI: **top promo strip**, **header/nav**, **search panel** (fields, chips, primary button), **cards** (offers, deals), **lists** (routes, trains), **data-dense rows** (inventory), **forms** (passenger / payment), **footer** columns, **links** vs buttons. For each: anatomy, default/hover/focus/disabled/loading, **which URL + step** it appears on, and **mWeb-specific** behavior (stacked layout, touch targets, sticky CTAs). Note app/desktop differences **only** if those viewports are in scope.

### 4. UX patterns

- **Primary journey:** search → results (inventory) → traveler / login → pay (adjust per vertical).
- **Density:** transactional, deal-led, high trust signals (IRCTC partner, refunds, testimonials).
- **Forms:** labels, placeholders, validation presentation if seen.

### 5. Content voice

Short, direct, offer-aware copy; feature lists with bold lead-ins; FAQ-style support. **redRail:** IRCTC authority, Free Cancellation / Alternate Trip, step-by-step “how to book.” Paraphrase patterns; do not paste long proprietary FAQ text unless the user needs a quote for analysis.

## Deliverables

Output a single markdown document (or separate files if the user prefers) containing:

1. **Summary** — aesthetic + intent (marketplace + vertical).
2. **Foundations** — token table with name, value, usage (`TBD` where unverified).
3. **Components** — bullet specs per component, tied to **URL + logical step** where relevant.
4. **Patterns** — flows including **explicit SPA steps**.
5. **References** — table: URL, title, logical step (if any), **viewport** (default **`mWeb`**; add `app` / `desktop` only if in scope), screenshot filename or fetch method, date.
6. **Stitch handoff** — pin **[STITCH.md](STITCH.md)** in Stitch (primary); keep [DESIGN.md](DESIGN.md) as the audit source of truth and sync token changes into `STITCH.md` when needed. For generated UX, plan **one mWeb generation per key screen** (`MOBILE` + “mobile web” prompt) unless the user asks for app/desktop too.

## Quality checks

- [ ] Every color/font/spacing claim ties to evidence or is marked TBD.
- [ ] **Every URL** in scope appears in the evidence table; **every SPA step** has its own row or is TBD.
- [ ] Payment/auth shells documented with honest **TBD** where session is required.
- [ ] Primary CTA hierarchy is explicit (one dominant action per view).
- [ ] **mWeb** is the default for scoped screens; app/desktop omitted unless explicitly requested.
- [ ] Stitch / prompts state **mobile web** (not native app) when using `MOBILE` device type.
- [ ] Output is copy-pasteable into Stitch or a design doc without extra narration.

## PRD-grounded UX

When UX must follow a **Product Requirements Document**, use **[prd-stitch-ux](../prd-stitch-ux/SKILL.md)** first: it **prompts for a PRD link**, ingests the spec, then runs **Google Stitch** with PRD-derived `designMd` and screen prompts. Combine with this skill so Stitch’s `designMd` and prompts include **redBus tokens** and **mWeb** defaults from [STITCH.md](STITCH.md).

## Additional resources

- **DESIGN.md template**, Stitch usage, screenshot naming, redRail IA cues: [reference.md](reference.md)
- **Google Stitch context (pin this):** [STITCH.md](STITCH.md)
- Full audit + evidence tables: [DESIGN.md](DESIGN.md)
- **PRD → Stitch workflow:** [prd-stitch-ux](../prd-stitch-ux/SKILL.md)
