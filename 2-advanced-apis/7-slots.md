# Slots

## Table of Contents
- [Introduction](https://learn.svelte.dev/tutorial/slots)
- [Default Slots](https://learn.svelte.dev/tutorial/slots#default-slots)
- [Named Slots](https://learn.svelte.dev/tutorial/named-slots)
- [Styling and Global Modifiers](https://learn.svelte.dev/tutorial/named-slots#styling-and-global-modifiers)
- [Slot Fallbacks](https://learn.svelte.dev/tutorial/slot-fallbacks)
- [Slot Props](https://learn.svelte.dev/tutorial/slot-props)
- [Optional Slots](https://learn.svelte.dev/tutorial/optional-slots)

---

Just like elements can have children:

```html
<div>
	<p>I'm a child of the div</p>
</div>
```

So can components. Before a component can accept children, though, it needs to know where to put them. We do this with the `<slot>` element. Put this inside `Card.svelte`:

```html
<div class="card">
	<slot />
</div>
```

You can now put things on the card in `App.svelte`:

```html
<Card>
	<span>Patrick BATEMAN</span>
	<span>Vice President</span>
</Card>
```

Just like elements can have children, certain components need to know where to place their children. Before a component can accept children, it uses the `<slot>` element for this purpose. The `Card.svelte` component now has a `<div>` with a class of "card" and a `<slot>` inside it. In `App.svelte`, you can place content within the `<Card>` component.

[Named Slots](https://learn.svelte.dev/tutorial/named-slots) allow more control over placement. In `Card.svelte`, named slots are added:

```html
<div class="card">
	<header>
		<slot name="telephone" />
		<slot name="company" />
	</header>

	<slot />

	<footer>
		<slot name="address" />
	</footer>
</div>
```

Styles for the `<small>` element in `App.svelte` are added for proper formatting:

```html
<style>
	main {
		display: grid;
		place-items: center;
		height: 100%;
		background: url(./wood.svg);
	}

	small {
		display: block;
		font-size: 0.6em;
		text-align: right;
	}
</style>
```

Alternatively, the `:global` modifier inside `Card.svelte` can be used:

```html
<style>
	/* ... */

	.card :global(small) {
		display: block;
		font-size: 0.6em;
		text-align: right;
	}
</style>
```

[Fallbacks](https://learn.svelte.dev/tutorial/slot-fallbacks) for empty slots in `Card.svelte`:

```html
<div class="card">
	<header>
		<slot name="telephone">
			<i>(telephone)</i>
		</slot>

		<slot name="company">
			<i>(company name)</i>
		</slot>
	</header>

	<slot>
		<i>(name)</i>
	</slot>

	<footer>
		<slot name="address">
			<i>(address)</i>
		</slot>
	</footer>
</div>
```

[Slot Props](https://learn.svelte.dev/tutorial/slot-props) example using `FilterableList.svelte` and `App.svelte`:

```html
<div class="content">
	{#each data.filter(matches) as item}
		<slot {item} />
	{/each}
</div>
```

```html
<FilterableList
	data={colors}
	field="name"
	let:item={row}
>
	<div class="row">
		<span class="color" style="background-color: {row.hex}" />
		<span class="name">{row.name}</span>
		<span class="hex">{row.hex}</span>
		<span class="rgb">{row.rgb}</span>
		<span class="hsl">{row.hsl}</span>
	</div>
</FilterableList>
```

[Optional Slots](https://learn.svelte.dev/tutorial/optional-slots) using `$$slots` in `FilterableList.svelte`:

```html
{#if $$slots.header}
	<div class="header">
		<slot name="header"/>
	</div>
{/if}
```

In some cases, you might want to vary the contents of the component based on whether slotted content has been provided. The special `$$slots` variable in Svelte components is used to check if a slot was provided.