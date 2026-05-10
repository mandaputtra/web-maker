# Marketing Design Style Guide

This document defines the brutalist "spec sheet" design language used across the Web Maker website (homepage, docs, footer).

## Aesthetic Direction

**Brutalist / terminal-editorial.** The site looks and feels like a developer's spec sheet or CLI man-page — intentionally raw, typographic, and high-contrast. No gradients, no rounded corners, no blur effects. Sharp edges, offset shadows, mono type, and a dark-first palette.

## Color Palette

| Token          | Hex       | Usage                                                     |
| -------------- | --------- | --------------------------------------------------------- |
| **ink**        | `#0a0a0f` | Primary background, text on light surfaces                |
| **paper**      | `#f4f1ea` | Content areas (docs, cards, testimonials)                 |
| **accent**     | `#ffe300` | CTAs, active states, highlights, tags, eyebrows           |
| **accent-2**   | `#ff5a36` | Secondary accent (error dots, occasional contrast)        |
| **line**       | `#23232c` | Borders, dividers, subtle separators on dark bg           |
| **line-dark**  | `#2a2a33` | Slightly lighter dividers (section separators)            |
| **surface**    | `#111118` | Elevated surfaces on dark bg (cards, inputs, code blocks) |
| **muted**      | `#8a8a96` | Secondary text, metadata, timestamps                      |
| **text-light** | `#b9b9c4` | Body text on dark backgrounds                             |

### Rules

- Dark sections (`#0a0a0f`) use a subtle **dotted grid texture**: `radial-gradient(circle at 1px 1px, rgba(255,255,255,0.06) 1px, transparent 0)` at `22px 22px`.
- Light/paper sections (`#f4f1ea`) may use a subtle **diagonal hairline pattern**: `repeating-linear-gradient(-45deg, transparent 0 18px, rgba(10,10,15,0.025) 18px 19px)`.
- Never use purple, blue, or orange (these were the old brand colors).

## Typography

### Fonts

| Font               | Weight             | Role                                                                |
| ------------------ | ------------------ | ------------------------------------------------------------------- |
| **JetBrains Mono** | 400, 500, 700, 800 | Display headings, labels, eyebrows, nav, code, card titles, buttons |
| **IBM Plex Sans**  | 400, 500           | Body copy, descriptions, longer paragraphs                          |

Both loaded via Google Fonts:

```
https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700;800&family=IBM+Plex+Sans:wght@400;500&display=swap
```

### Rules

- Headings: JetBrains Mono, weight 700–800, tight letter-spacing (`-0.02em` to `-0.04em`), tight line-height (`0.86`–`0.95`).
- Section titles use the **blinking caret** pattern: `<em>word</em>` where `em` is `font-style: normal; color: var(--accent)` and `em::after { content: '_'; animation: blink 1.1s steps(1) infinite; }`.
- Body: IBM Plex Sans, weight 400, `line-height: 1.55`.
- Labels/eyebrows: JetBrains Mono, `0.72rem–0.78rem`, uppercase, `letter-spacing: 0.12em–0.18em`.
- Never use Inter, Roboto, Arial, Space Grotesk, or system-ui as visible display fonts.

## Section Anatomy

Every major section follows the same header structure (class prefix `bb` for "brutalist block"):

```
.bb                       — full-bleed wrapper (dark or paper bg)
  .bb__head               — flex header spanning full width
    .bb__eyebrow          — mono uppercase label ("SPEC SHEET // v7.1")
      ::before            — small colored square (10x10, glowing on dark)
    .bb__title            — large mono heading with <em> for accent + caret
    .bb__meta             — right-aligned mono metadata ("$ command" style)
  [section content]       — grid, table, cards, etc.
```

### Eyebrow format

```
CATEGORY // count-or-version
```

Examples: `SPEC SHEET // v7.1`, `USE CASES // 04`, `DIFF // webmaker vs cloud`, `RECEIPTS // 11`.

### Meta format (right side)

```
description line
$ command arg
```

Examples: `$ open webmaker.app`, `$ git diff webmaker..cloud`, `$ cat testimonials.log`.

## Shadows & Hover

All interactive elements use **hard offset shadows** (no blur, no spread):

```css
box-shadow: 6px 6px 0 #ffe300; /* default */
box-shadow: 4px 4px 0 #0a0a0f; /* on light surfaces */
```

### Hover pattern

```css
.element:hover {
	transform: translate(-2px, -2px); /* lift up-left */
	box-shadow: 8px 8px 0 <color>; /* shadow grows */
}
```

Transition: `0.3s cubic-bezier(0.2, 0.8, 0.2, 1)` on transform + box-shadow.

No blur shadows, no glows (except the eyebrow dot `box-shadow: 0 0 12px`), no elevation shadows.

## Borders

- Always `1px solid` — no thicker borders except the post-title underline (`3px solid #ffe300`).
- Dark bg: border color `#23232c` or `#2a2a33`.
- Light bg: border color `#0a0a0f`.
- Dashed borders (`1px dashed`) for inner card separators and section dividers on paper surfaces.
- `border-radius: 0` everywhere. No rounded corners.

## Buttons

Two variants:

### Primary (yellow)

```css
background: #ffe300;
color: #0a0a0f;
border: 1px solid #ffe300;
box-shadow: 6px 6px 0 #f4f1ea;
font-family: 'JetBrains Mono';
font-weight: 700;
```

### Ghost (outlined)

```css
background: transparent;
color: #f4f1ea;
border: 1px solid #2c2c36;
box-shadow: 6px 6px 0 #2c2c36;
```

Hover: border turns yellow, text turns yellow, shadow becomes `#ffe300`.

## Cards (spec cards, testimonial cards)

- Background: `#f4f1ea` (paper) on dark sections, `#111118` (surface) for alternate dark cards.
- Border: `1px solid #0a0a0f` (paper cards) or `1px solid #23232c` (dark cards).
- Shadow: `6px 6px 0 #ffe300`.
- Hover: translate + shadow growth + optional bg flip to `#ffe300`.
- Numbered badges: JetBrains Mono, `0.62rem–0.72rem`, positioned top-right as dark chip (`bg: #0a0a0f; color: #ffe300`).
- CLI-style flags on spec cards: `--offline`, `--cdn`, `--lang=*` in accent color.

## Code & Technical Elements

- Inline `code`: `background: #0a0a0f; color: #ffe300; padding: 0.15em 0.4em; border-radius: 0`.
- Code blocks: `background: #0a0a0f; border: 1px solid #23232c; font-family: JetBrains Mono; font-size: 0.88rem`.
- Diff-style tables: `+` prefix in yellow, `-` prefix in `#ff5a36`, dimension labels prefixed with `#`.
- Terminal window frames: traffic-light dots (red `#ff5a36`, yellow `#ffe300`, grey `#b9b9c4`), mono breadcrumb.

## Docs Content Area

- Background: `#f4f1ea` (paper).
- Post title: JetBrains Mono 800, `border-bottom: 3px solid #ffe300`, inline-block.
- H2: JetBrains Mono 700, `border-bottom: 1px dashed #0a0a0f`.
- H3: JetBrains Mono 700.
- Body: IBM Plex Sans 400.
- Links: `color: #0a0a0f; text-decoration-color: #ffe300; text-decoration-thickness: 2px`. Hover inverts to yellow-on-dark.
- Blockquotes: `border-left: 3px solid #ffe300; background: rgba(255,227,0,0.06)`.
- Tables: dark header row (`bg: #0a0a0f; color: #ffe300`), `1px solid #0a0a0f` borders.
- Pagination: offset-shadow buttons with brutalist hover.

## Animation

Minimal. Only used for:

1. **Blinking caret** on section titles: `animation: blink 1.1s steps(1) infinite`.
2. **Levitate** on hero screenshot: `4s ease-in-out infinite`, 12px vertical bob. The "Open Source" badge levitates on the same cycle but offset by `-1.5s`.
3. **Ticker marquee**: `animation: ticker-scroll 20s linear infinite`.
4. **Hover transitions**: `0.25s–0.5s` on transform, box-shadow, background, color.

No spring physics, no bounce, no parallax, no scroll-triggered animations.

## Responsive Breakpoints

| Breakpoint | Behavior                                                  |
| ---------- | --------------------------------------------------------- |
| `> 880px`  | Full layout — 2-col hero, 6-col spec grid, 3-col grid     |
| `<= 880px` | Hero stacks to 1-col, grids collapse, 3D rotation removed |
| `<= 760px` | Docs sidebar unsticks and stacks above content            |
| `<= 560px` | Testimonial cards single column                           |
| `<= 480px` | Hero CTAs stack vertically, tags shrink                   |

## File Locations

| What                                      | File                                                                                                          |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Hero section + ticker                     | `packages/website/_includes/default.html` (inline `<style>` + markup in `{% if page.fileSlug == '' %}` block) |
| Docs layout + sidebar + TOC               | `packages/website/_includes/default.html` (main `<style>` block) + `packages/website/_includes/doc.html`      |
| Spec sheet, use cases, diff, testimonials | `packages/website/index.html` (inline `<style>` blocks per section)                                           |
| Footer                                    | `packages/website/_includes/footer.html` (markup + scoped `<style>`)                                          |

## Don'ts

- No rounded corners (`border-radius: 0` always).
- No gradient backgrounds (solid colors or dotted textures only).
- No blur/glow shadows (hard offset only).
- No emojis in the UI (use SVG icons).
- No orange (`#ff6c00` was the old accent — replaced by `#ffe300`).
- No purple gradient backgrounds (was the old hero/docs bg).
- No generic fonts (Inter, Roboto, Arial).
- No CSS modules or component-scoped styles — all styles are inline in their respective HTML files.
