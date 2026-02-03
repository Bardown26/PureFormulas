# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static HTML prototypes for PureFormulas autoship pricing and UX redesigns. No build tools, no frameworks, no dependencies beyond Google Fonts. Each page is a self-contained `.html` file with inline `<style>` and `<script>` blocks. Open directly in a browser.

## Pages

| File | Purpose |
|------|---------|
| `pf-pdp-redesign.html` | PDP — side-by-side current (broken pricing) vs proposed (correct autoship discount) |
| `pf-card-redesign.html` | Product card V1 — subscribe/one-time toggle with pricing module |
| `pf-card-redesign-v2.html` | Product card V2 — simplified, no toggle, autoship callout below price |
| `pf-cart-page-redesign.html` | Full cart page — per-item autoship radio selector, order summary sidebar |
| `pf-slideout-cart-redesign.html` | Slideout mini-cart — per-item autoship checkbox, savings banner in footer |

All pages link to each other via a sticky `<nav class="proto-nav">` at the top. Card V1/V2 are grouped in the nav.

## Architecture

- **No build step.** All CSS and JS is inline per file. Changes to shared patterns (nav, variables) must be replicated across all five files manually.
- **CSS variables** use `--pf-*` prefix, extracted from the live PureFormulas site. Key tokens: `--pf-green` (#00875a) for autoship/subscribe, `--pf-blue` (#0066cc) for one-time/primary CTA, `--pf-font` (Nunito Sans).
- **State management** uses `classList` toggling (`.is-active`, `.is-visible`, `.selected`) and `data-*` attributes for product data.
- **Price formatting** splits dollars and cents into separate `<span>` elements (`.main-val` / `.decimals`) with superscript styling.
- **Autoship discount** is hardcoded at 10% (`AUTOSHIP_DISCOUNT = 0.10`). Price calculation: `autoPrice = Math.round(basePrice * 0.9 * 100) / 100`.
- **Product images** use PureFormulas ccstore CDN: `https://www.pureformulas.com/ccstore/v1/images/?source=/file/[ID]/products/[name].jpg&height=940&width=940`

## Key Patterns

**Stacked radio options** (PDP, cart page): Adjacent options use `margin-top: -2px` to collapse borders, with `:first-child` / `:last-child` border-radius and `z-index: 1` on selected to lift above neighbors.

**Pricing toggle** (card V1): Two-button toggle (`.pricing-toggle__opt`) swaps visibility between `.pricing-subscribe` and `.pricing-onetime` views, updates CTA text and icon.

**Responsive breakpoint**: `@media (max-width: 900px)` collapses grid layouts from two columns to one.

## When Adding a New Page

1. Copy an existing file as a starting point
2. Add the `proto-nav` CSS block and `<nav>` HTML (match the pattern in other files)
3. Update the nav in **all other files** to include the new page link
4. Use `--pf-*` CSS variables for all colors, radii, shadows, and transitions
