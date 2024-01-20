# Module Context & Debugging

## Table of Contents
- [Module Context](#module-context)
- [Exporting Functions](#exporting-functions)
- [Debugging](#debugging)
- [Congratulations](#congratulations)

---

## Module Context
In all the examples we've seen so far, the `<script>` block contains code that runs when each component instance is initialized. For the vast majority of components, that's all you'll ever need.

Very occasionally, you'll need to run some code outside of an individual component instance. For example, in our custom audio player from a previous exercise, playing all four tracks simultaneously might not be desired. It would be better if playing one track stopped all the others.

We can achieve this by declaring a `<script context="module">` block at the top of `AudioPlayer.svelte`. Code inside it will run once when the module first evaluates, rather than when a component is instantiated.

```svelte
<!-- AudioPlayer.svelte -->

<script context="module">
	let current;
</script>

<audio
	src={src}
	bind:currentTime={time}
	bind:duration
	bind:paused
	on:play={(e) => {
		const audio = e.currentTarget;

		if (audio !== current) {
			current?.pause();
			current = audio;
		}
	}}
	on:ended={() => {
		time = 0;
	}}
/>
```

Now, components can communicate with each other without the need for explicit state management.

## Exporting Functions
Anything exported from a `context="module"` script block becomes an export from the module itself. In this case, we export a `stopAll` function from `AudioPlayer.svelte`.

```svelte
<!-- AudioPlayer.svelte -->

<script context="module">
	let current;

	export function stopAll() {
		current?.pause();
	}
</script>
```

This function can then be imported and used in the `App.svelte` file.

```svelte
<!-- App.svelte -->

<script>
	import AudioPlayer, { stopAll } from './AudioPlayer.svelte';
</script>

<div class="centered">
	{#each tracks as track}
		<AudioPlayer {...track} />
	{/each}

	<button on:click={stopAll}>
		stop all
	</button>
</div>

You can't have a default export because the component is the default export.

## Debugging
Occasionally, it's useful to inspect a piece of data as it flows through your app. You can use `console.log` inside your markup, but if you want to pause execution, you can use the `{@debug ...}` tag with a comma-separated list of values you want to inspect.

```svelte
<!-- App.svelte -->

{@debug user}

<h1>Hello {user.firstname}!</h1>
```

This allows you to interact with the values and understand how data is flowing through your application.

## Congratulations
Congratulations! You've completed the Svelte tutorial and are ready to start building. The next parts of the tutorial will focus on SvelteKit, a full-fledged framework for creating apps.

If you're not ready for SvelteKit yet, you can use your existing Svelte knowledge without learning all of SvelteKit. Just run the following command in your terminal and follow the prompts:

```bash
npm create svelte@latest
```

Start editing `src/routes/+page.svelte` when you're ready. Click the link below to continue your journey.

[Continue your journey](https://learn.svelte.dev/tutorial/congratulations)

And that concludes parts one and two of the Svelte tutorial. You now have a solid understanding of all the features Svelte has to offer. Take a break, pat yourself on the back, and feel good about becoming a Svelte expert.