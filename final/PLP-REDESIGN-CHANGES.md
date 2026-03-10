# PLP Redesign — Changes vs. Live Site

This document describes all changes the PLP prototype (`plp-redesign.html`) introduces compared to the live PureFormulas collection page.

---

## 1. Card Design: Autoship Radio Toggle

**Live site:** Shows a single price and a text line "**$XX.XX** with Autoship & Save" below it. No user interaction on the card — autoship frequency is configured at checkout.

**Prototype:** Adds a **one-time vs. autoship radio toggle** directly on the card:

- Two clickable rows with radio dots: **One-time** (shows price) and **Autoship** (shows discounted price + "with Autoship & Save")
- Autoship row defaults to active (`is-active`)
- When autoship is active, a **frequency selector** appears: "Every [−] 60 [+] days"
- A round **info icon** (ⓘ) replaces the "Learn More" link — clicking opens an autoship benefits modal
- Toggling between modes updates the card's visual state and syncs with the cart

---

## 2. Cart: Separate One-Time & Autoship Entries

**Live site:** Single "Add to Cart" button, no mode distinction.

**Prototype:**
- One-time and autoship are treated as **separate cart entries** for the same product
- Cart entries are keyed by `productIdx + mode`
- If autoship is in the cart and the user switches the card to one-time, the **qty stepper resets** to "Add to Cart" (since one-time isn't in cart yet)
- The slideout cart shows separate line items for each mode
- Autoship cart items render with the autoship upsell toggle **pre-checked** and the autoship price displayed
- Changing mode on the card after adding updates only that mode's cart entry

---

## 3. Qty Stepper on Card Image

**Live site:** Standard "Add to Cart" button only.

**Prototype:** After adding to cart, the ATC button transforms into a **qty stepper** (+/−) overlaid on the card image. The stepper reflects the qty for the **currently active mode** on that card.

---

## 4. Out-of-Stock Variant

**Live site:** Same structure exists but not visible in the reference collection.

**Prototype:** Products with `stock: 0` render differently:
- No ATC button or qty stepper
- Autoship toggle hidden
- Price still displayed
- **"Notify Me"** button (`btn btn-secondary`) + **"Out of Stock"** label in red (`#c14031`, `.8rem`, bold)
- Matches live site's `.PfProductListing .out-of-stock-label` styling

---

## 5. NEW Badge

**Live site:** Uses `<span class="badge badge-yellow card-badge">New</span>` on select products.

**Prototype:** Same markup and classes. Badge is positioned `absolute` at top-left of the card (direct child of `.pf-card`, not inside the image div). Appears on products with `isNew: true`.

---

## 6. Shipping Line

**Live site:** `<div class="pfCardProduct-shipping mt-2 fs-12">⚡️ Fast Free Shipping</div>` or `⚡️ Free Shipping on Orders $25+`.

**Prototype:** Same conditional logic — products $25+ show "⚡️ Fast Free Shipping", under $25 show "⚡️ Free Shipping on Orders $25+". Styled with `.pf-card-shipping` (12px, `#666`).

---

## 7. Promo Text

**Live site:** Not visible on collection cards.

**Prototype:** Optional promo badge below price: e.g., "**25% OFF** Code: **SAVE25**" in red (`#c14031`).

---

## 8. Promo Code System (Slideout Cart)

**Live site:** Basic promo code input with no visible validation.

**Prototype:** Full promo code system with validation, error handling, and two discount scopes:

### Cart-Level Codes
Apply a percentage or flat discount to the entire cart subtotal.

| Code | Discount |
|------|----------|
| `SAVE10` | 10% off cart |
| `SAVE20` | 20% off cart |
| `WELCOME15` | 15% off cart |
| `FLAT5` | $5 off cart |
| `PUREFORMULAS` | 10% off cart |

### Per-Product Codes
Only discount items whose product data has a matching `promo.code`. Products with `promo:{text:"25% OFF", code:"25PF"}` are eligible.

| Code | Discount | Eligible Products |
|------|----------|-------------------|
| `25PF` | 25% off | PureFormulas L-Theanine 60ct & 120ct |

- A green tag appears on each eligible cart item showing the per-item savings: "25PF: -$X.XX"
- If no eligible items are in the cart, shows error: "No eligible items in your cart for this code."

### Validation & Error Handling
- **Empty input:** "Please enter a promo code." (red text + red input border)
- **Invalid code:** "Invalid promo code. Please try again."
- **Already applied:** "A promo code is already applied. Remove it first."
- **No eligible items (product codes):** "No eligible items in your cart for this code."
- **Success:** "Code applied! X% off [eligible items / your order]." (green text)
- **Typing clears errors** — error message and red border reset on input
- **Enter key** submits the code
- **Remove button** on the applied code tag clears the promo and recalculates totals

### Order Summary
- Discount row appears showing "Promo Discount" with the applied code tag and total savings
- Subtotal shows original price crossed out when discount or autoship savings are active
- Autoship savings and promo discount stack (both reflected in final subtotal)

---

## 9. Sidebar Filters

**Live site:** Left sidebar (`col-lg-3`) with Bootstrap accordion, PF icon font for chevron arrows.

**Prototype:** Same structure — `col-lg-3 d-none d-lg-block` with `.filter-list > .accordion > .accordion-item`. Key differences:
- **Chevron arrows:** PF icon font files unavailable locally, so accordion arrows use inline SVG data URI chevron with CSS overrides
- **Accordion state:** All sections start collapsed (live site has Category expanded by default)
- **Sidebar padding:** `padding-top: 56px` on `.filter-list` to align with the product grid (below the toolbar)
- **Border:** `#d9dadb` bottom border per item, first item also gets a top border
- **Checkbox styling:** Unchecked border `#58585a`, checked `#205c40` (PF green)
- **Font:** 12px, 600 weight, `#232323` on accordion headers

---

## 9. Layout & Wrapper Classes

Matches live site structure:

```
div.PfProductListing.section.pfPLP
  div.container
    div.row.gx-5
      div.col-lg-3.d-none.d-lg-block  (sidebar)
      div.col-lg-9                      (content)
```

- Container uses `style.css` defaults: `width: 92.5vw; max-width: 1460px`
- Cards use `pf-card pfCardProduct` — the `pfCardProduct` class activates `style.css` rule: `.pfCardProduct button span { font-weight: 700; font-size: 15px; text-wrap: nowrap }`
- Card padding: `2rem .5rem 1rem` (matches `.pfCardProduct` pattern)
- Grid: `repeat(4, 1fr)` at desktop, responsive down to `repeat(2, 1fr)`

---

## 10. Pixel-Perfect Matching

These elements were specifically tuned to match the live site:

| Element | Live Site | Prototype |
|---------|-----------|-----------|
| **Price font** | `fs-21` (21px), `fw-bold` (600) | `21px`, `font-weight: 600` |
| **Brand text** | `fs-10`, `c-textlight` (#6a6a6a), weight 400 | `10px`, `#6a6a6a`, `font-weight: 400` |
| **Product name** | `fs-14`, `fw-bold` (600) | `14px`, `font-weight: 600` |
| **Review count** | `11px`, `#6a6a6a`, weight 400 | Same |
| **Trusted badge** | PF icon font `.i-check-badge` | Inline SVG: green filled badge + white checkmark |
| **Image aspect** | `ratio ratio-3x4` + `img.contain` | Same classes |
| **Card border** | `1px solid #d9dadb` | Same |
| **Accordion borders** | `#d9dadb` | Same |
| **Checkbox unchecked** | `border-color: #58585a` | Same |
| **Checkbox checked** | `background-color: #205c40` | Same |
| **Out-of-stock label** | `color: #c14031; .8rem; bold` | Same |
| **Button text** | `.pfCardProduct button span` (700, 15px) | Same (via `pfCardProduct` class) |

---

## 11. Summary: What's New vs. What's Matched

| Feature | Status |
|---------|--------|
| One-time / Autoship radio toggle on card | **New** |
| Frequency selector on card | **New** |
| Info icon (replaces "Learn More") | **New** |
| Qty stepper on card image | **New** |
| Separate cart entries per mode | **New** |
| Out-of-stock "Notify Me" variant | **New** (structure matches live) |
| NEW badge | **Matched** (same markup) |
| Shipping line | **Matched** |
| Promo text below price | **New** |
| Sidebar accordion filters | **Matched** (SVG chevron workaround) |
| Layout (col-lg-3 / col-lg-9) | **Matched** |
| Container width (92.5vw / 1460px) | **Matched** |
| Price/brand/name typography | **Matched** |
| Image sizing (ratio-3x4, contain) | **Matched** |
| Trusted badge icon | **Matched** (SVG equivalent) |
| Card padding & borders | **Matched** |
| Slideout cart with autoship upsell | **Enhanced** (pre-checks based on card mode) |
