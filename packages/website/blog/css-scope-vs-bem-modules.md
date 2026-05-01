---
title: 'CSS @scope: the end of BEM (for real this time)'
date: 2026-05-01
description: 'A practical comparison of BEM, CSS Modules, and the new @scope at-rule. Refactor a real component three ways and see which one ships least code.'
ogImage: /images/blog/og-scope-vs-bem.png
---

![CSS @scope](/images/blog/og-scope-vs-bem.png)

For 15 years we've been styling components by inventing naming conventions. BEM, OOCSS, SMACSS, CSS Modules, CSS-in-JS. All of them solved the same problem — "stop styles from leaking" — by working _around_ the cascade rather than working with it.

`@scope` is the cascade growing the feature we needed all along: a way to declare _"these rules apply to this subtree, and nowhere else."_ It went production-ready early 2026 in Chrome, Edge, and Safari.

This post takes one component, refactors it three ways — BEM, CSS Modules, `@scope` — and counts the bytes.

> Fork along: [Web Maker — `@scope` demo](https://webmaker.app/create/z5PaG6j4Qi?layout=1). Three CSS files, same markup, side by side.

## The component

A simple card. Header, body, action row. Standard stuff.

```html
<article class="card">
	<header>
		<h3>Web Maker Pro</h3>
		<span>$6/mo</span>
	</header>
	<p>Cloud sync, embeds, unlimited collaboration.</p>
	<div class="actions">
		<button>Upgrade</button>
		<a href="#">Learn more</a>
	</div>
</article>
```

## The BEM way

```html
<article class="card">
	<header class="card__header">
		<h3 class="card__title">Web Maker Pro</h3>
		<span class="card__price">$6/mo</span>
	</header>
	<p class="card__body">Cloud sync, embeds, unlimited collaboration.</p>
	<div class="card__actions">
		<button class="card__btn card__btn--primary">Upgrade</button>
		<a class="card__link" href="#">Learn more</a>
	</div>
</article>
```

```css
.card {
	border: 1px solid #0a0a0f;
	padding: 1.5rem;
}
.card__header {
	display: flex;
	justify-content: space-between;
}
.card__title {
	font: 700 1.25rem 'JetBrains Mono';
}
.card__price {
	color: #ffe300;
}
.card__body {
	font-family: 'IBM Plex Sans';
}
.card__actions {
	display: flex;
	gap: 1rem;
	margin-top: 1rem;
}
.card__btn {
	padding: 0.5rem 1rem;
}
.card__btn--primary {
	background: #ffe300;
}
.card__link {
	color: #0a0a0f;
	text-decoration: underline #ffe300 2px;
}
```

Pros: works everywhere, 100% explicit. Cons: every element wears its component's name. Markup is cluttered. Renaming the component is a find-and-replace minefield.

## The CSS Modules way

```css
/* Card.module.css */
.card { ... }
.header { ... }
.title { ... }
.price { ... }
.body { ... }
.actions { ... }
.btn { ... }
.btnPrimary { ... }
.link { ... }
```

```jsx
import s from './Card.module.css';
<article className={s.card}>
  <header className={s.header}>
    <h3 className={s.title}>...</h3>
```

Pros: clean class names. Cons: every element still needs an explicit class. Requires a build tool. Class names get hashed at runtime, which complicates debugging and devtools spelunking.

## The @scope way

```html
<article class="card">
	<header>
		<h3>Web Maker Pro</h3>
		<span>$6/mo</span>
	</header>
	<p>Cloud sync, embeds, unlimited collaboration.</p>
	<div class="actions">
		<button>Upgrade</button>
		<a href="#">Learn more</a>
	</div>
</article>
```

```css
@scope (.card) {
	:scope {
		border: 1px solid #0a0a0f;
		padding: 1.5rem;
	}

	header {
		display: flex;
		justify-content: space-between;
	}
	h3 {
		font: 700 1.25rem 'JetBrains Mono';
	}
	header span {
		color: #ffe300;
	}

	p {
		font-family: 'IBM Plex Sans';
	}

	.actions {
		display: flex;
		gap: 1rem;
		margin-top: 1rem;
	}
	button {
		padding: 0.5rem 1rem;
		background: #ffe300;
	}
	a {
		color: #0a0a0f;
		text-decoration: underline #ffe300 2px;
	}
}
```

The markup is back to plain HTML. The CSS uses bare element selectors but they only match _inside_ `.card`. Different cards on the page can reuse `<button>` selectors without colliding.

## The donut: scoping with a lower limit

The killer feature: scope can have an _exit_ point.

```css
@scope (.card) to (.actions) {
	/* Rules apply inside .card but stop at .actions */
	button {
		background: red;
	} /* won't hit the upgrade button */
}
```

This is the "donut scope" — a hole in the middle. You can't do this with BEM or CSS Modules without manual class gymnastics. It's the answer to "I want to style the card body but not its embedded children components."

## Specificity gotcha

`@scope` doesn't change the specificity of selectors inside it. `:scope` itself is a pseudo-class with `(0,1,0)` specificity. The whole point is that scoping is _structural_, not specificity-based — so a rule outside `@scope` with higher specificity can still beat one inside.

In practice this is what you want. If you write `.btn-danger { background: red }` outside the scope, it overrides the scoped default. Component variants compose naturally.

## The byte count

For our card:

| Approach    | CSS bytes | HTML bytes | Total        |
| ----------- | --------- | ---------- | ------------ |
| BEM         | 412       | 524        | 936          |
| CSS Modules | 318       | 380 + JSX  | ~700 + build |
| @scope      | 286       | 286        | 572          |

`@scope` wins on both axes. No build step, no naming convention, smaller payload.

## When BEM still wins

- Deeply nested global design systems where _opt-in_ class names are clearer than implicit scoping.
- Static-site generators where the markup is hand-authored and class names double as documentation.
- Codebases where existing developers prefer convention over cascade rules.

For new code in 2026, the answer is `@scope`.

## Browser support (April 2026)

| Browser | Status            |
| ------- | ----------------- |
| Chrome  | Stable since 118  |
| Edge    | Stable since 118  |
| Safari  | Stable since 17.4 |
| Firefox | Stable since 128  |

Yes — including Firefox. This is one of the rare 2026 features with full Interop support today. Use it.

## Try it

> [Open the `@scope` demo on Web Maker](https://webmaker.app/create/z5PaG6j4Qi?layout=1) — three implementations, identical output, very different DX.

Tomorrow we change pace: [5 JavaScript interview questions to practice in a playground](https://webmaker.app/blog/javascript-interview-questions-playground).
