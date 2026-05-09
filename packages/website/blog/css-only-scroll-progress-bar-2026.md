---
title: 'Scroll-driven animations: the reading-progress bar in 10 lines of CSS'
date: 2026-04-29
description: 'Build a reading-progress bar, a parallax hero, and a section-reveal effect using only CSS scroll-driven animations. Zero IntersectionObserver, zero requestAnimationFrame.'
ogImage: /images/blog/og-css-reading-progress-bar.jpg
---

If you've ever wired up a reading-progress bar, you wrote roughly the same JavaScript: listen to `scroll`, calculate `scrollTop / (scrollHeight - clientHeight)`, set a `width` percentage. It's not hard, but it ran on every scroll frame and competed for the main thread.

Scroll-driven animations make all of that go away. You declare what should animate and which scroll provides the timeline. The browser drives the rest, off the main thread.

> Fork along: [Web Maker — scroll-driven trio](https://webmaker.app/create/F57hVPFXzx?layout=1). Three demos in one creation.

## The two timelines you'll actually use

- `scroll()` — animation progress is tied to a scroll container's scroll position. Use this for progress bars, scroll-linked indicators.
- `view()` — animation progress is tied to an element's position _within_ the viewport. Use this for reveal effects and parallax.

Both replace `IntersectionObserver` for 90% of cases.

## Demo 1: the reading-progress bar

```html
<div class="progress"></div>
<article>... long content ...</article>
```

```css
.progress {
	position: fixed;
	top: 0;
	left: 0;
	height: 3px;
	width: 100%;
	background: #ffe300;
	transform-origin: left;
	animation: grow linear;
	animation-timeline: scroll(root block);
}

@keyframes grow {
	from {
		transform: scaleX(0);
	}
	to {
		transform: scaleX(1);
	}
}
```

That's the entire bar. Ten lines, all CSS. `scroll(root block)` means "the document's vertical scroll." The animation progresses from 0% to 100% as the page scrolls top-to-bottom.

Pause for a second on what's _not_ here: no `addEventListener`, no `requestAnimationFrame`, no debounce, no resize handler. The browser knows the scroll length and drives the keyframe. When you change page length, it just works.

## Demo 2: parallax hero

The classic effect: hero image translates slowly while text translates normally.

```css
.hero {
	position: relative;
	overflow: clip;
}

.hero img {
	animation: parallax linear;
	animation-timeline: view();
	animation-range: cover 0% cover 100%;
}

@keyframes parallax {
	from {
		transform: translateY(-15%);
	}
	to {
		transform: translateY(15%);
	}
}
```

`view()` defines the timeline as "while this element passes through the viewport." `animation-range: cover` means "from the moment its top edge enters until its bottom edge leaves." The translate range is up to you — 15% gives a subtle, non-vomit-inducing shift.

## Demo 3: section reveal

Cards that fade and lift in as they scroll into view.

```css
.card {
	opacity: 0;
	transform: translateY(40px);
	animation: reveal linear both;
	animation-timeline: view();
	animation-range: entry 0% entry 60%;
}

@keyframes reveal {
	to {
		opacity: 1;
		transform: translateY(0);
	}
}
```

`animation-range: entry 0% entry 60%` is the magic. Translation: "start when the card just enters the viewport, complete the animation when it's 60% of the way in." Past 60%, the card stays at the `to` state because of `both`.

This replaces your IntersectionObserver-driven `.is-visible` class toggles. The CSS engine handles the threshold math.

## Reduce motion: don't forget

Scroll-driven animations are still _animations_. People with vestibular disorders feel them.

```css
@media (prefers-reduced-motion: reduce) {
	.progress {
		animation: none;
		transform: scaleX(1);
	}
	.hero img {
		animation: none;
	}
	.card {
		animation: none;
		opacity: 1;
		transform: none;
	}
}
```

Ship this in the same file. It's three lines per effect; there's no excuse.

## The performance story

Pre-2025, a parallax effect commonly looked like:

```js
window.addEventListener('scroll', () => {
	const y = window.scrollY;
	hero.style.transform = `translateY(${y * 0.15}px)`;
});
```

Two problems:

1. The handler fires on every scroll event, on the main thread.
2. Every assignment triggers a style recalc + layout + paint cycle.

Scroll-driven animations run on the compositor thread. The main thread can be busy parsing JSON or rendering React — your animation is unaffected. Lighthouse Total Blocking Time goes down by default.

## Browser support (April 2026)

| Browser | Status                                               |
| ------- | ---------------------------------------------------- |
| Chrome  | Stable since 115                                     |
| Edge    | Stable since 115                                     |
| Safari  | Stable since 18.2                                    |
| Firefox | Behind `layout.css.scroll-driven-animations.enabled` |

For Firefox holdouts, gate behind `@supports (animation-timeline: scroll())` — your fallback is "no animation," which is fine.

## Try it

> [Open the scroll-driven trio on Web Maker](https://webmaker.app/create/F57hVPFXzx?layout=1) — progress bar, parallax, reveal, all in one file.

Tomorrow: [View Transitions API](https://webmaker.app/blog/view-transitions-api-tabs-tutorial). The same "let the browser do it" idea, applied to state changes.
