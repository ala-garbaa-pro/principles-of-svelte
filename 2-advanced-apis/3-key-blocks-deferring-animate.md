# Key Blocks, Deferring, & Animate

## Table of Contents

- [Key Blocks](#key-blocks)
- [Deferred Transitions](#deferred-transitions)
- [Animate](#animate)

---

### Key Blocks

[Learn more](https://learn.svelte.dev/tutorial/key-blocks)

```svelte
<script>
  import { onMount } from 'svelte';
  import { typewriter } from './transition.js';
  import { messages } from './loading-messages.js';

  let i = -1;

  onMount(() => {
    const interval = setInterval(() => {
      i += 1;
      i %= messages.length;
    }, 2500);

    return () => {
      clearInterval(interval);
    };
  });
</script>

<h1>loading...</h1>

{#key i}
  <p in:typewriter={{ speed: 10 }}>
    {messages[i] || ''}
  </p>
{/key}
```

Ordinarily, elements are present in the DOM, and changing their values does not trigger transitions. In the provided Svelte example, a typewriter transition is applied to text as it changes. To achieve this, a key block is used, enclosing the paragraph element. The key value (`i`) triggers the replacement of the entire key block content when it changes. As a result, a new paragraph with the typewriter transition is created.

### Deferred Transitions

[Learn more](https://learn.svelte.dev/tutorial/deferred-transitions)

```svelte
<script>
  import { createTodoStore } from './todos.js';
  import TodoList from './TodoList.svelte';

  const todos = createTodoStore([
    { done: false, description: 'write some docs' },
    { done: false, description: 'start writing blog post' },
    { done: true, description: 'buy some milk' },
    { done: false, description: 'mow the lawn' },
    { done: false, description: 'feed the turtle' },
    { done: false, description: 'fix some bugs' }
  ]);
</script>

<div class="board">
  <input
    placeholder="what needs to be done?"
    on:keydown={(e) => {
      if (e.key !== 'Enter') return;

      todos.add(e.currentTarget.value);
      e.currentTarget.value = '';
    }}
  />

  <div class="todo">
    <h2>todo</h2>
    <TodoList store={todos} done={false} />
  </div>

  <div class="done">
    <h2>done</h2>
    <TodoList store={todos} done={true} />
  </div>
</div>

<style>
  /* Styles go here */
</style>
```

Svelte's transitions can be deferred to allow them to interact with each other. In the given example, two todo lists are presented - one for todos yet to be done and the other for completed todos. By utilizing the crossfade transition, elements smoothly transition from one list to another, providing a more realistic user experience. The crossfade transition is implemented in a separate module (`transition.js`) and utilized in the `TodoList.svelte` component.

### Animate

[Learn more](https://learn.svelte.dev/tutorial/animate)

```svelte
<script>
  import { flip } from 'svelte/animate';
  import { send, receive } from './transition.js';

  export let store;
  export let done;
</script>

<ul class="todos">
  {#each $store.filter((todo) => todo.done === done) as todo (todo.id)}
    <li
      class:done
      in:receive={{ key: todo.id }}
      out:send={{ key: todo.id }}
      animate:flip={{ duration: 200 }}
    >
      <label>
        <input
          type="checkbox"
          checked={todo.done}
          on:change={(e) => store.mark(todo, e.currentTarget.checked)}
        />

        <span>{todo.description}</span>

        <button on:click={() => store.remove(todo)} aria-label="Remove" />
      </label>
    </li>
  {/each}
</ul>

<style>
  /* Styles go here */
</style>
```

The `animate` directive in Svelte allows for smooth motion between elements. In this example, the `flip` function is used in combination with `animate` to achieve smooth transitions when toggling todo items between done and undone states. The `flip` technique stands for first, last, invert, play, and facilitates smooth motion entirely through CSS transitions. The `duration` property is adjusted to control the speed of the animation. Additionally, consideration for users who prefer reduced motion is incorporated by setting the duration to zero in such cases.