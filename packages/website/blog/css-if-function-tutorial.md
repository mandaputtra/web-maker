---
title: 'CSS if() is here. Build your first conditional style in 5 minutes.'
date: 2026-04-26
description: 'A practical, copy-pasteable tour of the new CSS if() function. Replace a theme-switcher, a responsive padding rule, and a variant prop without writing a single line of JavaScript.'
---

For two decades, "conditional CSS" meant either media queries on a stylesheet level or a JavaScript class-toggle on the body. The new `if()` function ships in stable Chrome and Safari and gives you an _inline_ ternary you can drop into any property value.

This post is a tour. Five minutes from top to bottom, three runnable demos, no toolchain.

> Fork along: [Web Maker — `if()` demo](https://webmaker.app/create/2mp2R1gCTB). Open it in another tab, edit while you read.

## The shape of the function

```css
property: if(<condition>: <value-if-true>; else: <value-if-false>);
```

The condition slot accepts three query types:

- `media(<query>)` — the same syntax as `@media`
- `supports(<declaration>)` — the same syntax as `@supports`
- `style(--token: value)` — matches against a custom property on the element or an ancestor

That third one is the interesting one. It turns custom properties into _props_ — and your stylesheet starts to look a lot like a component library without ever importing one.

## Demo 1: a one-line theme switch

The old way (JS):

```js
document.documentElement.classList.toggle('dark');
```

```css
:root {
	--bg: white;
	--fg: black;
}
:root.dark {
	--bg: #0a0a0f;
	--fg: #f4f1ea;
}
body {
	background: var(--bg);
	color: var(--fg);
}
```

The 2026 way:

```css
body {
	background: if(media(prefers-color-scheme: dark): #0a0a0f; else: white);
	color: if(media(prefers-color-scheme: dark): #f4f1ea; else: #0a0a0f);
}
```

No class toggle. No JavaScript. The browser re-evaluates when the OS theme changes.

If you _do_ want a manual override toggle, you stack a `style()` check on top:

```css
body {
	background: if(
		style(--theme: dark): #0a0a0f; media(prefers-color-scheme: dark): #0a0a0f;
			else: white
	);
}
[data-theme='dark'] {
	--theme: dark;
}
```

## Demo 2: variant props on a button

This is where `style()` queries shine. Define a variant token, branch on it, ship one selector instead of five.

```css
.btn {
	--variant: default;
	background: if(
		style(--variant: primary): #ffe300; style(--variant: danger): #ff5a36; else:
			#f4f1ea
	);
	color: if(style(--variant: danger): white; else: #0a0a0f);
	border: 1px solid currentColor;
	padding: if(style(--size: lg): 1rem 2rem; else: 0.5rem 1rem);
	font-family: 'JetBrains Mono', monospace;
}
```

```html
<button class="btn" style="--variant: primary">PRIMARY</button>
<button class="btn" style="--variant: danger; --size: lg">DANGER LG</button>
<button class="btn">DEFAULT</button>
```

Three buttons, one rule block. No `.btn--primary`, no `.btn--danger.btn--lg`.

## Demo 3: progressive enhancement without `@supports` blocks

`supports()` works exactly like `@supports`, but inline. Useful when you want a one-property fallback rather than duplicating a whole block.

```css
.card {
	padding: 1rem;
	/* Use container query units when supported, viewport units otherwise. */
	font-size: if(
		supports(font-size: 1cqi): 4cqi; else: clamp(1rem, 2vw, 1.5rem)
	);
}
```

The clamp fallback runs everywhere; modern browsers get the container-aware version.

## Gotchas

1. **The semicolons matter.** `if()` uses `;` between branches, not `,`. This is so commas can still appear inside individual values (e.g. inside `rgb()`).
2. **You can chain branches.** Order matters — first true wins, just like an `if/else if/else`.
3. **No nested `if()`** in the spec yet. If you need that, fall back to a stack of `style()` queries.
4. **Specificity is unchanged.** `if()` is a value, not a selector — it doesn't bump specificity, which is exactly what you want.

## Browser support (April 2026)

| Browser | Status            |
| ------- | ----------------- |
| Chrome  | Stable since 137  |
| Edge    | Stable since 137  |
| Safari  | Stable since 18.4 |
| Firefox | Behind flag       |

For Firefox-critical projects, ship the old class-toggle alongside `if()` — they don't conflict.

## Why this matters

Every value-level abstraction CSS absorbs is a layer of build tooling, runtime JavaScript, or hand-written boilerplate that goes away. `if()` is the smallest of the 2026 features but the one you'll reach for daily.

Fork the demo, swap in your own `style()` queries, ship something.

> [Open the `if()` demo on Web Maker](https://webmaker.app/create/2mp2R1gCTB?layout=2) — three working examples, ready to remix.

If you build something cool with `if()`, tag [@webmakerApp](https://x.com/webmakerApp) — we'll boost the best ones.
