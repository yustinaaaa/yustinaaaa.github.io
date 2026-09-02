# Yustina Selezneva — Portfolio

Image-free, bilingual (RU/EN) creative portfolio for a creative strategist /
special-projects creative. The design uses typography, restrained color
fields and information design instead of NDA-protected campaign visuals —
that absence of imagery is intentional, not a placeholder.

Art direction is deliberately editorial rather than illustrative: no
decorative shape systems, no per-case graphic motifs. Content is
communicated through type hierarchy, a small reused palette (four
gradients, two accents — see "Design tokens" below), and scale (numbers set
large where the number itself is the story, e.g. the federal-promo case).
When editing, resist adding a new decorative element per section — reuse
what exists or ask whether the section needs it at all.

Zero-build static site: one HTML file, one CSS file, one JS file. No
dependencies, no bundler, no framework.

## Run locally

Open `index.html` directly in a browser, or serve the folder with any static
server, e.g.:

```
python3 -m http.server 8080
```

then visit `http://localhost:8080`.

## Deploy

Any static host works — the folder is self-contained. Drag-and-drop onto
Netlify/Vercel, push to GitHub Pages, or upload via `rsync`/`scp` to any web
server. No environment variables, no build step, no server-side code.

## Files

- `index.html` — structure and content (Russian by default; English strings
  live in `script.js` and are swapped in at runtime)
- `styles.css` — all styling: tokens, layout, responsive rules, motion
- `script.js` — RU/EN language switch, cursor interaction
- `assets/` — currently empty by design (see NDA note below)

## Where the RU/EN content lives

Visible RU copy sits directly in `index.html` as the default text on each
element tagged `data-i18n="key"`. The full bilingual dictionary — both the RU
strings (mirrored) and the EN translations — lives in `script.js` in the
`copy` object, keyed the same way (`copy.ru['key']`, `copy.en['key']`).

To edit copy:
1. Find the key (e.g. `hero.copy`, `case5.lead`, `about.p1`) in `script.js`.
2. Edit the string there. If it's shown by default on page load, also update
   the matching text in `index.html` so the two don't drift out of sync.

The switch itself (`renderLanguage()` in `script.js`) writes `innerHTML` for
every `[data-i18n]` element, updates `<html lang>`, and remembers the choice
in `localStorage` — no page reload, no scroll-position loss.

## How to edit proof points

The big-number scale section (`50M ₽`, `5×`, `2/4`, `3`, `100`, `360°`) is
the `<section class="outcomes">` block in `index.html` — each `<article
class="outcome">` pairs a `<strong>` (the number) with a `<p data-i18n="…">`
(the one-line description, translated in `script.js`). The four-item scale
strip near the top of the hero (`<section class="scale-strip">`) works the
same way. Keep numbers short (they're set in a huge display size) and
descriptions under ~2 lines — the layout isn't designed for longer copy here.

Wording matters for NDA accuracy — see "Content integrity / NDA notes" below
before changing any of the federal-promo or budget language.

## How to edit case studies

Each case is one `<article class="case case-0N">` (add `case-reverse` to flip
the field to the right) inside `<section id="work">`. A case has:
- `.case-field` — a restrained color/gradient panel, `aria-hidden`, showing
  a single typographic mark: the case number (`.case-num`) for most cases,
  or a proof number + label (`.case-field-proof`, `.case-proof-number`,
  `.case-proof-label`) for the one case that leads with a figure instead —
  currently case 05, the federal promo. No illustration, no shapes.
- `.case-copy` — the real content: category tag, `<h3>` title, lead
  paragraph, and a `<dl>` of role / scale / outcome

The six cases alternate which side the field sits on via the `case-reverse`
class (odd cases: field left; even cases with `case-reverse`: field right) —
add or drop that class per case rather than relying on automatic ordering.

Field colors are assigned by case number in `styles.css`
(`.case-01 .case-field`, `.case-02 .case-field`, etc.) from the four
restrained gradients defined in `:root` — see "Design tokens" below. To add
a new case, copy an existing `<article class="case case-0N">` block, give it
the next number, add a `.case-0N .case-field` color rule using one of the
existing `--grad-*` tokens (don't introduce a new color per case — the
system depends on reusing the same small palette), then add its copy to both
`index.html` and the `copy.ru`/`copy.en` dictionaries in `script.js` under a
new `caseN.*` key prefix.

## How the animation system works

Motion is deliberately minimal:
- `.cursor-dot` — a blend-mode dot that follows the pointer, shown only on
  devices with real hover (`matchMedia('(hover:hover)')` in `script.js`,
  reinforced by a `(hover:none)` CSS media query that hides it on touch).
- `html { scroll-behavior: smooth }` for anchor-link scrolling.

There is no other motion on the page — no rotating shapes, no
scroll-triggered reveals, no hover-only decoration. `@media
(prefers-reduced-motion: reduce)` turns off the cursor dot and switches
scrolling to instant. Content is visible immediately on load/scroll, which
keeps the page fast and avoids the "everything fades in" pattern that reads
as generic/templated.

## Design tokens

`:root` in `styles.css` defines the reusable values:
- Color: `--paper`, `--ink`, `--muted`, `--line`, plus two accents
  (`--accent` burgundy, `--accent-2` ink blue) used sparingly — proof
  points, one case field, small UI moments. Not a rainbow palette by
  design; adding a third accent color should be a deliberate decision,
  not a default.
- Gradients: `--grad-ink-blue`, `--grad-burgundy`, `--grad-cream-blush`,
  `--grad-graphite-navy` — the only four background treatments used across
  hero cards, case fields, and the startup section. Reuse these rather than
  inventing a new gradient per section; the restraint is the point.
- Layout: `--pad` (page margin), `--content-max` (longest headline measure)
- Spacing rhythm: `--space-xs` through `--space-xl` — the gap/margin sizes
  reused across multiple unrelated sections. Most section-specific spacing
  is still hand-tuned `clamp()` values inline (this is an editorial layout,
  not a rigid grid system), so not every number on the page traces back to a
  token — the scale exists for the rhythm that's genuinely shared across
  sections, not to force everything onto one grid.
- Type roles: `--font-display` (Arial/Helvetica — headlines, labels, UI) and
  `--font-body` (Georgia — leads, paragraphs, quotes)

## Content integrity / NDA notes

- The federal-promo case (case 05) must always describe the RUB 50M figure
  as the **initial tender brief budget**, never as a budget personally
  managed, and the 5-retail-chain rollout as something the original concept
  was **later adapted across** — not something personally executed end to
  end. Both distinctions are already reflected in the current RU/EN copy;
  keep that phrasing if you edit this case.
- No client screenshots or NDA-protected visuals are used anywhere, by
  design — `assets/` is intentionally empty. All case "visuals" are
  generated CSS shapes, not images.
- Brand names in `.brand-wall` are shown separately from the case studies on
  purpose and are not mapped to specific projects anywhere in the markup or
  copy.

## Accessibility

- Keyboard focus is visible on all interactive elements
  (`:focus-visible` outline, inverted to `--paper` on dark sections).
- All decorative visuals (`.case-field` panels, hero artifact cards,
  cursor dot, startup diagram) are `aria-hidden="true"`.
- The language-switch button has a stable `aria-label` and an expanded tap
  target (~44×44px) beyond its small visible text.
- Heading order is `h1` (name) → `h2` (section titles) → `h3` (case
  titles), with no skipped levels.
- `prefers-reduced-motion` and `(hover: none)` are both respected (see
  "How the animation system works" above).

## Responsive coverage

Verified with no horizontal overflow and no clipped/overlapping content at
1920, 1440, 1280, 1024, 900, 768, 600, 430, 390, 375, 360 and 320px, plus the
touch/mobile-specific interaction states (tap targets, hover-only effects
disabled).


## Responsive QA patch
This revision prevents display headings from splitting inside words, constrains the hero cards at narrow widths, replaces the ACE orbital diagram with a typographic gradient panel, and converts the practice flow into a responsive typographic strip.
