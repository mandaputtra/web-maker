---
title: 'Build a tooltip with zero JavaScript using CSS Anchor Positioning'
date: 2026-04-27
description: 'Replace Popper.js, Floating UI, and 11KB of runtime with three CSS properties. A complete, accessible tooltip in pure CSS using anchor-name, position-anchor, and position-try.'
---

The tooltip-positioning problem has powered an entire category of JavaScript libraries. Popper.js, Floating UI, Tippy.js — each one wraps the same core idea: "given an anchor element, place this floating element nearby, flip it if it overflows, hide it if the anchor scrolls offscreen."

CSS Anchor Positioning lands all of that as native primitives. Chrome and Edge have it stable, Safari is shipping behind a flag in TP, and the spec is on the W3C track for full Interop 2026.

This post builds a complete tooltip — accessible, flippable, scroll-aware — in pure CSS.

> Fork along: [Web Maker — anchor tooltip demo](https://webmaker.app/create/IcayNZlKko).

## The three properties you actually need

1. `anchor-name: --my-button` — gives an element an addressable name.
2. `position-anchor: --my-button` — declares which anchor a positioned element targets.
3. `anchor()` — a function that resolves to one of the anchor's edges (`top`, `bottom`, `start`, etc.) usable inside `top`, `left`, `inset`, etc.

That's the core. A fourth — `position-try` — handles overflow flips.

## The minimum viable tooltip

```html
<button class="trigger" popovertarget="tip">Hover or focus me</button>
<div id="tip" class="tooltip" popover>Anchor positioning, no JS.</div>
```

```css
.trigger {
	anchor-name: --trigger;
}

.tooltip {
	position-anchor: --trigger;
	position: fixed;
	top: anchor(bottom);
	left: anchor(center);
	translate: -50% 8px;

	background: #0a0a0f;
	color: #ffe300;
	padding: 0.4rem 0.75rem;
	border: 1px solid #2a2a33;
	font-family: 'JetBrains Mono', monospace;
	font-size: 0.85rem;
}
```

`anchor(bottom)` resolves to the trigger's bottom edge. `anchor(center)` resolves to the horizontal centre. We translate `-50%` to center the tooltip on that point and add 8px of clearance. That's it for placement.

We're using the [Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API) for the show/hide behaviour — that's also native, also no JS.

## Add the flip behaviour

A tooltip near the bottom of the viewport should flip up. `position-try-fallbacks` lets the browser try alternate positions in order until one fits.

```css
.tooltip {
	position-anchor: --trigger;
	position: fixed;
	top: anchor(bottom);
	left: anchor(center);
	translate: -50% 8px;
	position-try-fallbacks: --above, --to-right;
}

@position-try --above {
	top: auto;
	bottom: anchor(top);
	translate: -50% -8px;
}

@position-try --to-right {
	top: anchor(center);
	left: anchor(right);
	translate: 8px -50%;
}
```

The browser tries the default placement first. If the tooltip overflows, it tries `--above`. If that also overflows, it tries `--to-right`. If everything fails, it falls back to the last attempted position.

This is the exact algorithm Popper.js implements, in three CSS rules.

## Add a tail (the little arrow)

```css
.tooltip::before {
	content: '';
	position: absolute;
	top: -6px;
	left: 50%;
	translate: -50% 0;
	width: 12px;
	height: 12px;
	background: #0a0a0f;
	border-left: 1px solid #2a2a33;
	border-top: 1px solid #2a2a33;
	rotate: 45deg;
}
```

Static for now. If you want the arrow to flip with the tooltip, target a `position-fallback` selector and override the tail position. The pattern is the same as the parent flip.

## Accessibility: don't skip this

A tooltip that isn't keyboard-focusable or doesn't announce to screen readers is a worse tooltip than no tooltip.

- Use `popovertarget` and `popover` so the browser handles open/close + focus management.
- Prefer `aria-describedby="tip"` on the trigger over `aria-label` on the tooltip — describedby announces in context.
- Don't put critical information in tooltips. Mobile users tap-to-open and frequently miss them.

```html
<button class="trigger" popovertarget="tip" aria-describedby="tip">Save</button>
<div id="tip" class="tooltip" popover>Saves locally, syncs when online.</div>
```

## Replacing a real Popper setup

Old code (typical Popper integration):

```js
import { createPopper } from '@popperjs/core';
const popper = createPopper(trigger, tooltip, {
	placement: 'bottom',
	modifiers: [
		{ name: 'flip', options: { fallbackPlacements: ['top', 'right'] } }
	]
});
trigger.addEventListener('mouseenter', () => (tooltip.dataset.show = 'true'));
trigger.addEventListener('mouseleave', () => delete tooltip.dataset.show);
```

11KB minified, plus your event-listener boilerplate, plus a teardown step on unmount.

New code: zero JavaScript, zero bytes of runtime. The browser handles scroll updates, viewport resizes, ancestor transforms.

## Browser support (April 2026)

| Browser | Status                            |
| ------- | --------------------------------- |
| Chrome  | Stable since 125                  |
| Edge    | Stable since 125                  |
| Safari  | Behind flag in Technology Preview |
| Firefox | In progress                       |

For Safari/Firefox holdouts, ship Floating UI as a progressive-enhancement fallback only when `CSS.supports('anchor-name: --x')` is false. You'll delete the polyfill in 6 months.

## Try it

> [Open the anchor-tooltip demo on Web Maker](https://webmaker.app/create/IcayNZlKko) — fork it, retarget it to your own components, ship.

Next post in this series: [scroll-driven animations](https://webmaker.app/blog/css-only-scroll-progress-bar-2026).
