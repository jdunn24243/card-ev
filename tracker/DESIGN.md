# Stax — Design Specification
> Card portfolio tracker by Lakes and Loons Cards

---

## Product overview

Stax is a card inventory and P&L tracker. The core job: show a collector whether they are making money, how much their unsold inventory is worth, and what their historical performance looks like. It serves both casual collectors trying to stay non-negative and serious flippers trying to grow profits.

**Companion to:** Card EV calculator (private tool, separate identity — do not share visual language)

**Hosting:** GitHub Pages, `/tracker/` in existing repo  
**Backend:** Supabase (project ID: `jporxlodndnxmmsazgol`)  
**Stack:** Static PWA, plain HTML/CSS/JS, Supabase JS SDK via CDN

---

## Name & branding

**App name:** STAX  
**Byline:** by Lakes and Loons Cards  
**Tagline / descriptor:** card tracker  

### Why "Stax"
Every card you buy goes on the stack — inventory, investment, collection. Every card you sell adds to a different stack — profits, history, track record. The name works for casual collectors and serious flippers equally. "Stacks" in common usage also just means money, so it reads immediately even outside the hobby.

---

## Visual direction

**Direction:** The Ledger  
**Feel:** Physical trading ledger meets financial statement. Warm, archival, precise. Belongs to the collector.

### Color tokens
```
--bg:       #f4efe6   /* parchment base */
--bg2:      #ede8dc   /* section header tint */
--bg3:      #e6e0d2   /* deeper tint */
--ink:      #1a1612   /* primary text + rules */
--ink2:     #5a5248   /* secondary text */
--ink3:     #9a9080   /* muted / labels */
--rule:     #d4ccc0   /* light dividers */
--rule2:    #c8c0b4   /* medium dividers */
--pos:      #2a6818   /* profit / positive */
--neg:      #b83020   /* loss / negative */
--gold:     #9a7830   /* hold time / neutral accent */
```

### Typography
**One typeface: IBM Plex Mono**  
Hierarchy is achieved through size and weight only — no second typeface.  
`@import` from Google Fonts: weights 300, 400, 500, 600, 700.

> Note: Playfair Display was considered for the profit hero number and remains a one-line change if the decision is revisited. Keep the import commented out in the CSS for easy switching.

### What to avoid
- Do NOT use the EV calculator's design tokens (dark bg `#0f0f0f`, lime green `#c8f060`, DM Mono, Syne)
- No dark backgrounds on the main screens
- No rounded pill buttons — use `border-radius: 8px` max

---

## Navigation model

**Pattern:** Hub and spoke — Dashboard is the landing screen.  
**Add Card:** Full-screen form, triggered by the Add Card button. Slides in over Dashboard (push), slides back on submit or cancel (pop).  
**Search/Update:** Full-screen list + detail, triggered by the search icon. Same push/pop behavior.  
**No tab bar.** Navigation is intentional — you go somewhere to do something, then return home.

---

## Dashboard screen

### Header (H3)
```
STAX                          card tracker
```
- `STAX` in IBM Plex Mono, 22–24px, weight 600, letter-spacing 0.1em
- `card tracker` right-aligned, 9px, weight 400, letter-spacing 0.1em, color `--ink3`
- Bottom border: 1.5px solid `--ink`

### P&L hero
- Large profit number: IBM Plex Mono, ~40px, weight 700, tight tracking
- Right of number: small label ("Total profit · all time") + ROI callout with ▲ prefix
- ROI color: `--pos`
- Bottom border: 1px solid `--rule`

### Sold performance block
- Section label bar (bg `--bg2`): "Sold performance" left, card count right
- 2×2 grid of stat cells, separated by 1px `--rule` gaps
- Cells: Total cost · Total revenue · Total profit (color `--pos`) · Avg hold time (color `--gold`)
- Bottom border: 1.5px solid `--rule2`

### Inventory block
- Section label bar (bg `--bg2`): "Inventory" left, card count right
- Two horizontal strip rows, each with two items (label + value, left and right aligned)
- Row 1: Total invested (left) · Est. market value + range sub-label (right)
- Row 2: Proj. at hist. ROI (left) · Unrealized gain in `--pos` + % sub-label (right)
- Est. value shown as midpoint with ±20% range until personalized ROI data available

### Footer (F3)
- "by Lakes and Loons Cards" right-aligned, 8px, color `--ink3`
- Sits above the button row
- Add Card button: full-width dark (`--ink` bg, `--bg` text), mono caps, + icon
- Search button: icon only, transparent bg, `--rule2` border, sits beside Add Card

---

## Screens to build (in order)

1. **Dashboard** — read-only, all data from Supabase
2. **Add Card** — form to log a new purchase
3. **Search/Update** — find a card, log a sale, add notes

### Add Card fields (from existing data schema)
Card Name, Cost, Purchase Date, Purchase Location, Notes

### Search/Update fields
Sale price, Sell Date, Sale Location, Notes  
Also: status updates like "submitted for grading"

---

## Data & inventory logic

### Price tiers (for ROI breakdown feature — future)
- Under $10
- $10–$75  
- $75+

### Inventory estimate
- Default: ±20% of unsold cost basis (midpoint displayed, range shown as sub-label)
- Future: personalize to user's historical ROI by price tier
- New users default to ±20% until enough data exists

### CSV import
One-time import of existing dataset for Jack's account. Schema maps to:
`Card Name, Cost, Sale, Profit/Loss, ROI, Purchase Date, Sell Date, Hold Time, Price Adjustment Needed, Purchase Location, Sale Location, Notes`

---

## Future considerations

- ROI breakdown by price tier (tap avg ROI row on dashboard to expand)
- Monetization via Stripe (potential)
- Public product — design and code should be written as if other people will use it

---

## Decisions log

| Decision | Choice | Reason |
|---|---|---|
| Backend | Supabase | Multi-device access requirement |
| Typography | IBM Plex Mono only | Unified feel; Playfair felt like a different app |
| Navigation | Hub and spoke | Add Card is a task not a destination |
| App name | STAX | Short, hobby-adjacent, financial connotation, works for all user types |
| Design direction | The Ledger | Warm, personal, archival — distinct from EV calculator |
| Layout | B+C hybrid | C's inline P&L hero, B's 2×2 grid and inventory strips |
| Header | H3 mono caps | Purposeful, financial-tool feel |
| Footer imprint | F3 right-aligned | Present but unobtrusive, above the action button |
