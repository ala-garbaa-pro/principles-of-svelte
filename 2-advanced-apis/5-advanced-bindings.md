# Advanced Bindings

## Table of Contents
- [Advanced Bindings](#advanced-bindings)
	- [Contenteditable Bindings](#contenteditable-bindings)
	- [Each Block Bindings](#each-block-bindings)
	- [Media Elements Bindings](#media-elements-bindings)
	- [Dimensions Bindings](#dimensions-bindings)
	- [Bind 'this' Directive](#bind-this-directive)
	- [Component Bindings](#component-bindings)
	- [Component 'this' Binding](#component-this-binding)

---



## [Contenteditable Bindings](https://learn.svelte.dev/tutorial/contenteditable-bindings)

Elements with a `contenteditable` attribute support `textContent` and `innerHTML` bindings:

```svelte
// App.svelte
<div bind:innerHTML={html} contenteditable />
```

In this example, the `div` element is made editable, and its inner HTML is bound to the `html` value, allowing dynamic updates.

---

## [Each Block Bindings](https://learn.svelte.dev/tutorial/each-block-bindings)

You can bind properties inside an `each` block, for example, with a todo list:

```svelte
// App.svelte
{#each todos as todo}
	<li class:done={todo.done}>
		<input type="checkbox" bind:checked={todo.done} />
		<input type="text" placeholder="What needs to be done?" bind:value={todo.text} />
	</li>
{/each}
```

Interacting with these `<input>` elements will mutate the array. If you prefer immutable data, use event handlers instead.

---

## [Media Elements Bindings](https://learn.svelte.dev/tutorial/media-elements)

You can bind properties of `<audio>` and `<video>` elements for custom player UI:

```svelte
// AudioPlayer.svelte
<div class="player" class:paused>
	<audio
		{src}
		bind:currentTime={time}
		bind:duration
		bind:paused
	/>
	<button class="play" aria-label={paused ? 'play' : 'pause'} />
</div>
```

This example demonstrates binding properties like `currentTime`, `duration`, and `paused` for an audio player.

---

## [Dimensions Bindings](https://learn.svelte.dev/tutorial/dimensions)

Every block-level element has `clientWidth`, `clientHeight`, `offsetWidth`, and `offsetHeight` bindings:

```svelte
// App.svelte
<div bind:clientWidth={w} bind:clientHeight={h}>
	<span style="font-size: {size}px" contenteditable>{text}</span>
	<span class="size">{w} x {h}px</span>
</div>
```

These bindings are read-only and are useful for measuring the dimensions of block-level elements.

---

## [Bind 'this' Directive](https://learn.svelte.dev/tutorial/bind-this)

Use the `bind:this` directive to bind to the component instance:

```svelte
// App.svelte
<canvas bind:this={canvas} width={32} height={32}></canvas>
```

The `canvas` variable will be undefined until the component has mounted. This is useful for cases where you need to interact with a component programmatically.

---

## [Component Bindings](https://learn.svelte.dev/tutorial/component-bindings)

You can bind to component props, similar to binding to DOM elements:

```svelte
// App.svelte
<Keypad bind:value={pin} on:submit={handleSubmit} />
```

Binding to component props should be used sparingly to maintain a clear data flow in your application.

---

## [Component 'this' Binding](https://learn.svelte.dev/tutorial/component-this)

Bind to component instances themselves with `bind:this`:

```svelte
// App.svelte
<div class="controls">
	<button on:click={() => canvas.clear()}>clear</button>
</div>
```

This is useful for rare cases when you need to interact with a component programmatically, providing a way to call methods on the component instance.

```svelte
// Canvas.svelte
<script>
	export let color;
	export let size;

	export function clear() {
		context.clearRect(0, 0, canvas.width, canvas.height);
	}
</script>
```

In this example, the `clear` function is defined in the `Canvas` component and called from the parent component using `bind:this`.