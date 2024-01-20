# Bindings

- [Bindings](#bindings)
	- [Text Inputs](#text-inputs)
	- [Numeric Inputs](#numeric-inputs)
	- [Checkbox Inputs](#checkbox-inputs)
	- [Select Bindings](#select-bindings)
	- [Group Inputs](#group-inputs)
	- [Multiple Select Bindings](#multiple-select-bindings)
	- [Textarea Inputs](#textarea-inputs)

Feel free to explore each section by clicking on the respective links!

### [Text Inputs](https://learn.svelte.dev/tutorial/text-inputs)

You can use bindings in Svelte to simplify the synchronization of data between components and elements. In the following examples, we explore different types of bindings.

```svelte
<script>
	let name = 'world';
</script>

<input bind:value={name} />

<h1>Hello {name}!</h1>
```

Okay, let's discuss bindings. In Svelte, data generally flows from the top down into your components. A parent component can set props on a child component, and a component can set attributes on an element, but not the other way around.

However, there are situations where breaking this rule is useful. Take the input element, for example. Instead of using event handlers to update the value, we can simplify by binding the value directly to a variable. So, when the text in the input changes, it keeps the value of the input and the variable (name in this case) in sync.

---


### [Numeric Inputs](https://learn.svelte.dev/tutorial/numeric-inputs)
```svelte
<script>
	let a = 1;
	let b = 2;
</script>

<label>
	<input type="number" bind:value={a} min="0" max="10" />
	<input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
	<input type="number" bind:value={b} min="0" max="10" />
	<input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>
```
The binding concept extends to various input types. For numeric inputs, Svelte understands the input is numeric, converting the string value from the DOM into a usable number. By applying bindings, changes in one input automatically update others, making it more convenient than using event handlers.


---


### [Checkbox Inputs](https://learn.svelte.dev/tutorial/checkbox-inputs)
```svelte
<script>
	let yes = false;
</script>

<label>
	<input type="checkbox" bind:checked={yes} />
	Yes! Send me regular email spam
</label>

{#if yes}
	<p>
		Thank you. We will bombard your inbox and sell
		your personal details.
	</p>
{:else}
	<p>
		You must opt in to continue. If you're not
		paying, you're the product.
	</p>
{/if}

<button disabled={!yes}>Subscribe</button>
```

Bindings are applicable to checkbox inputs as well. In this example, the checked property is bound to the yes variable, providing a straightforward way to manage the state of the checkbox. The conditional rendering based on the yes variable demonstrates the flexibility and simplicity of using bindings.

---

### [Select Bindings](https://learn.svelte.dev/tutorial/select-bindings)
```svelte
<script>
	let questions = [
		{
			id: 1,
			text: `Where did you go to school?`
		},
		{
			id: 2,
			text: `What is your mother's name?`
		},
		{
			id: 3,
			text: `What is another personal fact that an attacker could easily find with Google?`
		}
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(
			`answered question ${selected.id} (${selected.text}) with "${answer}"`
		);
	}
</script>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
	<select
		bind:value={selected}
		on:change={() => (answer = '')}
	>
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>

	<input bind:value={answer} />

	<button disabled={!answer} type="submit">
		Submit
	</button>
</form>

<p>
	selected question {selected
		? selected.id
		: '[waiting...]'}
</p>
```

It's not just limited to input elements; bindings can also be used with select elements. The selected option is bound to the selected variable, allowing for easy interaction and dynamic updates within the component.

---

### [Group Inputs](https://learn.svelte.dev/tutorial/group-inputs)
```svelte
<script>
	let scoops = 1;
	let flavours = [];

	const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
</script>

<h2>Size</h2>

{#each [1, 2, 3] as number}
	<label>
		<input
			type="radio"
			name="scoops"
			value={number}
			bind:group={scoops}
		/>

		{number} {number === 1 ? 'scoop' : 'scoops'}
	</label>
{/each}

<h2>Flavours</h2>

{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
	<label>
		<input
			type="checkbox"
			name="flavours"
			value={flavour}
			bind:group={flavours}
		/>

		{flavour}
	</label>
{/each}

{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops!</p>
{:else}
	<p>
		You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
		of {formatter.format(flavours)}
	</p>
{/if}
```
Svelte enables binding multiple inputs using the same value, introducing the concept of group bindings. This is particularly useful for radio and checkbox inputs that share a common value. The example illustrates how to manage the state of ordering ice cream scoops and flavors using group bindings.


---

### [Multiple Select Bindings](https://learn.svelte.dev/tutorial/multiple-select-bindings)
```svelte
<script>
	let scoops = 1;
	let flavours = [];

	const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
</script>

<h2>Size</h2>

{#each [1, 2, 3] as number}
	<label>
		<input
			type="radio"
			name="scoops"
			value={

number}
			bind:group={scoops}
		/>

		{number} {number === 1 ? 'scoop' : 'scoops'}
	</label>
{/each}

<h2>Flavours</h2>

<select multiple bind:value={flavours}>
	{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
		<option>{flavour}</option>
	{/each}
</select>

{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops!</p>
{:else}
	<p>
		You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
		of {formatter.format(flavours)}
	</p>
{/if}
```
When dealing with multiple selections, Svelte provides options such as checkbox inputs or a select element with the multiple attribute. The example showcases using a select element to bind values to an array, demonstrating the flexibility of Svelte bindings in handling different input scenarios.

---

### [Textarea Inputs](https://learn.svelte.dev/tutorial/textarea-inputs)
```svelte
<script>
	import { marked } from 'marked';
	let value = `Some words are *italic*, some are **bold**\n\n- lists\n- are\n- cool`;
</script>

<div class="grid">
	input
	<textarea bind:value></textarea>

	output
	<div>{@html marked(value)}</div>
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: 5em 1fr;
		grid-template-rows: 1fr 1fr;
		grid-gap: 1em;
		height: 100%;
	}

	textarea {
		flex: 1;
		resize: none;
	}
</style>
```

Text area elements can also benefit from bindings. By binding the value of a text area, changes made by the user are reflected in the variable, allowing for dynamic updates. The example utilizes the marked library to convert the text to Markdown and showcases the versatility of bindings even with larger text inputs.
