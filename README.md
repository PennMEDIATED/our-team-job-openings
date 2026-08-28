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
- `--f-mono`: `'Courier New', Courier, monospace` — small meta labels only

**Layout:** `--max-w: 1440px` page cap, `--pad-x: var(--space-1000)` (80px) side padding on the shared `*__inner` containers, scaling down responsively (32px under 900px, 20px under 480px) — backported from `home` since this page's heading is wide enough to benefit on mobile.

### Layout conventions

- Every section's content wrapper is named `.<section>__inner` and shares one rule (`width:100%; max-width:var(--max-w); margin-inline:auto; padding-inline:var(--pad-x);`). Add new sections to that shared selector list instead of writing a one-off inner container.
- BEM-ish naming: `.block__element`, modifiers as `.block--variant` or `.block__element--variant`.

### Heading and body-copy positioning

- **Body-copy blocks get no `max-width` of their own.** Paragraphs fill the full width of their padded `*__inner` container instead of stopping at a narrower fixed value. A narrower `max-width` inside a wide container looks asymmetric — all the leftover space piles up on the right since text is left-aligned, not centered.
- **A heading immediately followed by body copy uses a flat `--space-300` (24px) gap**, consistently, wherever that pattern occurs across the three repos. `.openings__title` → `.openings__lead` in this repo follows the same rule as `.partners__title` → `.partners__lede` in `about`.

### Shared components

- **Eyebrow label** (`.eyebrow`): a small uppercase red kicker above a section heading, used above "Mission Statement" in `about`. Not used on this page (the heading stands alone here) — pull it in if this page later wants a kicker above "Employment Opportunities."
- **External-link arrow badge** (`.card-arrow`): a 26px black circle with a white arrow (10px), used on `about`/`home`'s logo and card tiles to signal "opens an external site." Not used on this page yet (no external-link tiles here) — pull it in if a listings-grid card component gets added.
- **Scroll-triggered reveal**: sections carry a `.reveal` class; once the page's script confirms it can run (`js-reveal-ready` added to `<html>`), each fades/lifts in the first time it scrolls into view. This page's single `.openings` section uses the same one-shot reveal as most `about`/`home` sections (no `.reveal--toggle` re-fade behavior, since there's nothing to scroll past). A no-JS fallback marks everything visible immediately.

### Keeping the repos in sync

`about`, `home`, and `our-team-job-openings` are separate repos with duplicated CSS, not a shared stylesheet — so consistency is a discipline, not something enforced automatically. When you change a shared token or component in one repo, check whether the same change belongs in the others before considering the task done.
