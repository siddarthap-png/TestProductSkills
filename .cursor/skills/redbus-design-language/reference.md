# redBus design language — reference

## DESIGN.md template (Stitch / AI handoff)

Copy and fill from audited evidence. Replace `TBD` after verifying on [redbus.in](https://www.redbus.in/).

```markdown
# Design system: [Project name] (redBus-informed)

## Product intent
- Domain: Online bus and train booking (India-first, high-trust, deal-led).
- Primary job: Complete search → select → pay with minimal friction.
- Tone: Direct, promotional where appropriate, reassurance (support, refunds, tracking).

## Foundations
### Color tokens
| Token | Value | Usage |
|-------|-------|--------|
| color.brand.primary | TBD | Primary CTA, key accents |
| color.text.primary | TBD | Headings, body |
| color.text.muted | TBD | Secondary labels |
| color.surface.page | TBD | Page background |
| color.surface.card | TBD | Elevated panels |
| color.border.default | TBD | Inputs, dividers |
| color.link | TBD | Inline links |

### Typography
- Font sans: TBD (fallback stack if known)
- Scale: display / h1 / h2 / body / small / caption (sizes TBD)
- Rules: TBD (e.g. sentence case for UI labels)

### Layout
- Max content width: TBD
- Section vertical spacing: TBD
- Grid: TBD (columns, gutter)

### Shape & elevation
- Radius.input: TBD | Radius.button: TBD | Radius.card: TBD
- Shadow.card: TBD or none

## Components
### Header
- Nav items: Bus tickets, Train tickets, Hotels; utility: Bookings, Help, Account
- Optional: app install / promo strip above nav

### Search module (hero)
- Fields: From, To, Date of journey
- Quick actions: Today, Tomorrow; optional feature row (e.g. women booking)
- Primary CTA: Search buses (or context-appropriate)

### Cards & lists
- Offer/deal cards; statistic callouts; link-heavy footer lists

### Footer
- About, Info, global sites, partners, social, copyright

## Patterns
- Home: promo → nav → hero search → value props → offers → FAQs → SEO lists → footer
- Results/seat/checkout: TBD per audit scope

## Viewport coverage (UX output)
- **Default — mWeb only:** mobile browser width (~360–430px), touch-first, typical responsive chrome. Generate and capture **mWeb** unless the user asks for more.
- **App / desktop:** optional; include only when the user explicitly requests native app and/or desktop web.

## Non-goals
- TBD (e.g. no dark mode unless specified)

## Evidence
- Screenshots: [filenames or links]
- Audit date: YYYY-MM-DD
```

## Traverse any page (generic checklist)

Apply this whenever the skill should cover **more than the marketing home** (trains, bus search, payment, etc.):

1. **Enumerate URLs** — user list + nav/footer + vertical entry points.
2. **Per URL:** extract tokens from HTML/CSS **or** screenshots + DevTools; add a row to an **Evidence** table (URL, title, date, method).
3. **Same path, multiple steps (SPA):** add a **Logical step** column (`inventory`, `customer_info`, `payment`, …). One row per step; never merge two steps into one without both captures.
4. **Payment routes** (e.g. [`/railways/paymentDetails`](https://www.redbus.in/railways/paymentDetails)) — document shell and global chrome; mark payment body `TBD` if a booking session is missing. Reuse for other `…/payment…` paths the same way.
5. **Viewports (default mWeb):** for each logical screen, capture or generate **mWeb** first. Add **app** / **desktop** only if the user expands scope.

## Viewport naming (default: mWeb)

| Viewport | Meaning | Stitch `deviceType` | Prompt disambiguation |
|----------|---------|---------------------|------------------------|
| **mWeb** (default) | Site in mobile browser | `MOBILE` | Always say “mobile **web**” / “mWeb”; **not** native app. |
| **App** | Native iOS/Android | `MOBILE` or `TABLET` | Only if requested: “**native app**”, safe areas. |
| **Desktop** | Desktop browser | `DESKTOP` | Only if requested. |

**Default filename pattern:** `{flow}-{screen}-mweb.png`. Use `-app.png` / `-desktop.png` only when those viewports are in scope.

## redRail — canonical URLs & Stitch labels

Use these labels when uploading screenshots to Stitch so prompts can target the right screen:

| Logical screen | Example URL | Suggested screenshot name (mWeb default) |
|----------------|-------------|----------------------------------------|
| Train landing | `https://www.redbus.in/railways` | `redrail-landing-mweb.png` |
| Search · inventory | `https://www.redbus.in/railways/search?src=NDLS&dst=HWH&doj=YYYYMMDD&srcName=…&dstName=…&fcOpted=false` | `redrail-search-inventory-mweb.png` |
| Search · customer info | *same URL, after advancing flow* | `redrail-search-customer-info-mweb.png` |
| Payment | `https://www.redbus.in/railways/paymentDetails` | `redrail-payment-mweb.png` (TBD if empty without session) |

**Query params:** preserve `src`, `dst`, `doj`, `srcName`, `dstName`, `fcOpted` pattern; use a valid `doj` for the audit date.

### redRail — IA cues for DESIGN.md (copy structure, not long FAQ text)

- Hero: train booking headline, **IRCTC Authorised Partner**, optional promo code, From / To / Date of journey, quick date chips, Free Cancellation callouts, primary **Search Trains**.
- Below: top routes, testimonials, “How to book” steps, why redRail, IRCTC explainer, cancellation table, FAQs, footer columns (railway info, trains, routes, stations).
- Funnel: search results (trains/classes/fares) → IRCTC login + passenger details → payment.

## Using this in Google Stitch

1. Create a project at [stitch.withgoogle.com](https://stitch.withgoogle.com/).
2. Pin **[STITCH.md](STITCH.md)** as project context (Stitch-optimized: descriptive prose + hex). Use **DESIGN.md** for the full audit trail and token tables; merge updates into **STITCH.md** before pinning if you change tokens.
3. Add **labeled mWeb screenshots** per logical step (`…-mweb.png`). Add app/desktop references only if the user asks.
4. **Generate mWeb UX by default:** `deviceType` **`MOBILE`** with prompts that say **“mobile web”** / **“mWeb”** — not native app. One generation per key screen unless the user requests additional viewports.
5. Generate screens with prompts that reference tokens by name, e.g. “Use `color.brand.primary` for the primary CTA; match the Search module pattern from DESIGN.md.”
6. For multi-step flows, reference **screenshot filenames**, e.g. “Match `redrail-search-inventory-mweb.png` for mobile web train list density.”
7. Link screens into a **prototype** flow when testing journeys ([Stitch updates](https://blog.google/technology/google-labs/stitch-gemini-3)).

## Public IA cues (sanity check only)

Cross-check structure against the live home: bus/train/hotels, bookings/help/account, search block with From/To/Date and primary search action, trust and offer sections, FAQs, popular routes/cities/operators, footer links. Always **re-verify** colors and type in DevTools; this list is not a visual spec.
