# Design system: redBus.in (evidence-based, home bundle)

**Audit date:** 2026-04-04  
**Primary evidence:** Server-rendered HTML/CSS for `https://www.redbus.in/` (first ~150 KB of response, including inlined module CSS and `@font-face` blocks).  
**Coverage note:** Colors, radii, shadows, and type below are **verified for the home shell**. Search results, seat map, checkout, and account flows often ship additional chunks — re-audit those URLs the same way (or DevTools computed styles) before treating tokens as global.

---

## 1. Visual theme & atmosphere

- **Mood:** High-trust Indian travel marketplace — promotional but orderly; **search-first** hero; dense SEO and support content below the fold.
- **Density:** Transactional — strong primary actions, card-based promos, long link lists (routes, cities, operators).
- **Depth:** Mostly **flat** with **soft, layered shadows** on elevated cards (twin stop shadows using low-opacity black).
- **Motion:** View Transitions API hooks for **push** (home → search/bus-tickets) and **pop** (back to home) with ~0.7s slide; otherwise favor subtle `transition: all .3s ease` on focusable elements where present.

---

## 2. Color palette & roles

Use **semantic token names in Stitch prompts**; hex values are from the home stylesheet unless noted.

| Descriptive name | Hex | Role |
|------------------|-----|------|
| Brand scarlet | `#CD2400` | Primary brand / strong emphasis (appears in warm/CTA contexts in bundle) |
| Coral red | `#D63941` | Alternate strong accent / fills |
| Soft coral | `#DA5253`, `#EE8783` | Lighter reds, gradients, secondary emphasis |
| Deep wine | `#622726` | Dark red-brown for contrast blocks |
| Focus indigo | `#4040F2` | Visible focus ring (skip-link outline) |
| Action blue | `#285BF3` | Interactive / link-adjacent blues |
| Periwinkle | `#5258E4`, `#7DA3F9`, `#909FF5` | Accents, highlights, soft UI blues |
| Tint surface | `#E4ECFD` | Light blue-tinted panel background |
| Ink black | `#18181B`, `#1D1D1D`, `#202023` | Near-black text / chrome |
| Zinc 800 | `#27272A` | Dark UI surfaces |
| Gray body | `#303030`, `#4A4A4A`, `#4B4B4B`, `#6F6F6F` | Body and secondary text |
| Muted gray | `#B0B0B0`, `#B2B2B2`, `#D0D0D0` | Borders, dividers, placeholders |
| Page white | `#FFFFFF`, `#FDFDFD`, `#FCFCFC` | Page and card surfaces |
| Mist lilac | `#F2F2F8` | Soft section background |
| Success green | `#007B28`, `#76B27D` | Positive / success cues where used |
| Warning amber | `#B14B00`, `#F9E2B7`, `#FFF5D7`, `#FEDAC8`, `#FED9D5` | Offer/warning tints |

**RGBA overlays (elevation / imagery):** `rgba(0,0,0,.078)`, `.322`, `.4`, `.6`, `.7` on shadows and banner scrims; `rgba(29,29,29,.46)` / `.64` for modal-style scrims; light tints on `rgba(255,235,142,.302)` and `rgba(197,25,28,.302)` for highlight washes.

---

## 3. Typography rules

- **Font family:** **Inter** (self-hosted WOFF2), with `Inter, sans-serif` as stack; weights **400**, **500**, **700** present in rules; **600** appears at least once in bundle.
- **Observed scale (home):** Mobile section title pattern **22px / 28px line-height**, **700** weight, **letter-spacing ~-0.4px**; skip link **14px / 500**.
- **Rules for Stitch prompts:** Sentence case for UI labels unless matching marketing headers; **one dominant weight** for hierarchy (700 for headings, 400–500 for body).  
- **TBD (verify in funnel):** Full type ramp (H1 hero, tab labels, legal/footer) — not fully extractable from a single minified line; sample in browser.

---

## 4. Component stylings

- **Buttons / chips:** **Pill shapes** common (`border-radius: 999px` repeated). Use brand red fills with white label for primary; ghost/secondary uses gray borders (see neutral palette).
- **Cards / containers:** **16px** and **24px** corner radii; **32px** for larger panels; **soft double box-shadow**: e.g. `0 4px 8px rgba(0,0,0,.078), 0 8px 16px rgba(0,0,0,.078)` (lighter) and a stronger variant with `.322` opacity for emphasis/hover layers.
- **Inputs / forms:** Rounded fields (8px / 16px contexts in bundle); light borders from gray tokens; **focus ring** should align with `#4040F2` for accessibility parity with skip control.
- **Header / nav (IA):** Bus tickets, Train tickets, Hotels; utilities Bookings, Help, Account — optional promo strip above (pattern described in product; exact copy varies).
- **Hero search module:** From / To / Date of journey; quick date chips (e.g. Today, Tomorrow); primary CTA for search — **single scarlet-family CTA** per view.

---

## 5. Layout principles

- **Max content width:** **1280px** on SEO/partials wrapper (`.max-width:1280px`) on wide breakpoints; **mWeb-first UX** uses full-bleed **single column** with **1rem** horizontal padding.
- **Spacing:** Vertical section rhythm uses **0.75rem** gaps in stacked modules; **1.5rem**+ margins on mobile headers.
- **Grid:** Flex-column stacks for mobile-first sections; trust/offer grids implied by card components.
- **Breakpoints:** TBD — extract from full CSS or DevTools; not resolved from truncated fetch alone.

### Viewport default: mWeb

**Generated UX and primary specs target mWeb** (mobile web: ~360–430px browser width, touch-first, responsive chrome). **Native app** and **desktop web** are **out of scope** unless the user explicitly requests them; tokens stay valid if you add those later.

---

## 6. Patterns & journeys

- **Home stack:** Promo / app install → global nav → **hero search** → value props → offers → FAQs → SEO lists (routes, cities, operators) → footer.
- **Documented route transitions (script):** From `/` to paths starting with `/search` or `/bus-tickets` uses forward transition; reverse uses pop. Use the same **information hierarchy** in Stitch prototypes: never bury primary search.

---

## 7. Token table (machine-friendly): Stitch / code

| Token | Value | Usage |
|-------|-------|--------|
| color.brand.primary | `#CD2400` | Primary CTA, brand bars |
| color.brand.primary.alt | `#D63941` | Alternate fills, badges |
| color.accent.blue | `#285BF3` | Links, secondary actions |
| color.accent.focus | `#4040F2` | Focus rings |
| color.text.primary | `#1D1D1D` / `#202023` | Headings, body |
| color.text.muted | `#6F6F6F` | Secondary labels |
| color.surface.page | `#FFFFFF` | Default page |
| color.surface.subtle | `#F2F2F8` | Section bands |
| color.surface.card | `#FDFDFD` | Cards |
| color.border.default | `#D0D0D0` | Inputs, dividers |
| radius.pill | `999px` | Chips, pill buttons |
| radius.card | `16px` | Cards |
| radius.panel | `24px`–`32px` | Large containers |
| shadow.card.soft | `0 4px 8px rgba(0,0,0,0.078), 0 8px 16px rgba(0,0,0,0.078)` | Default elevation |
| shadow.card.strong | same with `0.322` alpha | Emphasized / hover |
| font.family.sans | Inter, sans-serif | All UI |
| font.weight.regular | 400 | Body |
| font.weight.medium | 500 | Labels, skip link |
| font.weight.bold | 700 | Headings |

---

## 8. Using this in Google Stitch

**Preferred handoff file:** [`STITCH.md`](STITCH.md) — concise, descriptive language plus hex (matches [Stitch DESIGN.md-style context](https://github.com/google-labs-code/stitch-skills/tree/main/skills/design-md)). Pin the whole file as project context.

1. Create a project at [stitch.withgoogle.com](https://stitch.withgoogle.com/).
2. Pin **`STITCH.md`** (or paste **sections 1–5 + 6–8** from this doc if you want the long-form version).
3. Add **mWeb** screenshots (`…-mweb.png`) for each important screen. Add app/desktop only if the user expands scope.
4. **Generate mWeb by default:** Stitch `deviceType` **`MOBILE`** + prompts that say **“mobile web”** / **mWeb** (not native app). One generation per key screen unless the user asks for more viewports.
5. Prompt with tokens, e.g. *“Primary button: `color.brand.primary` fill, white text, `radius.pill`; card uses `shadow.card.soft` and `radius.card`.”*
6. Follow [Stitch prompting guide](https://stitch.withgoogle.com/docs/learn/prompting/) for visual descriptions + hex in parentheses.

---

## 9. Evidence & next steps

| URL / step | Default viewport | Notes |
|------------|------------------|--------|
| `https://www.redbus.in/` | **mWeb** | Home shell — hex/type/shadows from inlined CSS (see §2–5). |
| `https://www.redbus.in/railways` | **mWeb** | redRail landing — IA per [live page](https://www.redbus.in/railways). |
| `https://www.redbus.in/railways/search?…` · **inventory** | **mWeb** | Train list. |
| `https://www.redbus.in/railways/search?…` · **customer_info** | **mWeb** | IRCTC + passenger — same path, separate logical screen. |
| `https://www.redbus.in/railways/paymentDetails` | **mWeb** | Payment shell — may need session; mark `TBD` if not captured. |

**Next:** Merge any **new** hex values from redRail bundles into §2; document train row cards, class selectors, and form fields in §4 per step. For bus, add `/search`, `/bus-tickets/...`, seat map, payment analogs using the same evidence table pattern.

---

## 10. redRail (trains) — product IA (Stitch-friendly)

Use with §1–7 for prompts about **train** UI. Visual tokens are expected to **align with the global redBus shell** (Inter, brand reds, card shadows) unless step-specific CSS contradicts — always verify.

- **Landing** ([`/railways`](https://www.redbus.in/railways)): “Train Ticket Booking”; IRCTC authorised partner; promo code (e.g. savings messaging); **From / To / Date of journey**; chips such as **Tomorrow**, **Day after**; **Free Cancellation** / refund messaging; primary **Search Trains**; top train routes with secondary “Search train” actions; testimonials; long-form trust and how-to; IRCTC FAQs; footer rich in railway links (PNR, Tatkal, stations, routes).
- **Search · inventory:** User has submitted origin/destination/date — surface **train list**, times, classes, fares, availability patterns (exact UI: evidence from screenshot).
- **Search · customer info:** **IRCTC login**, passenger details, add-ons (e.g. insurance, cancellation options) — document after advancing from inventory.
- **Payment** ([`/railways/paymentDetails`](https://www.redbus.in/railways/paymentDetails)): Treat as **generic payment template** for Stitch — header/nav from global system; payment body `TBD` without session.

**Stitch prompt snippets (mWeb default):**  
- *Landing:* “**Mobile web** redRail landing, single column, touch targets…”  
- *Inventory:* “**Mobile web** train list — one primary select per row, dense stacked cards…”
