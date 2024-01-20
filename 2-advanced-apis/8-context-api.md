# Context API

## Table of Contents
- [Introduction](#introduction)
- [Canvas Component](#canvas-component)
- [Square Component](#square-component)
- [App Component](#app-component)
- [Usage of Context API](#usage-of-context-api)

---

### Introduction

The context API provides a mechanism for components to communicate with each other without the need to pass data and functions as props or dispatch numerous events. This advanced feature is particularly useful in certain scenarios. In the following exercise, we will replicate the artwork "Schotter" by George Nees, a pioneer of generative art, using the context API.

### Canvas Component

Inside the `Canvas.svelte` file, there is an `addItem` function responsible for adding an item to the canvas. This function can be made available to components within `<Canvas>`, such as `<Square>`, using the `setContext` function. Here's a snippet:

```svelte
<script>
    import { setContext, afterUpdate, onMount, tick } from 'svelte';

    // ...

    onMount(() => {
        ctx = canvas.getContext('2d');
    });

    setContext('canvas', {
        addItem
    });

    function addItem(fn) {...}

    function draw() {...}
</script>
```

### Square Component

In child components like `Square.svelte`, the context can be obtained using the `getContext` function:

```svelte
<script>
    import { getContext } from 'svelte';

    export let x;
    export let y;
    export let size;
    export let rotate;

    getContext('canvas').addItem(draw);

    function draw(ctx) {...}
</script>
```

### App Component

The `App.svelte` file demonstrates the use of the context API to create a grid with some randomness:

```svelte
<div class="container">
    <Canvas width={800} height={1200}>
        {#each Array(12) as _, c}
            {#each Array(22) as _, r}
                <Square
                    x={180 + c * 40 + jitter(r * 2)}
                    y={180 + r * 40 + jitter(r * 2)}
                    size={40}
                    rotate={jitter(r * 0.05)}
                />
            {/each}
        {/each}
    </Canvas>
</div>
```

### Usage of Context API

Like lifecycle functions, `setContext` and `getContext` must be called during component initialization. The context key (in this case, 'canvas') can be any string or non-string value. The context object can include stores, enabling the passing of values that change over time to child components. Here's an example:

```svelte
// in a parent component
import { setContext } from 'svelte';
import { writable } from 'svelte/store';

setContext('my-context', {
    count: writable(0)
});

// in a child component
import { getContext } from 'svelte';

const { count } = getContext('my-context');

$: console.log({ count: $count });
```

In summary, the context API is a mechanism for components to communicate effectively without passing data and functions explicitly. While it may not be frequently used in individual applications, it proves useful, especially in the context of a component library.