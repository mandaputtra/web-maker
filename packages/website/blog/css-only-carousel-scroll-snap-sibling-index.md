---
title: 'A CSS-only carousel with scroll-snap and sibling-index()'
date: 2026-05-03
description: 'Build a complete carousel — snapping track, native pagination dots, staggered entry animations — with zero JavaScript. Uses scroll-snap, ::scroll-marker, and the new sibling-index() function.'
ogImage: /images/blog/og-css-carousel.jpg
---

Carousels are a staple of "how much can you do without JavaScript" challenges. Until 2026 the answer was "almost everything except the dots." `scroll-snap` handled the snap-to-card behaviour. CSS could draw the dots. But updating the active dot as you scrolled? That needed a few lines of JS.

Two new features close the gap: `::scroll-marker` (a pseudo-element automatically created for each scroll-snap target) and `sibling-index()` (a CSS function returning a child's position among its siblings, usable inside any value).

This post builds a complete carousel — snapping track, animated active dot, staggered entry — entirely in CSS.

> Fork along: [Web Maker — CSS-only carousel](https://webmaker.app/create/eBbjNxrM8O?layout=1).

## The markup

```html
<div class="carousel">
	<div class="card">1</div>
	<div class="card">2</div>
	<div class="card">3</div>
	<div class="card">4</div>
	<div class="card">5</div>
	<div class="card">6</div>
	<div class="card">7</div>
	<div class="card">8</div>
</div>
```

That's it. No nav, no buttons, no `<input type="radio">` hacks. Eight cards in a div.

## The snapping track

```css
.carousel {
	display: flex;
	gap: 1rem;
	overflow-x: auto;
	scroll-snap-type: x mandatory;
	scroll-marker-group: after; /* dots appear after the track */
	padding: 1rem;
}

.card {
	flex: 0 0 80%;
	height: 240px;
	scroll-snap-align: center;
	scroll-snap-stop: always;
	background: #ffe300;
	border: 1px solid #0a0a0f;
	display: grid;
	place-items: center;
	font: 700 3rem 'JetBrains Mono';
}
```

`scroll-snap-type: x mandatory` makes horizontal scrolling lock to snap points. `scroll-snap-align: center` makes each card the snap target. Drag the track, release, the nearest card snaps centred.

`scroll-marker-group: after` is the new bit. It tells the browser to generate a marker container _after_ the carousel and populate it with one `::scroll-marker` per snap target.

## The dots

```css
.card::scroll-marker {
	content: '';
	width: 10px;
	height: 10px;
	border: 1px solid #0a0a0f;
	background: transparent;
	margin: 0 4px;
}

.card::scroll-marker:target-current {
	background: #ffe300;
}
```

`::scroll-marker` is a real pseudo-element with `content`, `display`, layout. Each card's marker shows up as a tiny square in the marker group.

`:target-current` is the new pseudo-class that matches the marker for the currently-snapped card. As you scroll, the browser updates which marker matches `:target-current`. Active-state styling, zero JavaScript.

You can even click a dot to scroll to its card — that's free.

## Staggered entry animation

This is where `sibling-index()` shows up. Each card should animate in with a delay based on its position.

```css
.card {
	animation: card-in 600ms ease-out backwards;
	animation-delay: calc(sibling-index() * 80ms);
}

@keyframes card-in {
	from {
		opacity: 0;
		transform: translateY(20px);
	}
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

`sibling-index()` returns 1 for the first sibling, 2 for the second, and so on. Multiply by 80ms and the cascade staggers automatically. Add or remove a card — the timing adjusts itself, no JavaScript, no manual `--i` custom properties on each element.

Pre-`sibling-index()`, you would have written:

```html
<div class="card" style="--i: 1">...</div>
<div class="card" style="--i: 2">...</div>
<!-- ...etc -->
```

```css
animation-delay: calc(var(--i) * 80ms);
```

Doable, but every render had to set `--i`. Now the CSS engine does it.

## Reverse staggers and conditional staggers

`sibling-index()` is a regular value function. Combine it with `if()` (covered [in Day 1](https://webmaker.app/blog/css-if-function-tutorial)) or `calc()` for any pattern:

```css
/* reverse stagger */
animation-delay: calc((sibling-count() - sibling-index()) * 80ms);

/* odd cards drift left, even cards drift right */
@keyframes card-in {
	from {
		opacity: 0;
		transform: translateX(if(style(--side: left): -20px; else: 20px));
	}
	to {
		opacity: 1;
		transform: translateX(0);
	}
}
```

`sibling-count()` is the partner function — total siblings under the same parent.

## The reduce-motion variant

```css
@media (prefers-reduced-motion: reduce) {
	.card {
		animation: none;
		opacity: 1;
		transform: none;
	}
	.carousel {
		scroll-snap-type: none;
	}
}
```

Disable both the entry animation _and_ the snap behaviour — some vestibular conditions react to snap-induced velocity changes.

## Browser support (April 2026)

| Feature           | Chrome | Edge | Safari | Firefox     |
| ----------------- | ------ | ---- | ------ | ----------- |
| `scroll-snap`     | 75+    | 79+  | 11+    | 99+         |
| `::scroll-marker` | 130+   | 130+ | TP     | In progress |
| `:target-current` | 130+   | 130+ | TP     | In progress |
| `sibling-index()` | 137+   | 137+ | 18.4+  | Behind flag |

For Firefox and older Safari, fall back to a plain snap track without dots — the carousel still works, just without the active-state indicator. Gate behind:

```css
@supports (animation-delay: calc(sibling-index() * 1ms)) {
	/* progressive enhancement */
}
```

## What you actually shipped

A carousel with:

- Native horizontal snap-scrolling
- Native pagination dots that update on scroll
- Native click-to-scroll on dot tap
- Staggered entry animation that auto-adjusts to card count
- Full reduce-motion support
- ~40 lines of CSS, zero JavaScript

Every one of those features was a JavaScript dependency a year ago.

> [Open the carousel demo on Web Maker](https://webmaker.app/create/eBbjNxrM8O?layout=1) — fork it, swap the cards for your own content, ship.

---

## That's the series

Seven days, seven 2026 features, seven runnable demos:

1. [CSS `if()`](https://webmaker.app/blog/css-if-function-tutorial)
2. [Anchor positioning](https://webmaker.app/blog/css-tooltip-without-javascript-anchor-positioning)
3. [Scroll-driven animations](https://webmaker.app/blog/css-only-scroll-progress-bar-2026)
4. [View Transitions](https://webmaker.app/blog/view-transitions-api-tabs-tutorial)
5. [`@scope`](https://webmaker.app/blog/css-scope-vs-bem-modules)
6. [JS interview pack](https://webmaker.app/blog/javascript-interview-questions-playground)
7. [CSS-only carousel](https://webmaker.app/blog/css-only-carousel-scroll-snap-sibling-index)

Pattern: every one of these features used to need JavaScript, a library, or both. The 2026 platform absorbs the work and leaves your apps lighter, faster, and more accessible.

If you build something with any of these, tag [@webmakerApp](https://twitter.com/webmakerApp) — best demos go in the next series.
