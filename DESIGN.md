---
version: alpha
name: Web Maker — Brutalist Spec Sheet
description: Terminal-editorial design system for the Web Maker website. Dark-first, monospace, high-contrast. Hard offset shadows replace elevation. Zero rounded corners. Reads like a developer's spec sheet or CLI man-page.

colors:
  ink: '#0a0a0f'
  paper: '#f4f1ea'
  accent: '#ffe300'
  accent-2: '#ff5a36'
  surface: '#111118'
  line: '#23232c'
  line-dark: '#2a2a33'
  muted: '#8a8a96'
  text-light: '#b9b9c4'

typography:
  headline-display:
    fontFamily: JetBrains Mono
    fontSize: 4.4rem
    fontWeight: 800
    lineHeight: 0.9
    letterSpacing: -0.04em
  headline-lg:
    fontFamily: JetBrains Mono
    fontSize: 3.6rem
    fontWeight: 800
    lineHeight: 0.95
    letterSpacing: -0.03em
  headline-md:
    fontFamily: JetBrains Mono
    fontSize: 1.95rem
    fontWeight: 800
    lineHeight: 1.15
    letterSpacing: -0.02em
  headline-sm:
    fontFamily: JetBrains Mono
    fontSize: 1.18rem
    fontWeight: 700
    lineHeight: 1.18
    letterSpacing: -0.02em
  body-lg:
    fontFamily: IBM Plex Sans
    fontSize: 1.18rem
    fontWeight: 400
    lineHeight: 1.55
  body-md:
    fontFamily: IBM Plex Sans
    fontSize: 1.02rem
    fontWeight: 400
    lineHeight: 1.65
  body-sm:
    fontFamily: IBM Plex Sans
    fontSize: 0.92rem
    fontWeight: 400
    lineHeight: 1.55
  label-md:
    fontFamily: JetBrains Mono
    fontSize: 0.78rem
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.18em
  label-sm:
    fontFamily: JetBrains Mono
    fontSize: 0.72rem
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.12em
  code-inline:
    fontFamily: JetBrains Mono
    fontSize: 0.88em
    fontWeight: 500
    lineHeight: 1.4
  code-block:
    fontFamily: JetBrains Mono
    fontSize: 0.88rem
    fontWeight: 400
    lineHeight: 1.55

rounded:
  none: 0px

spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
  2xl: 64px
  hairline: 1px
  dot-grid: 22px
  hairline-stripe: 18px
  shadow-offset: 6px
  shadow-offset-hover: 8px
  shadow-offset-lift: 2px

components:
  button-primary:
    backgroundColor: '{colors.accent}'
    textColor: '{colors.ink}'
    typography: '{typography.label-md}'
    rounded: '{rounded.none}'
    padding: 0.95rem 1.4rem
    boxShadow: 6px 6px 0 {colors.paper}
  button-primary-hover:
    boxShadow: 9px 9px 0 {colors.paper}
    transform: translate(-3px, -3px)
  button-primary-on-paper:
    backgroundColor: '{colors.accent}'
    textColor: '{colors.ink}'
    typography: '{typography.label-md}'
    rounded: '{rounded.none}'
    padding: 0.95rem 1.4rem
    boxShadow: 6px 6px 0 {colors.ink}
  button-ghost:
    backgroundColor: transparent
    textColor: '{colors.paper}'
    typography: '{typography.label-md}'
    rounded: '{rounded.none}'
    padding: 0.95rem 1.4rem
    borderColor: '{colors.line-dark}'
    boxShadow: 6px 6px 0 {colors.line-dark}
  button-ghost-hover:
    textColor: '{colors.accent}'
    borderColor: '{colors.accent}'
    boxShadow: 9px 9px 0 {colors.accent}
    transform: translate(-3px, -3px)
  button-dark:
    backgroundColor: '{colors.ink}'
    textColor: '{colors.accent}'
    typography: '{typography.label-md}'
    rounded: '{rounded.none}'
    padding: 0.95rem 1.4rem
    boxShadow: 6px 6px 0 {colors.ink}
  card-paper:
    backgroundColor: '{colors.paper}'
    textColor: '{colors.ink}'
    borderColor: '{colors.ink}'
    rounded: '{rounded.none}'
    padding: 24px
    boxShadow: 6px 6px 0 {colors.accent}
  card-paper-hover:
    boxShadow: 8px 8px 0 {colors.accent}
    transform: translate(-2px, -2px)
  card-dark:
    backgroundColor: '{colors.surface}'
    textColor: '{colors.paper}'
    borderColor: '{colors.line}'
    rounded: '{rounded.none}'
    padding: 24px
    boxShadow: 6px 6px 0 {colors.accent}
  spec-card-badge:
    backgroundColor: '{colors.ink}'
    textColor: '{colors.accent}'
    typography: '{typography.label-sm}'
    padding: 0.15em 0.4em
    rounded: '{rounded.none}'
  code-inline:
    backgroundColor: '{colors.ink}'
    textColor: '{colors.accent}'
    typography: '{typography.code-inline}'
    padding: 0.15em 0.4em
    rounded: '{rounded.none}'
  code-block:
    backgroundColor: '{colors.ink}'
    textColor: '{colors.paper}'
    borderColor: '{colors.line}'
    typography: '{typography.code-block}'
    padding: 16px 20px
    rounded: '{rounded.none}'
  table-header:
    backgroundColor: '{colors.ink}'
    textColor: '{colors.accent}'
    typography: '{typography.label-sm}'
  eyebrow:
    textColor: '{colors.accent}'
    typography: '{typography.label-md}'
  eyebrow-dot:
    backgroundColor: '{colors.accent}'
    size: 10px
    rounded: '{rounded.none}'
    boxShadow: 0 0 12px {colors.accent}
  blockquote:
    backgroundColor: 'rgba(255,227,0,0.06)'
    textColor: '{colors.ink}'
    borderColor: '{colors.accent}'
    typography: '{typography.body-md}'
    padding: 16px 20px
---

# Web Maker — Brutalist Spec Sheet

## Overview

Web Maker's marketing surface looks and feels like a developer's spec sheet or a CLI man-page. The aesthetic direction is **brutalist / terminal-editorial**: intentionally raw, typographic, high-contrast. The product is a code playground — the site should feel like the same kind of tool: dense with information, mono-typed, with sharp edges and command-line tics.

The system has three load-bearing decisions:

1. **Dark-first.** The primary canvas is near-black ink (`#0a0a0f`), broken by an off-white paper (`#f4f1ea`) for content-heavy surfaces. Yellow (`#ffe300`) is the single driver of action and emphasis.
2. **Mono-typed.** JetBrains Mono carries every display, label, button, eyebrow and code element. IBM Plex Sans handles long-form body copy. Nothing else.
3. **Offset shadows, not elevation.** Depth is conveyed by hard offset shadows (e.g. `6px 6px 0 #ffe300`). No blur, no spread, no glow. The page reads as flat planes intersecting, not floating cards.

The voice is curt and technical. Section headers follow a CLI convention: an eyebrow label like `SPEC SHEET // v7.1`, a meta line like `$ open webmaker.app`, and titles ending in a blinking yellow caret (`_`).

## Colors

The palette is built around two grounding neutrals and one driving accent. Every other token is a support role.

- **Ink (#0a0a0f):** Near-black. Primary background on hero, diff, FAQ, and final-CTA surfaces. Also the text color on light surfaces. Used as a hard outline (`1px solid`) on paper-bg cards.
- **Paper (#f4f1ea):** A warm off-white. The canvas for docs, content areas, spec/testimonial cards on dark sections, and the primary-CTA offset shadow color.
- **Accent (#ffe300):** Vivid web-safe yellow. The single driver of interaction and emphasis — used for CTAs, active states, highlights, eyebrow dots, tags, the blinking title caret, and `<em>` accents. Never used as a body color.
- **Accent-2 (#ff5a36):** Red-orange. The negative counterpart to yellow. Used only for: the leftmost terminal-traffic-light dot, `-` rows in diff tables, error indicators, and the offset shadow on the "competitor" card in comparison pages. Never used at body scale.
- **Surface (#111118):** Elevated surface on dark bg — cards, inputs, code blocks. One step lighter than ink so a card visibly sits on top of the page.
- **Line (#23232c):** Dividers, borders, subtle separators on dark backgrounds.
- **Line-dark (#2a2a33):** A half-step lighter than line. Used for section-to-section separators on dark, where a slightly more visible boundary helps.
- **Muted (#8a8a96):** Secondary text on dark — metadata, timestamps, eyebrow meta.
- **Text-light (#b9b9c4):** Body text on dark backgrounds. The default for paragraphs inside a dark section.

### Banned colors

Purple, blue, and orange (`#ff6c00`) were the previous brand palette and must not appear. Pure white is also avoided — content surfaces use paper, not `#fff`.

## Typography

Two fonts. No third option.

- **JetBrains Mono** carries every display, heading, label, button, eyebrow, card title, nav link, and code element. Weights 400, 500, 700, 800. Headings use 700–800 with tight letter-spacing (`-0.02em` to `-0.04em`) and tight line-height (`0.86`–`0.95`).
- **IBM Plex Sans** carries long-form body copy. Weights 400 and 500. Line-height 1.55–1.65.

Both load from Google Fonts with a single request:

```
https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700;800&family=IBM+Plex+Sans:wght@400;500&display=swap
```

### Heading conventions

- Headings always set in JetBrains Mono. Never IBM Plex Sans.
- Section titles use the **blinking caret** pattern: `<em>word</em>` where `em` is rendered as `font-style: normal; color: var(--accent)` and `em::after { content: '_'; animation: blink 1.1s steps(1) infinite; }`. The caret blinks 24/7 — it is the system's signature.
- H2 in long-form content gets a `border-bottom: 1px dashed #0a0a0f`.
- H3 in long-form content is prefixed with a `# ` chip styled as dark+yellow code-style label.

### Eyebrows and labels

- Eyebrow: JetBrains Mono, 0.72rem–0.78rem, uppercase, letter-spacing `0.12em`–`0.18em`. Preceded by a 10×10 colored square (`::before`). On dark backgrounds the square glows (`box-shadow: 0 0 12px <accent>`).
- Eyebrow text format: `CATEGORY // count-or-version`. Examples: `SPEC SHEET // v7.1`, `USE CASES // 04`, `DIFF // webmaker vs cloud`, `RECEIPTS // 11`, `PRO // unlock`.
- Tag chips: `5px 10px` padding, `1px solid` border in muted yellow or ink, mono uppercase.

### Banned typefaces

Inter, Roboto, Arial, Space Grotesk, and `system-ui` are never used as visible display fonts. They appear only in fallback stacks.

## Layout

The page is a vertical stack of full-bleed sections. Each section follows the same anatomy (the `.bb` "brutalist block" pattern):

```
.bb                       — full-bleed wrapper (dark or paper bg)
  .bb__head               — flex header spanning full width
    .bb__eyebrow          — mono uppercase label with colored dot
    .bb__title            — large mono heading with <em> for accent + caret
    .bb__meta             — right-aligned mono metadata, two lines:
                              line 1: description ("3 connected", "v7.1")
                              line 2: $ command arg
  [section content]       — grid, table, cards, or split layout
```

Sections alternate between dark (`#0a0a0f` with the dotted-grid texture) and paper (`#f4f1ea` with the diagonal hairline texture) for visual rhythm. Yellow (`#ffe300`) is reserved for short accent strips — the final-CTA band, occasional callouts.

### Section padding

Sections use fluid clamps for breathing room without breakpoints:

```
padding: clamp(2.5rem, 6vw, 5rem) clamp(1rem, 4vw, 3rem);
padding-left: max(1rem, calc(50vw - 600px));
padding-right: max(1rem, calc(50vw - 600px));
```

The horizontal padding clamp creates a soft ~1200px max content width without requiring a wrapping `max-width` element — the section is genuinely full-bleed.

### Textures

- **Dark sections** carry a subtle dotted grid: `radial-gradient(circle at 1px 1px, rgba(255,255,255,0.06) 1px, transparent 0)` at `22px 22px`. The grid is barely visible but signals "terminal grid paper" to the eye.
- **Paper sections** may carry a subtle diagonal hairline: `repeating-linear-gradient(-45deg, transparent 0 18px, rgba(10,10,15,0.025) 18px 19px)`. Same role — adds tooth without color.

### Grids

- Hero / split layouts: 2-column grid with a clamp gap (`clamp(1.5rem, 4vw, 4rem)`).
- Spec / benefit grids: 2×2 or 2×3 with **hard-edged cells** separated by `1px dashed` lines, not gutters. The grid reads as a single object.
- Diff table: 3-column grid (dimension / + PRO / − FREE) with `1px solid` row dividers.

### Responsive breakpoints

- **> 880px:** Full layout — 2-col hero, multi-col spec grids.
- **≤ 880px:** Hero stacks to 1-col, grids collapse to single column, any 3D rotation removed.
- **≤ 760px:** Docs sidebar unsticks and stacks above content. Diff tables collapse to single column with `+ PRO`/`- FREE` prefixes inlined.
- **≤ 560px:** Testimonial cards single column.
- **≤ 480px:** Hero CTAs stack vertically and span full width.

## Elevation & Depth

Depth is conveyed entirely through **hard offset shadows**. There is no `filter: blur()`, no elevation hierarchy à la Material, no soft glows.

The single shadow primitive:

```css
box-shadow: 6px 6px 0 <color>;
```

Where `<color>` is:

- `#ffe300` (accent) — the default for cards and CTAs on dark backgrounds.
- `#f4f1ea` (paper) — the default for primary CTAs (yellow button) so the shadow contrasts with the dark canvas.
- `#0a0a0f` (ink) — the default for cards and buttons on light/paper surfaces.
- `#ff5a36` (accent-2) — the offset shadow for the "competitor" card in comparison pages.

### Hover pattern

Interactive elements lift up-and-to-the-left and grow their shadow:

```css
.element:hover {
	transform: translate(-2px, -2px);
	box-shadow: 8px 8px 0 <color>;
}
```

Transition: `0.25s–0.3s cubic-bezier(0.2, 0.8, 0.2, 1)` on transform + box-shadow + background + color.

The CTA buttons use a slightly larger lift (`-3px, -3px`) with a `9px 9px 0` shadow.

### Glows

The only blurred-shadow exception is the eyebrow dot on dark sections, which carries a faint glow:

```css
.bb__eyebrow::before {
	background: #ffe300;
	box-shadow: 0 0 12px #ffe300;
}
```

This is the single sanctioned glow in the system.

## Shapes

Sharp edges, everywhere. `border-radius: 0` is the universal rule.

The only intentional curve in the entire system is the small filled circle used for traffic-light dots in terminal-window mocks (`width: 11px; height: 11px; border-radius: 50%`), and the tiny circles used as `::marker` punctuation on list items. Every container, button, card, input, table cell, code block, and tag is rectangular.

### Borders

- All borders are `1px solid` — no thicker borders except the docs post-title underline (`3px solid #ffe300`).
- On dark backgrounds: borders use `#23232c` (line) or `#2a2a33` (line-dark) for section breaks.
- On paper backgrounds: borders use `#0a0a0f` (ink).
- **Dashed `1px dashed`** is reserved for inner separators inside a grid (cell-to-cell) and the dashed underline on H2 inside long-form content.
- The post-title underline at `3px solid #ffe300` is the single thick-border exception.

## Components

### Buttons

Two primary variants. Both share JetBrains Mono weight 700, all-uppercase label text, and a hard offset shadow.

**Primary (yellow on dark, or yellow on paper):**

```css
background: #ffe300;
color: #0a0a0f;
border: 1px solid #ffe300;
box-shadow: 6px 6px 0 #f4f1ea; /* on dark bg */
/* box-shadow: 6px 6px 0 #0a0a0f;     on paper bg */
font-family: 'JetBrains Mono';
font-weight: 700;
padding: 0.95rem 1.4rem;
```

**Ghost (outlined):**

```css
background: transparent;
color: #f4f1ea;
border: 1px solid #2c2c36;
box-shadow: 6px 6px 0 #2c2c36;
```

Hover: border turns yellow, text turns yellow, shadow becomes `#ffe300`.

**Dark (inverse — used on yellow CTA strips):** Black background, yellow text, ink offset shadow.

### Spec / benefit cards

- Paper bg (`#f4f1ea`) on dark sections, surface bg (`#111118`) for alternate dark cards.
- Border: `1px solid #0a0a0f` (paper cards) or `1px solid #23232c` (dark cards).
- Shadow: `6px 6px 0 #ffe300`.
- Hover: translate `-2px, -2px` + shadow grows to `8px 8px 0`. Sometimes a bg flip to `#0a0a0f` with yellow text — the "ink invert" pattern used on use-case benefits.
- Numbered badges live top-right as a dark chip: `background: #0a0a0f; color: #ffe300`, JetBrains Mono `0.62rem`–`0.72rem`.
- CLI-style flags on spec cards (`--offline`, `--cdn`, `--lang=*`) render in accent color, mono lowercase, with light tracking.

### Code & technical elements

- **Inline `code`**: `background: #0a0a0f; color: #ffe300; padding: 0.15em 0.4em; border-radius: 0;` JetBrains Mono.
- **Code blocks**: `background: #0a0a0f; border: 1px solid #23232c; font-family: JetBrains Mono; font-size: 0.88rem; line-height: 1.55`. Same on dark and paper backgrounds — code blocks are always dark.
- **Diff-style tables**: `+` prefix rendered in yellow, `-` prefix in `#ff5a36`, dimension labels prefixed with `#`. The first row is a header row in lighter background with uppercase mono labels.
- **Terminal window frames**: Three traffic-light dots (red `#ff5a36`, yellow `#ffe300`, grey `#b9b9c4`), followed by a mono breadcrumb like `~/webmaker · untitled.html`. The frame body uses `surface` color with the `line` border.

### Long-form content (docs and marketing markdown)

The same paper canvas, but with stricter typography:

- Post title: JetBrains Mono 800, `border-bottom: 3px solid #ffe300`, inline-block.
- H2: JetBrains Mono 700, `border-bottom: 1px dashed #0a0a0f`.
- H3: JetBrains Mono 700, prefixed with a `# ` chip (dark+yellow).
- Body: IBM Plex Sans 400, line-height `1.65`.
- Links: `color: #0a0a0f; text-decoration-color: #ffe300; text-decoration-thickness: 2px`. Hover inverts to yellow-on-dark.
- Blockquotes: `border-left: 3px solid #ffe300; background: rgba(255,227,0,0.06)`.
- Tables: dark header row (`bg: #0a0a0f; color: #ffe300`), `1px solid #0a0a0f` borders, no zebra striping.
- Pagination: offset-shadow buttons with the standard brutalist hover.

### Pros / Cons / Verdict (comparison pages)

- `.pros`: paper bg, `border-left: 4px solid #ffe300`, `6px 6px 0 #ffe300` shadow. Heading prefixed with a dark `+ WINS` chip.
- `.cons`: paper bg, `border-left: 4px solid #ff5a36`, `6px 6px 0 #ff5a36` shadow. Heading prefixed with a dark `- TRADEOFFS` chip.
- `.verdict-box`: paper card with a dark mono title bar (`VERDICT $ verdict`) and `10px 10px 0 #0a0a0f` shadow.

## Do's and Don'ts

### Do

- Use the blinking yellow caret on every section title — it is the system's signature animation.
- Lead every section with the eyebrow/title/meta triplet so the page reads as a stack of CLI receipts.
- Pair the dotted-grid texture (dark) and the diagonal-hairline texture (paper) to add tooth without color.
- Always pair an offset shadow with a CTA or card. A flat button with no shadow looks broken.
- Use `1px dashed` for _inner_ dividers and `1px solid` for _outer_ boundaries — the distinction reads.
- Prefer `box-shadow: 6px 6px 0` on default state and `box-shadow: 8px 8px 0` on hover, with a `translate(-2px, -2px)` lift.
- Use SVG icons exclusively. No emoji in UI.

### Don't

- No rounded corners. `border-radius: 0` everywhere except the literal traffic-light dots.
- No gradient backgrounds. Solid colors or the two documented textures only.
- No blur shadows, no glows. (Except the single sanctioned eyebrow-dot glow.)
- No emoji in UI. Use SVG icons.
- No orange (`#ff6c00` was the old accent — replaced by `#ffe300`).
- No purple gradient backgrounds (was the old hero/docs bg).
- No generic fonts (Inter, Roboto, Arial, Space Grotesk) as visible display type.
- No CSS modules or component-scoped styles. Site styles live inline in their respective HTML files.
- No spring physics, no bounce, no parallax, no scroll-triggered animation. The only animations are the blinking caret, the levitate on hero screenshots, the ticker marquee, and hover transitions.
