# Motion & Transitions

## Table of Contents

- [Motion & Transitions](#motion-transitions)
	- [Tweening with Svelte Motion](#tweening-with-svelte-motion)
	- [Springs in Svelte Motion](#springs-in-svelte-motion)
	- [Transitions in Svelte](#transitions-in-svelte)
	- [Parameterized Transitions in Svelte](#parameterized-transitions-in-svelte)
	- [Separate In and Out Transitions in Svelte](#separate-in-and-out-transitions-in-svelte)
	- [Custom CSS Transitions in Svelte](#custom-css-transitions-in-svelte)
	- [Custom JavaScript Transitions in Svelte](#custom-javascript-transitions-in-svelte)
	- [Transition Events in Svelte](#transition-events-in-svelte)
	- [Global Transitions in Svelte](#global-transitions-in-svelte)
	- [Tweening with Svelte Motion](#tweening-with-svelte-motion)

---


## [Tweening with Svelte Motion](https://learn.svelte.dev/tutorial/tweens)

In Svelte, motion refers to the smooth animation of elements, and we can achieve this using motion stores. One common use case is tweening, where a value gradually transitions over time. In the example below, a progress bar smoothly animates to new values using Svelte's motion store.

```svelte
<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});
</script>

<progress value={$progress} />

<button on:click={() => progress.set(0)}>
	0%
</button>

<button on:click={() => progress.set(0.25)}>
	25%
</button>

<button on:click={() => progress.set(0.5)}>
	50%
</button>

<button on:click={() => progress.set(0.75)}>
	75%
</button>

<button on:click={() => progress.set(1)}>
	100%
</button>

<style>
	progress {
		display: block;
		width: 100%;
	}
</style>
```

By using the `tweened` function from `svelte/motion`, we can smoothly transition between values with specified duration and easing functions.

## [Springs in Svelte Motion](https://learn.svelte.dev/tutorial/springs)


Springs provide another method of achieving motion in Svelte, particularly effective for values that frequently change. In the example below, the position of a circle follows the mouse cursor, creating a springy motion.

```svelte
<script>
	import { spring } from 'svelte/motion';

	let coords = spring({ x: 50, y: 50 }, {
		stiffness: 0.1,
		damping: 0.25
	});

	let size = spring(10);
</script>

<svg
	on:mousemove={(e) => {
		coords.set({ x: e.clientX, y: e.clientY });
	}}
	on:mousedown={() => size.set(30)}
	on:mouseup={() => size.set(10)}
	role="presentation"
>
	<circle
		cx={$coords.x}
		cy={$coords.y}
		r={$size}
	/>
</svg>

<div class="controls">
	<label>
		<h3>stiffness ({coords.stiffness})</h3>
		<input
			bind:value={coords.stiffness}
			type="range"
			min="0.01"
			max="1"
			step="0.01"
		/>
	</label>

	<label>
		<h3>damping ({coords.damping})</h3>
		<input
			bind:value={coords.damping}
			type="range"
			min="0.01"
			max="1"
			step="0.01"
		/>
	</label>
</div>

<style>
	svg {
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
	}

	circle {
		fill: #ff3e00;
	}

	.controls {
		position: absolute;
		top: 1em;
		right: 1em;
		width: 200px;
		user-select: none;
	}

	.controls input {
		width: 100%;
	}
</style>
```

By utilizing the `spring` function, we can create spring physics for smooth and responsive motion.

## [Transitions in Svelte](https://learn.svelte.dev/tutorial/transition)

Transitions in Svelte add a layer of visual appeal to elements entering and leaving the DOM. In the example below, the `fade` transition is applied to a paragraph, making it fade in and out based on the visibility checkbox.

```svelte
<script>
	import { fade } from 'svelte/transition';
	let visible = true;
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p transition:fade>
		Fades in and out
	</p>
{/if}
```

Here, the `fade` transition is applied when the paragraph enters or leaves the DOM.

## [Parameterized Transitions in Svelte](https://learn.svelte.dev/tutorial/adding-parameters-to-transitions)

Transitions in Svelte can be parameterized to add additional effects. In the example below, the `fly` transition is applied to a paragraph, making it fly in and out with specified parameters.

```svelte
<script>
	import { fly } from 'svelte/transition;
	let visible = true;
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p transition:fly={{ y: 200, duration: 2000 }}>
		Flies in and out
	</p>
{/if}
```

By specifying parameters such as `y` and `duration`, you can customize the behavior of the transition.

## [Separate In and Out Transitions in Svelte](https://learn.svelte.dev/tutorial/in-and-out)

Sometimes, you might want different transitions for elements entering and leaving the DOM. In this example, the `fly` transition is applied when the paragraph enters the DOM, and the `fade` transition is applied when it leaves.

```svelte
<script>
	import { fade, fly } from 'svelte/transition';
	let visible = true;
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p in:fly={{ y: 200, duration: 2000 }} out:fade>
		Flies in, fades out
	</p>
{/if}
```

Separate transitions for entering and leaving the DOM provide more control over the visual effects.

## [Custom CSS Transitions in Svelte](https://learn.svelte.dev/tutorial/custom-css-transitions)

Svelte allows the creation of custom CSS transitions using JavaScript functions. In the example below, a `spin` transition is defined, creating a unique spinning and color-changing effect.

```svelte
<script>
	import { fade } from 'svelte/transition';
	import { elasticOut } from 'svelte/easing';

	let visible = true;

	function spin(node, { duration }) {
		return {
			duration,
			css: (t) => {
				const eased = elasticOut(t);

				return `
					transform: scale(${eased}) rotate(${eased * 1080}deg);
					color: hsl(
						${Math.trunc(t * 360)},
						${Math.min(100, 1000 * (1 - t))}%,
						${Math.min(50, 500 * (1 - t))}%
					);`;
			}
		};
	}
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<div
		class="centered"
		in:spin={{ duration: 8000 }}
		out:fade
	>
		<span>transitions!</span>
	</div>
{/if}

<style>
	.centered {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	span {
		position: absolute;
		transform: translate(-50%, -50%);
		font-size: 4em;
	}
</style>
```

By defining the `spin

` transition, we can create highly customized and dynamic transitions.

## [Custom JavaScript Transitions in Svelte](https://learn.svelte.dev/tutorial/custom-js-transitions)

When more control is needed, Svelte allows the creation of custom JavaScript transitions. In the example below, a `typewriter` transition is defined, making text appear one character at a time.

```svelte
<script>
	let visible = false;

	function typewriter(node, { speed = 1 }) {
		const valid = node.childNodes.length === 1 && node.childNodes[0].nodeType === Node.TEXT_NODE;

		if (!valid) {
			throw new Error(`This transition only works on elements with a single text node child`);
		}

		const text = node.textContent;
		const duration = text.length / (speed * 0.01);

		return {
			duration,
			tick: (t) => {
				const i = Math.trunc(text.length * t);
				node.textContent = text.slice(0, i);
			}
		};
	}
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p transition:typewriter>
		The quick brown fox jumps over the lazy dog
	</p>
{/if}
```

Custom JavaScript transitions, like `typewriter`, enable fine-tuned control over element animations.

## [Transition Events in Svelte](https://learn.svelte.dev/tutorial/transition-events)

Svelte provides transition events that allow you to track when transitions start and end. In the example below, the `fly` transition is accompanied by events indicating the start and end of the intro and outro transitions.

```svelte
<script>
	import { fly } from 'svelte/transition';

	let visible = true;
	let status = 'waiting...';
</script>

<p>status: {status}</p>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p
		transition:fly={{ y: 200, duration: 2000 }}
		on:introstart={() => status = 'intro started'}
		on:outrostart={() => status = 'outro started'}
		on:introend={() => status = 'intro ended'}
		on:outroend={() => status = 'outro ended'}
	>
		Flies in and out
	</p>
{/if}
```

Tracking transition events using `on:introstart`, `on:outrostart`, `on:introend`, and `on:outroend` provides additional insights into the transition lifecycle.

## [Global Transitions in Svelte](https://learn.svelte.dev/tutorial/global-transitions)

By default, transitions play when any container block is added or destroyed. However, if you want individual elements to transition independently, you can turn a transition into a local transition. In the example below, the `slide` transition is applied to individual elements, allowing them to transition independently when the list length changes.

```svelte
<script>
	import { slide } from 'svelte/transition';

	let showItems = true;
	let i = 5;
	let items = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten'];
</script>

<label>
	<input type="checkbox" bind:checked={showItems} />
	show list
</label>

<label>
	<input type="range" bind:value={i} max="10" />
</label>

{#if showItems}
	{#each items.slice(0, i) as item}
		<div transition:slide|global>
			{item}
		</div>
	{/each}
{/if}

<style>
	div {
		padding: 0.5em 0;
		border-top: 1px solid #eee;
	}
</style>
```

By using `transition:slide|global`, you can control when individual elements transition, independent of container additions or removals.
