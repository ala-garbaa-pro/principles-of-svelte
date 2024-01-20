# Logic

# Table of Contents

- [Logic](#logic)
  - [If Blocks](#if-blocks)
  - [Else Blocks](#else-blocks)
  - [Else If Blocks](#else-if-blocks)
  - [Each Blocks](#each-blocks)
  - [Keyed Each Blocks](#keyed-each-blocks)
  - [Await Blocks](#await-blocks)

## [If Blocks](https://learn.svelte.dev/tutorial/if-blocks)

```svelte
<script>
	let count = 0;

	function increment() {
		count += 1;
	}
</script>

<button on:click={increment}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

{#if count > 10}
	<p>{count} is greater than 10</p>
{/if}
```

In Svelte, logic can be expressed through conditionals. The example demonstrates the use of an `if` block to conditionally render markup based on the value of `count`. A paragraph is displayed when the count is greater than 10.

## [Else Blocks](https://learn.svelte.dev/tutorial/else-blocks)

```svelte
<script>
	let count = 0;

	function increment() {
		count += 1;
	}
</script>

<button on:click={increment}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

{#if count > 10}
	<p>{count} is greater than 10</p>
{:else}
	<p>{count} is between 0 and 10</p>
{/if}
```

An `else` block is introduced in this example, providing an alternative condition for when `count` is not greater than 10. The markup is updated to reflect whether `count` is between 0 and 10.

## [Else If Blocks](https://learn.svelte.dev/tutorial/else-if-blocks)

```svelte
<script>
	let count = 0;

	function increment() {
		count += 1;
	}
</script>

<button on:click={increment}>
	Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

{#if count > 10}
	<p>{count} is greater than 10</p>
{:else if count < 5}
	<p>{count} is less than 5</p>
{:else}
	<p>{count} is between 5 and 10</p>
{/if}
```

Multiple conditions are handled using `else if` blocks. Depending on the value of `count`, different paragraphs are displayed, indicating whether it is greater than 10, less than 5, or between 5 and 10.

## [Each Blocks](https://learn.svelte.dev/tutorial/each-blocks)

```svelte
<script>
	const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];
	let selected = colors[0];
</script>

<h1 style="color: {selected}">Pick a colour</h1>

<div>
	{#each colors as color, i}
		<button
			aria-current={selected === color}
			aria-label={color}
			style="background: {color}"
			on:click={() => selected = color}
		>{i + 1}</button>
	{/each}
</div>

<style>
	h1 {
		transition: color 0.2s;
	}

	div {
		display: grid;
		grid-template-columns: repeat(7, 1fr);
		grid-gap: 5px;
		max-width: 400px;
	}

	button {
		aspect-ratio: 1;
		border-radius: 50%;
		background: var(--color, #fff);
		transform: translate(-2px,-2px);
		filter: drop-shadow(2px 2px 3px rgba(0,0,0,0.2));
		transition: all 0.1s;
	}

	button[aria-current="true"] {
		transform: none;
		filter: none;
		box-shadow: inset 3px 3px 4px rgba(0,0,0,0.2);
	}
</style>
```

An `each` block is utilized to iterate over the `colors` array, rendering buttons dynamically with different background colors. The selected color is updated when a button is clicked.

## [Keyed Each Blocks](https://learn.svelte.dev/tutorial/keyed-each-blocks)

App.svelte
```svelte
<script>
	import Thing from './Thing.svelte';

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	];

	function handleClick() {
		things = things.slice(1);
	}
</script>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing (thing.id)}
	<Thing name={thing.name} />
{/each}
```

Thing.svelte
```svelte
<script>
	const emojis = {
		apple: '🍎',
		banana: '🍌',
		carrot: '🥕',
		doughnut: '🍩',
		egg: '🥚'
	};

	export let name;

	const emoji = emojis[name];
</script>

<p>{emoji} = {name}</p>
```

A keyed `each` block is employed to iterate over the `things` array in App.svelte. The unique `id` is used as a key to track and efficiently update components when items are added or removed.

## [Await Blocks](https://learn.svelte.dev/tutorial/await-blocks)

App.svelte
```svelte
<script>
	import { getRandomNumber } from './utils.js';

	let promise = getRandomNumber();

	function handleClick() {
		promise = getRandomNumber();
	}
</script>

<button on:click={handleClick}>
	generate random number
</button>

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

utils.js
```js
export async function getRandomNumber() {
    const res = await fetch('/random-number');

    if (res.ok) {
        return await res.text();
    } else {
        throw new Error('Request failed');
    }
}
```

An `await`

block is introduced to handle asynchronous operations in Svelte. In this example, a button triggers the asynchronous function `getRandomNumber()`, and the result is displayed based on the promise's state. The user interface updates with a loading message while waiting, displays the resolved value when available, and shows an error message if the promise is rejected. The asynchronous function is located in the `utils.js` file.