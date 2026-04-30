---
title: "View Transitions API: animate between states like it's 2026"
date: 2026-04-30
description: 'Build a tabs component with morphing underline and crossfading panels using the View Transitions API. One JavaScript call, zero animation libraries.'
---

The View Transitions API turns "before-state to after-state" into a one-liner. You wrap a DOM mutation in `document.startViewTransition()` and the browser:

1. Snapshots the current page.
2. Lets your callback mutate the DOM.
3. Snapshots the new state.
4. Crossfades between them.

That's the default behaviour. With one extra CSS property — `view-transition-name` — you can make individual elements morph between their old and new positions instead of just fading. This is what "shared element transitions" used to require an entire native-app framework to do.

> Fork along: [Web Maker — view-transitions tabs](https://webmaker.app/c/wm-view-transitions-tabs).

## The simplest possible transition

```js
function setActiveTab(name) {
	document.startViewTransition(() => {
		document
			.querySelectorAll('.tab')
			.forEach(t => t.classList.toggle('active', t.dataset.tab === name));
		panel.textContent = panels[name];
	});
}
```

That's the whole API surface for in-page transitions. The callback does the mutation; the browser handles the animation.

If `document.startViewTransition` isn't supported, just call your mutation directly:

```js
function setActiveTab(name) {
	const update = () => {
		document
			.querySelectorAll('.tab')
			.forEach(t => t.classList.toggle('active', t.dataset.tab === name));
		panel.textContent = panels[name];
	};
	document.startViewTransition
		? document.startViewTransition(update)
		: update();
}
```

## The morphing underline

Default crossfades are fine for panel content. For the active-tab indicator, we want the underline to _slide_ from the old tab to the new one. That's where `view-transition-name` earns its keep.

```html
<nav class="tabs">
	<button class="tab active" data-tab="overview">OVERVIEW</button>
	<button class="tab" data-tab="api">API</button>
	<button class="tab" data-tab="examples">EXAMPLES</button>
</nav>
```

```css
.tab.active::after {
	content: '';
	position: absolute;
	inset: auto 0 -4px 0;
	height: 3px;
	background: #ffe300;
	view-transition-name: tab-underline;
}
```

The key is `view-transition-name: tab-underline`. Only one element on the page can have a given transition name at any moment — and that's the trick. When `setActiveTab('api')` runs, the old `.active::after` disappears and a new `.active::after` appears on a different button. Both share the `tab-underline` name, so the browser treats them as the _same_ element moving from A to B. It animates the position, size, and any style differences automatically.

## Crossfade timings

The default duration is 250ms. Tune the curve and length per element:

```css
::view-transition-old(tab-underline),
::view-transition-new(tab-underline) {
	animation-duration: 200ms;
	animation-timing-function: cubic-bezier(0.2, 0.8, 0.2, 1);
}

::view-transition-old(root),
::view-transition-new(root) {
	animation-duration: 180ms;
}
```

`(root)` is the implicit name for the whole page. `(tab-underline)` is our custom one.

## Image grid expand/collapse

The same pattern works for grid-to-detail morphs.

```html
<div class="grid">
	<img src="a.jpg" data-id="a" />
	<img src="b.jpg" data-id="b" />
	<img src="c.jpg" data-id="c" />
</div>
<dialog class="lightbox"><img id="hero" /></dialog>
```

```js
function open(id) {
	const tile = document.querySelector(`[data-id="${id}"]`);
	document.startViewTransition(() => {
		tile.style.viewTransitionName = 'hero-img';
		document.getElementById('hero').src = tile.src;
		document.getElementById('hero').style.viewTransitionName = 'hero-img';
		document.querySelector('.lightbox').showModal();
	});
}
```

Apply the same `view-transition-name` to the source thumbnail and the destination image. The browser morphs one into the other — size, position, aspect ratio handled.

After the transition, clear the names so the next interaction works:

```js
document.addEventListener('transitionend', () => {
	document
		.querySelectorAll('[style*="view-transition-name"]')
		.forEach(el => (el.style.viewTransitionName = ''));
});
```

## The MPA variant (one line)

For multi-page navigations within the same origin, opt in once:

```css
@view-transition {
	navigation: auto;
}
```

Done. Same-origin link clicks now crossfade between pages. Add `view-transition-name` on shared elements (logos, nav, hero images) and they morph across pages.

## Reduce motion

The browser respects `prefers-reduced-motion: reduce` automatically — animations are replaced with an instant swap. You don't need to write anything for the default behaviour. If you _do_ have custom keyframes, gate them:

```css
@media (prefers-reduced-motion: no-preference) {
	::view-transition-old(tab-underline),
	::view-transition-new(tab-underline) {
		animation-duration: 200ms;
	}
}
```

## Browser support (April 2026)

| Browser | Status                                               |
| ------- | ---------------------------------------------------- |
| Chrome  | Stable for SPA since 111, MPA since 126              |
| Edge    | Stable since 111 / 126                               |
| Safari  | Stable for SPA since 18.2; MPA in Technology Preview |
| Firefox | In progress                                          |

The SPA API is widely shipped. The MPA variant is the cutting edge — gate its CSS behind `@supports`.

## Why this changes things

Before View Transitions, "morphing the active tab indicator" required FLIP, GSAP, Framer Motion, or a custom layout-and-transform dance with `getBoundingClientRect()`. None of those scaled well to MPA navigations.

The native API is one function call, one CSS property, and works for both SPA mutations _and_ full-page navigations.

> [Open the View Transitions tabs demo on Web Maker](https://webmaker.app/c/wm-view-transitions-tabs).

Tomorrow: [`@scope` and the end of BEM](https://webmaker.app/blog/css-scope-vs-bem-modules).
