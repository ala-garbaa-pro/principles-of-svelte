# Classes & Styles

## Table of Contents
- [Classes](#classes)
  - [Basic Class Specification](#basic-class-specification)
  - [Using the Class Directive](#using-the-class-directive)
  - [Class Shorthand](#class-shorthand)
- [Styles](#styles)
  - [Inline Styles](#inline-styles)
  - [Style Directive](#style-directive)
  - [Component Styles](#component-styles)
- [Questions and Answers](#questions-and-answers)

---

## Classes

### Basic Class Specification

To specify classes on an element using attributes, you can utilize JavaScript. For instance, adding a flipped class to a card button can be done as follows:

```html
<button
  class="card {flipped ? 'flipped' : ''}"
  on:click={() => flipped = !flipped}
>
```

### Using the Class Directive

Svelte provides a directive to simplify the common pattern of adding or removing a class based on a condition:

```html
<button
  class="card"
  class:flipped={flipped}
  on:click={() => flipped = !flipped}
>
```

This directive means 'add the flipped class whenever flipped is truthy'.

### Class Shorthand

A shorthand form is available when the class name matches the condition:

```html
<button
  class="card"
  class:flipped
  on:click={() => flipped = !flipped}
>
```

### More on Classes: [Svelte Tutorial - Classes](https://learn.svelte.dev/tutorial/classes)

## Styles

### Inline Styles

Similar to class attributes, styles can be specified directly in the HTML:

```html
<button
  class="card"
  style="transform: {flipped ? 'rotateY(0)' : ''}; --bg-1: palegoldenrod; --bg-2: black; --bg-3: goldenrod"
  on:click={() => flipped = !flipped}
>
```

### Style Directive

For better organization, Svelte offers a style directive to manage individual styles:

```html
<button
  class="card"
  style:transform={flipped ? 'rotateY(0)' : ''}
  style:--bg-1="palegoldenrod"
  style:--bg-2="black"
  style:--bg-3="goldenrod"
  on:click={() => flipped = !flipped}
>
```

### Component Styles

When influencing styles inside a child component, using `:global` is an option, but it's recommended to use CSS custom properties:

```html
<style>
  .boxes :global(.box:nth-child(1)) {
    background-color: red;
  }
  /* ... similar styles for other boxes ... */
</style>

<Box --color="red" />
<Box --color="green" />
<Box --color="blue" />
```

This allows components to control which styles can be influenced from the outside.

### More on Styles: [Svelte Tutorial - Styles](https://learn.svelte.dev/tutorial/styles)

## Questions and Answers

### Adding Multiple Classes

Q: When adding a class, can you add multiple classes?

A: Yes, you can add multiple classes to a component using multiple directives. For example:

```html
<button class:foo={1} class:bar={2} on:click={() => /* ... */}>
```

### Binding to Multiple Elements

Q: What if I needed to bind more than one element with something like query selector all?

A: You can bind to multiple elements using arrays. For example:

```html
<script>
  import { onMount } from 'svelte';

  let divs;

  onMount(() => {
    for (const div of divs) {
      div.textContent = 'It works';
    }
  });
</script>
```

This approach allows you to bind to multiple elements inside arrays.
