# Svelte Basics

## Table of Contents

- [Svelte Basics](#svelte-basics)
  - [Hello World Example](#hello-world-example)
  - [Your First Component](#your-first-component)
  - [Dynamic Attributes](#dynamic-attributes)
  - [Styling](#styling)
  - [Nested Components](#nested-components)
  - [HTML Tags](#html-tags)

---

[Svelte](https://learn.svelte.dev/tutorial/welcome-to-svelte) is a user interface framework that simplifies app development by adopting a declarative state-driven approach. This markdown will guide you through the basics of Svelte and its unique features.

## Hello World Example

```svelte
<h1>Hello world!</h1>
```

SvelteKit, belonging to the realm of user interface frameworks, shares a common goal with UI frameworks by simplifying app development through a declarative state-driven approach. This approach replaces manual DOM updates with a more maintainable and comprehensible code structure. Svelte distinguishes itself by embracing the concept that a compiler can transform code into another form, making it not just a framework but also a language. This language feeds into the compiler, producing highly optimized vanilla JavaScript. Svelte allows flexibility in building entire applications and developing reusable components, which can be shared for wider usage.

# [Your First Component](https://learn.svelte.dev/tutorial/your-first-component)

```svelte
<script>
	let name = 'Svelte';
</script>

<h1>Hello {name.toUpperCase()}!</h1>
```

In Svelte, an application consists of one or more components, each being a reusable, self-contained block of code. Components combine HTML, CSS, and JavaScript within a `.svelte` file. Svelte treats HTML as valid Svelte, allowing seamless integration of HTML snippets from various sources into Svelte components. To enhance functionality, data can be introduced into components.

# [Dynamic Attributes](https://learn.svelte.dev/tutorial/dynamic-attributes)

```svelte
<script>
	let src = '/image.gif';
	let name = 'Rick Astley';
</script>

<img {src} alt="{name} dances." />
```

SvelteKit supports shorthand attributes where the attribute name and value are contained within curly braces. This shorthand usage is demonstrated in the code snippet with the "source" attribute.

# [Styling](https://learn.svelte.dev/tutorial/styling)

```svelte
<p>This is a paragraph.</p>

<style>
	p {
		color: goldenrod;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
```

Svelte is not just a JavaScript framework; it's a comprehensive web framework. It allows you to manage styles alongside components, incorporating them within a `style` tag.

# [Nested Components](https://learn.svelte.dev/tutorial/nested-components)

App.Svelte :
```svelte
<script>
	import Nested from './Nested.svelte';
</script>

<p>This is a paragraph.</p>
<Nested />

<style>
	p {
		color: goldenrod;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
```

Nested.Svelte :
```svelte
<p>This is another paragraph.</p>
```

Components in Svelte are conventionally capitalized to distinguish them from HTML elements. This separation between components helps in maintaining code effectively.

# [HTML Tags](https://learn.svelte.dev/tutorial/html-tags)

```svelte
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

Svelte provides an {@html} directive to render HTML stored in a variable. However, caution is needed, as this approach does not sanitize HTML. It should only be used with trustworthy data to avoid security vulnerabilities.