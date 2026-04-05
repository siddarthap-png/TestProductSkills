# Design system: redBus.in — Google Stitch context

**Use this file:** Pin or paste the full document into your [Stitch](https://stitch.withgoogle.com/) project as **design system context**. Pair it with **mWeb** reference screenshots (`…-mweb.png`) per screen.  
**Prompting guide:** [Stitch — Effective prompting](https://stitch.withgoogle.com/docs/learn/prompting/)

---

## 0. Viewport default: mWeb only

**Generated UX targets mobile web (mWeb)** unless the user explicitly asks for **native app** and/or **desktop**.

| Default | Stitch | Prompt must say |
|---------|--------|-----------------|
| **mWeb** | `deviceType: MOBILE` | “**Mobile web**” / “mWeb” / “responsive site in mobile **browser**” — **not** a native app (no iOS/Android system status bar unless simulating in-browser chrome only if needed). |

**Workflow:** One **`MOBILE`** generation per screen with mWeb wording. Same tokens (`color.brand.primary`, etc.). Optional **app** (`MOBILE` + “native app”) or **desktop** (`DESKTOP`) only on user request.

---

## 1. Visual theme & atmosphere

The product feels like a **confident, high-trust Indian travel marketplace**: promotional but clean, **search-first**, with conversion-focused heroes and long supporting sections below the fold. Visual language is **bright and transactional** — white and soft gray surfaces, **brand scarlet** for the main action, and restrained blue accents for links and focus. Cards use **gentle stacked shadows** (soft black at low opacity), not heavy glassmorphism. Density is **medium-high** in funnels (lists of buses/trains); marketing pages breathe more with section breaks. Motion is **subtle** (short fades/slides on route change); avoid playful bounces unless matching reference screenshots.

---

## 2. Color palette & roles

Stitch reads **visual descriptions best when every important color includes its hex** in parentheses.

- **Brand scarlet (`#CD2400`)** — Primary brand bar, filled primary buttons, urgent promotional emphasis.  
- **Coral red (`#D63941`)** — Alternate primary fills, badges, warm highlights.  
- **Soft coral (`#DA5253`, `#EE8783`)** — Gradients, secondary emphasis, softer red UI blocks.  
- **Deep wine (`#622726`)** — Dark warm contrast for banners or rich footer bands.  
- **Focus indigo (`#4040F2`)** — Keyboard focus rings and strong accessible outlines.  
- **Action blue (`#285BF3`)** — Text links, secondary interactive accents.  
- **Periwinkle family (`#5258E4`, `#7DA3F9`, `#909FF5`)** — Soft highlights, informational chips, tinted UI.  
- **Cool tint surface (`#E4ECFD`)** — Light panels and highlighted rows.  
- **Ink (`#18181B`, `#1D1D1D`, `#202023`)** — Primary text and strong chrome.  
- **Zinc slab (`#27272A`)** — Dark surfaces in modals or dense chrome.  
- **Body gray (`#303030`, `#4A4A4A`, `#4B4B4B`, `#6F6F6F`)** — Secondary text and icons.  
- **Border gray (`#B0B0B0`, `#B2B2B2`, `#D0D0D0`)** — Input strokes, dividers, disabled fill hints.  
- **Page white (`#FFFFFF`, `#FDFDFD`, `#FCFCFC`)** — Default page and card faces.  
- **Mist lilac (`#F2F2F8`)** — Alternating section backgrounds.  
- **Success green (`#007B28`, `#76B27D`)** — Positive states, “Free cancellation” style reassurance.  
- **Warning amber (`#B14B00`, `#F9E2B7`, `#FFF5D7`, `#FEDAC8`, `#FED9D5`)** — Offers, gentle warnings, promo washes.

**Shadows / overlays:** Use **whisper-soft elevation**: twin shadows such as `0 4px 8px rgba(0,0,0,0.078)` and `0 8px 16px rgba(0,0,0,0.078)` for default cards; a **stronger** pair with **0.322** alpha when the card is selected or hovered. Banner scrims: `rgba(0,0,0,0.7)` gradients over imagery.

---

## 3. Typography rules

- **Family:** **Inter** (sans-serif), weights **400** (body), **500** (labels, UI chrome), **700** (headings); **600** may appear for subhead emphasis.  
- **Hierarchy:** Clear step down from **bold section titles** to **regular body**; avoid more than three distinct sizes on one screen.  
- **Section titles (mobile-style reference):** ~**22px**, **700**, tight line height (~**28px**), slight negative letter-spacing for a modern marketing feel.  
- **UI chrome:** ~**14px** / **500** for compact controls (e.g. skip link pattern).  
- **Copy style:** Direct, offer-aware, short sentences; **sentence case** for controls unless matching a marketing headline.

---

## 4. Component stylings

- **Primary button:** **Pill or fully rounded** (`border-radius` ~**999px**); fill **Brand scarlet (`#CD2400`)** or **Coral (`#D63941`)**; **white** label; single primary CTA per viewport when possible.  
- **Secondary / ghost:** Outline or light fill using **border gray**; text **Ink** or **Action blue** for text-button style.  
- **Chips / date quick actions (Today, Tomorrow, Day after):** Pill shape; light surface or subtle border; selected state uses **brand** or **tint** — match screenshot.  
- **Cards:** **16px** corners (larger panels **24px–32px**); background **Page white** or **Mist lilac**; **soft twin shadow** as in §2.  
- **Inputs:** Rounded (**8px–16px**); border **Border gray**; focus ring **Focus indigo (`#4040F2`)**.  
- **Header:** Top nav with **Bus tickets**, **Train tickets**, **Hotels**; utilities **Bookings**, **Help**, **Account**. Optional promo strip above.  
- **Hero search module:** **From**, **To**, **Date of journey**; primary CTA **Search buses** or **Search trains** depending on vertical.

---

## 5. Layout principles (mWeb-first)

- **mWeb (default):** Full-bleed **single column**; **sticky** primary CTA or bottom bar when appropriate for long forms; **44px+** touch targets; collapsible or compact top nav typical of mobile web.  
- **Vertical rhythm:** **0.75rem** gaps in tight stacks; **1.5rem+** above section titles.  
- **Priority:** **Search and primary CTA** above the fold; legal and SEO below.  
- **Desktop / native app:** out of scope unless the user asks — then add max-width columns, hover, or safe-area app chrome respectively.

---

## 6. Token quick reference (paste into prompts)

| Token | Value | When to mention in a prompt |
|-------|-------|-----------------------------|
| `color.brand.primary` | `#CD2400` | “Primary CTA fill” |
| `color.brand.primary.alt` | `#D63941` | “Alternate red button / badge” |
| `color.accent.focus` | `#4040F2` | “Focus ring” |
| `color.accent.blue` | `#285BF3` | “Text links” |
| `color.text.primary` | `#1D1D1D` | “Headings and body” |
| `color.text.muted` | `#6F6F6F` | “Secondary labels” |
| `color.surface.page` | `#FFFFFF` | “Page background” |
| `color.surface.subtle` | `#F2F2F8` | “Section band” |
| `color.border.default` | `#D0D0D0` | “Input border” |
| `radius.pill` | `999px` | “Chip / primary button” |
| `radius.card` | `16px` | “Card corners” |
| `shadow.card.soft` | see §2 | “Default card elevation” |

---

## 7. redRail (trains) — screen guidance

Apply **the same tokens and components** unless a screenshot shows a deliberate divergence.

- **Landing (`/railways`):** Headline **Train ticket booking**; **IRCTC authorised partner** trust; promo code line; same **From / To / Date** + **Tomorrow / Day after** chips; **Free cancellation** reassurance row; primary **Search trains** (`color.brand.primary`).  
- **Search · inventory:** Dense **train list**; times, classes, fares; one clear **select / continue** action per row; card/list styling per §4.  
- **Search · passenger / IRCTC:** Form-heavy; maintain **input** and **focus** rules; keep **single primary** continue.  
- **Payment (`/railways/paymentDetails`):** Reuse **global header** and surfaces; payment-specific layout **follow attached screenshot** if provided.

---

## 8. Example Stitch prompts (mWeb default)

- *“**Mobile web** (mWeb) redBus home: `color.surface.page`, stacked hero From/To/Date, pill chips, full-width primary `color.brand.primary` (`#CD2400`), `radius.pill`; responsive mobile **browser**, not native app.”*  
- *“**Mobile web** redRail landing: IRCTC partner line, Search trains CTA, Inter, same tokens; single column, touch-friendly.”*  
- *“**Mobile web** redRail **inventory**: dense train list, one primary select per row, member badges per §7 if subscription UX; `MOBILE` layout.”*

*If the user later asks for desktop or native app, add separate prompts with `DESKTOP` or explicit “native app” wording.*

---

## 9. Evidence note (for humans)

Detailed audit trail, extra hex mining, and URL checklist live in **`DESIGN.md`** in this folder. Refresh **§2–§4** there if you capture new screenshots or DevTools values, then mirror any changes into this **STITCH.md** before regenerating UI in Stitch. Default evidence filenames: **`…-mweb.png`** per screen.
