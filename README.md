# Penn MEDIATED — Job Openings

The job-openings page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu). Static HTML/CSS, no build step. Mirrors the content of the live [`infodem.upenn.edu/job-openings`](https://infodem.upenn.edu/job-openings/) embed — heading and current-openings status only, not the surrounding WordPress nav/menu bar or site footer (newsletter signup, funder logos, social links, address), which are page chrome supplied by the WordPress wrapper rather than part of the embedded content.

Same conventions as the [`about`](https://github.com/PennMEDIATED/about) and [`home`](https://github.com/PennMEDIATED/home) repos — shared spacing tokens, brand colors, and fonts.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `assets/` — images and logos (none yet — add here if the page grows a logo or graphic)

## Updating content

When there's an open position to list, replace the `.openings__lead` paragraph ("We have no current job openings — please check back soon!") in `index.html` with the listing content, and add one `.openings__lead` (or a new `.openings__body p`, for longer copy) per posting — title, department, and a link to the application instructions. Keep the `.openings__title` heading as-is; only the status message below it needs to change. If listings grow to more than a couple of postings, consider a repeating card component (title + summary + "Apply" link) styled on the shared `.card-arrow` external-link pattern from `about`/`home`, rather than a long stack of paragraphs.

## Style guide (shared across `about`, `home`, and `our-team-job-openings`)

All three repos are static HTML/CSS built off the same design system. If you're adding or editing anything, pull values from here rather than guessing new ones — that's what keeps the sites looking like one brand instead of drifting apart.

### Design tokens (`:root` in `styles.css`)

**Spacing** — Atlassian's 8px scale. Always use the variable, never a raw pixel value:

```
--space-025: 2px   --space-100: 8px   --space-300: 24px  --space-600: 48px
--space-050: 4px   --space-150: 12px  --space-400: 32px  --space-800: 64px
--space-075: 6px   --space-200: 16px  --space-500: 40px  --space-1000: 80px
--space-250: 20px
```

**Color:**

| Token | Hex | Use |
|---|---|---|
| `--c-dark` | `#0d0d0c` | Primary text, dark backgrounds |
| `--c-accent` | `#5533ee` | Brand purple |
| `--c-red` | `#f03d1f` | Brand red/orange, links, tags |
| `--c-gray` | `#888680` | Secondary/muted text |
| `--c-gray-dark` | `#54534f` | Body copy needing real contrast (~8:1 on white) — matches `home`; prefer this over `--c-gray` for paragraph text if this page adds long-form copy |
| `--c-light-bg` | `#f8f7f4` | Placeholder/image background |
| `--c-white` | `#ffffff` | — |
| `--c-bg` | `#ffffff` | Page/body background |

This repo doesn't currently use `--c-light-bg` or the card-arrow badge — both exist in the shared token/component set but have no matching use case here yet, since the page is a single status message. Reach for them if this page grows a listings grid.

**Brand gradient** — used on every purple-to-red surface (the `about` page's orbital section, both repos' newsletter/supporters block, the `home` hero): `linear-gradient(150deg, #5533ee 0%, #df3611 81%)` via `--c-gradient`. Never write this gradient out by hand or approximate it with different stops — reference the variable so a future palette tweak only has to happen in one place per repo. Not currently used on this page (no gradient surface here yet).

**Type:**
- `--f-serif`: `'EB Garamond', Georgia, 'Times New Roman', serif` — headlines, quotes, the "MEDIATED" wordmark
- `--f-sans`: `'DM Sans', system-ui, -apple-system, sans-serif` — everything else

**Layout:** `--max-w: 1440px` page cap, `--pad-x: var(--space-1000)` (80px) side padding on the shared `*__inner` containers, scaling down responsively (32px under 900px, 20px under 480px) — backported from `home` since this page's heading is wide enough to benefit on mobile.

### Layout conventions

- Every section's content wrapper is named `.<section>__inner` and shares one rule (`width:100%; max-width:var(--max-w); margin-inline:auto; padding-inline:var(--pad-x);`). Add new sections to that shared selector list instead of writing a one-off inner container.
- BEM-ish naming: `.block__element`, modifiers as `.block--variant` or `.block__element--variant`.

### Heading and body-copy positioning

- **Body-copy blocks get no `max-width` of their own.** Paragraphs fill the full width of their padded `*__inner` container instead of stopping at a narrower fixed value. A narrower `max-width` inside a wide container looks asymmetric — all the leftover space piles up on the right since text is left-aligned, not centered.
- **A heading immediately followed by body copy uses a flat `--space-300` (24px) gap**, consistently, wherever that pattern occurs across the three repos. `.openings__title` → `.openings__lead` in this repo follows the same rule as `.partners__title` → `.partners__lede` in `about`.

### Shared components

- **No eyebrow/kicker above hero or section headings** — sitewide convention (the `.eyebrow` component that used to sit above headings in `about`, `team-leadership`, and `events` has been removed from all three). Don't add one above "Employment Opportunities" or anywhere else.
- **External-link arrow badge** (`.card-arrow`): a 26px black circle with a white arrow (10px), used on `about`/`home`'s logo and card tiles to signal "opens an external site." Not used on this page yet (no external-link tiles here) — pull it in if a listings-grid card component gets added.
## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` — no colour swap |

The underline is a `border-bottom`, not `text-decoration`, so its colour can be transitioned independently of the text on hover. Pair it with `transition: color 0.15s, border-color 0.15s` on light grounds and `transition: opacity 0.15s` on coloured ones.

White-to-anything reads poorly on a saturated ground, which is why the coloured case fades instead of changing hue.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Same colours, decoration and hover as category 1, **plus a thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Colour shift on hover per the ground rules above, and **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red` stroke, `stroke-width: 1.8`) beside a `--c-red` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

- **Scroll-triggered reveal**: sections carry a `.reveal` class; once the page's script confirms it can run (`js-reveal-ready` added to `<html>`), each fades/lifts in the first time it scrolls into view. This page's single `.openings` section uses the same one-shot reveal as most `about`/`home` sections (no `.reveal--toggle` re-fade behavior, since there's nothing to scroll past). A no-JS fallback marks everything visible immediately.

### Keeping the repos in sync

`about`, `home`, and `our-team-job-openings` are separate repos with duplicated CSS, not a shared stylesheet — so consistency is a discipline, not something enforced automatically. When you change a shared token or component in one repo, check whether the same change belongs in the others before considering the task done.

## Typography

Sitewide convention. The `--fs-*`/`--lh-*` block at the top of `styles.css` is canonical and identical in every page repo.

**Two families, no third.** `--f-serif` (EB Garamond) for page and section titles and pull-quote copy; `--f-sans` (DM Sans) for everything else. There is no monospace face — uppercase micro-labels are DM Sans 700 uppercase with `letter-spacing: 0.08em`.

**Sizes come from tokens, never raw px.**

| Token | Mobile (=<480px) | Desktop (>=1440px) | Used for |
| --- | --- | --- | --- |
| `--fs-display` | 36px | 76px | full-bleed hero |
| `--fs-h1` | 36px | 56px | page title |
| `--fs-h2` | 26px | 40px | section titles |
| `--fs-h3` | 20px | 24px | card and third-level titles |
| `--fs-lede` | 18px | 20px | intro paragraphs |
| `--fs-body` | 16px | 16px | body copy |
| `--fs-small` | 14px | 14px | captions, meta, form controls |
| `--fs-small-serif` | 15px | 15px | EB Garamond at small sizes |
| `--fs-micro` | 12px | 12px | uppercase labels, tags, counts |

The top five are `clamp()` values that interpolate across the viewport, so tablet widths need no separate `@media` override. Only add a breakpoint font-size when a specific layout actually demands it.

**12px is the floor.** Nothing ships smaller. EB Garamond and uppercase-with-letter-spacing both read smaller than their nominal size, which is what `--fs-small-serif` and the 12px floor exist to absorb.

**Line heights are tokens too** — `--lh-display` 1.05, `--lh-heading` 1.15, `--lh-lede` 1.26, `--lh-title` 1.3, `--lh-body` 1.55. Never set a line-height in px; it breaks the fluid sizes.

**Heading gaps.** Section title to first content is `var(--space-300)` (24px); page or hero title to content is `var(--space-250)` (20px).

**Section rhythm.** A full-width colored section carries `var(--space-1000)` (80px) top and bottom padding, so its heading never sits flush against the band's edge. The page hero's bottom padding is `var(--space-600)` (48px) — shorter than 80px because the section below supplies its own.
