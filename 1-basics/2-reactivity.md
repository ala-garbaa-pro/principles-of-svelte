# Reactivity

## Table of Contents

- [Reactivity](#reactivity)
  - [Reactive Assignments](#reactive-assignments)
  - [Reactive Declarations](#reactive-declarations)
  - [Reactive Statements](#reactive-statements)
  - [Updating Arrays and Objects](#updating-arrays-and-objects)


# [Reactive Assignments](https://learn.svelte.dev/tutorial/reactive-assignments)

```svelte
<script>
    let count = 0;

    function increment() {
        count += 1;
    }
</script>

<button on:click={increment}>
    Clicked {count}
    {count === 1 ? 'time' : 'times'}
</button>
```

In Svelte, the reactivity system is a fundamental feature that effortlessly keeps the DOM synchronized with your application's state. As users interact with your application, the state undergoes changes, and the view seamlessly adapts to these alterations. The provided example demonstrates a basic Svelte component where a button click increments the `count` variable.

# [Reactive Declarations](https://learn.svelte.dev/tutorial/reactive-declarations)

```svelte
<script>
    let count = 0;
    $: doubled = count * 2;

    function increment() {
        count += 1;
    }
</script>

<button on:click={increment}>
    Clicked {count}
    {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>
```

Reactive declarations in Svelte allow you to derive values based on reactive variables. The syntax `$: doubled = count * 2` establishes a dependency between the values of `count` and `doubled`. Whenever `count` changes, `doubled` is automatically updated. This example introduces a paragraph displaying the doubled value alongside the button.

# [Reactive Statements](https://learn.svelte.dev/tutorial/reactive-statements)

```svelte
<script>
    let count = 0;

    $: if (count >= 10) {
        alert('count is dangerously high!');
        count = 0;
    }

    function handleClick() {
        count += 1;
    }
</script>

<button on:click={handleClick}>
    Clicked {count}
    {count === 1 ? 'time' : 'times'}
</button>
```

Reactive statements in Svelte go beyond updating values; they allow entire blocks of statements to run reactively. In this example, the block of code within `$: {...}` executes whenever any referenced elements within it undergo reactive updates. The provided example demonstrates an alert when `count` becomes greater than or equal to 10.

# [Updating Arrays and Objects](https://learn.svelte.dev/tutorial/updating-arrays-and-objects)

```svelte
<script>
    let numbers = [];

    function addNumber() {
        // Explicitly inform the compiler about array updates
        numbers = numbers;
        // Or use an immutable approach
        // numbers = [...numbers, newValue];
    }
</script>
```

This section explores updating arrays and objects in Svelte. Due to Svelte's reactivity being triggered by assignments, array methods like `push` won't automatically prompt updates. The example demonstrates a self-assignment (`numbers = numbers`) to inform the compiler about array updates. An alternative is using an immutable approach (`numbers = [...numbers, newValue]`). Svelte provides flexibility in choosing between mutable and immutable approaches based on your application's context. Additionally, Svelte supports direct assignments to properties of arrays and objects.