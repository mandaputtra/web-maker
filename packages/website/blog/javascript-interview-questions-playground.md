---
title: '5 JavaScript interview questions you can practice right now in a playground'
date: 2026-05-07
description: 'Closures, the event loop, this, prototypes, debounce. Each question with a runnable demo, the expected answer, and the trap interviewers actually look for.'
---

Frontend interviews in 2026 don't ask you to define "closure." They ask you to _use_ one — usually under a 25-minute timer, often live-shared. You won't have docs open. You won't have an LLM. You'll have a blank editor and an interviewer waiting.

The fastest way to internalise these patterns is to write them, break them, and watch the output change. That's exactly what a playground is for.

This post is a five-question pack. Each question links to a Web Maker creation pre-loaded with the prompt as HTML, the expected output in the console, and an empty JS file ready for your attempt.

> Pack index: [Web Maker — JS interview pack](https://webmaker.app/create/zLAC0JBHGf?layout=1).

---

## Q1 — Closures: build a counter factory

> Implement `makeCounter(start)` such that:
>
> ```js
> const c = makeCounter(10);
> c.inc(); // 11
> c.inc(); // 12
> c.value(); // 12
> c.reset(); // 10
> ```

Try it before reading on: [Q1 demo](https://webmaker.app/create/zLAC0JBHGf?layout=1).

### The answer

```js
function makeCounter(start) {
	let count = start;
	return {
		inc() {
			return ++count;
		},
		value() {
			return count;
		},
		reset() {
			return (count = start);
		}
	};
}
```

### The trap

If you write `this.count = start` and use `this`, the interviewer will follow up with: "now what happens if I do `const inc = c.inc; inc()`?" and you'll get `NaN`. Closures over locals are immune to `this`-binding loss; that's exactly why they're the right tool here.

---

## Q2 — Event loop: order this output

> Without running it, predict the console output:
>
> ```js
> console.log('A');
> setTimeout(() => console.log('B'), 0);
> Promise.resolve().then(() => console.log('C'));
> queueMicrotask(() => console.log('D'));
> console.log('E');
> ```

Try it: [Q2 demo](https://webmaker.app/create/zLAC0JBHGf?layout=1). Run it, _then_ try to explain the order.

### The answer

```
A
E
C
D
B
```

### The trap

People mix up microtasks and tasks. The rule:

1. The current synchronous block runs to completion (`A`, `E`).
2. The microtask queue drains — `Promise.then` callbacks and `queueMicrotask` callbacks, in FIFO order (`C`, `D`).
3. _Then_ the next macrotask runs — `setTimeout` (`B`).

If your interviewer asks "what about `requestAnimationFrame`?" — that's a separate queue, processed before paint, after microtasks. Bonus points.

---

## Q3 — `this` binding: fix this code

> Why does `delay()` log `undefined`, and how do you fix it without changing the call site?
>
> ```js
> const user = {
> 	name: 'Avery',
> 	greet() {
> 		console.log(`Hi, ${this.name}`);
> 	},
> 	delay() {
> 		setTimeout(this.greet, 100);
> 	}
> };
> user.delay(); // Hi, undefined
> ```

Try it: [Q3 demo](https://webmaker.app/create/zLAC0JBHGf?layout=1).

### The answer

`setTimeout` calls `this.greet` as a plain function — `this` becomes `undefined` (in strict mode) or `globalThis`. Three fixes, ranked from worst to best:

```js
// 1. .bind() — verbose
delay() { setTimeout(this.greet.bind(this), 100); }

// 2. Arrow function — preserves outer this
delay() { setTimeout(() => this.greet(), 100); }

// 3. Pass a method directly through an arrow
delay() { setTimeout(() => this.greet(), 100); }
```

Use the arrow. It reads as "do this, in this lexical context."

### The trap

If you instinctively reach for `bind`, the interviewer asks the follow-up: "what does `bind` return?" — a new function, every time. In a tight loop, `bind` is the GC-pressure answer. The arrow form is allocation-equivalent but reads better.

---

## Q4 — Prototypal inheritance: extend an array

> Add a `last()` method to all arrays without using ES6 `class`. Then explain why your fix doesn't break `for...in`.

Try it: [Q4 demo](https://webmaker.app/create/zLAC0JBHGf?layout=1).

### The answer

```js
Object.defineProperty(Array.prototype, 'last', {
	value() {
		return this[this.length - 1];
	},
	enumerable: false,
	writable: true,
	configurable: true
});

[1, 2, 3].last(); // 3
```

### The trap

If you write `Array.prototype.last = function() {...}`, the property is _enumerable by default_. That breaks any code doing `for (const k in arr)` — `last` shows up as a key.

`Object.defineProperty` with `enumerable: false` is the safe form. It's also why you should never extend built-ins in library code unless you _know_ no consumer iterates the prototype.

---

## Q5 — Implement debounce from scratch

> Write `debounce(fn, ms)` such that the returned function only fires `fn` after `ms` milliseconds have passed without further calls.
>
> ```js
> const log = debounce(console.log, 200);
> log('a');
> log('b');
> log('c'); // only 'c' should print, after 200ms
> ```

Try it: [Q5 demo](https://webmaker.app/c/wm-js-interview-q5).

### The answer

```js
function debounce(fn, ms) {
	let timer;
	return function (...args) {
		clearTimeout(timer);
		timer = setTimeout(() => fn.apply(this, args), ms);
	};
}
```

### The trap

Three common follow-ups:

1. **"Now add a `cancel()` method."** Return an object or attach a property: `debounced.cancel = () => clearTimeout(timer);`
2. **"What if I want the _first_ call to fire immediately?"** That's _leading-edge_ debounce. Track a flag, fire immediately on first call, set a timer to reset the flag.
3. **"Difference between debounce and throttle?"** Debounce: only fires after silence. Throttle: fires at most once per window. Search-as-you-type uses debounce; scroll handlers use throttle.

---

## How to use Web Maker for interview prep

A few patterns that work for actual practice:

1. **Don't peek.** Open the question demo, _close this tab_, attempt the solution, then come back to compare.
2. **Time yourself.** Real interviews are timed. 5–8 minutes per question is realistic.
3. **Run the broken version first.** Each demo ships with the bug intact. Watch the output, then fix it. You learn more from a failing program than a passing one.
4. **Fork and add edge cases.** Once your solution passes the prompt, add three calls that should break it. Senior interviews are about edge cases.

> [Open the JS interview pack on Web Maker](https://webmaker.app/create/zLAC0JBHGf?layout=1) and start with Q1.

If you're using these for a workshop or mock-interview programme, the [Web Maker Pro](https://webmaker.app/pro) team plan has shared collections — drop the pack into one and your candidates fork from it.

Tomorrow, the final post: [a CSS-only carousel using `scroll-snap` and `sibling-index()`](https://webmaker.app/blog/css-only-carousel-scroll-snap-sibling-index).
