# Stores

- [Stores](#stores)
  - [Introduction to Stores](#introduction-to-stores)
  - [Managing a Count Store](#managing-a-count-store)
  - [Readable Stores](#readable-stores)
  - [Derived Stores](#derived-stores)
  - [Custom Stores](#custom-stores)
  - [Store Bindings](#store-bindings)

## [Introduction to Stores](https://learn.svelte.dev/tutorial/stores-intro)

This section concludes Part 1, focusing on state management in Svelte using stores. In applications, there is often a need for state that does not belong to a specific component. This shared state can be accessed by multiple components or by a regular JavaScript module. In Svelte, a built-in state management primitive known as a store facilitates this functionality.

A store is essentially an object with a `subscribe` method. When you call `subscribe` on a store, a callback is executed every time the value of the store changes.

## [Managing a Count Store](https://learn.svelte.dev/tutorial/managing-a-count-store)

In the `app.svelte` component, a store named `count` is introduced. The `count.subscribe` method is used to track changes in the count value, and the current count value is assigned to the variable `count_value`.

```javascript
// app.svelte
<script>
  import {count} from './stores.js'; let count_value; const increment = () =>
  count.update(n => n + 1); const decrement = () => count.update(n => n - 1);
  const reset = () => count.set(0);
</script>
```

### Addressing Subtle Bugs

A potential memory leak issue is identified when a component is instantiated multiple times, creating multiple subscriptions to the store. To address this, a reference to the `unsubscribe` function returned from `count.subscribe` is obtained. The `onDestroy` lifecycle hook is used to clean up subscriptions when the component is removed from the DOM.

```javascript
// app.svelte
<script>
  import {count} from './stores.js'; let $count; const increment = () => $count
  += 1; const decrement = () => $count -= 1; const reset = () => $count = 0;
</script>
```

The shorthand syntax, using the `$` prefix (e.g., `$count`), is introduced to simplify the code and automatically handle subscription cleanup.

## [Readable Stores](https://learn.svelte.dev/tutorial/readable-stores)

Not all stores should be writable. For read-only scenarios, readable stores are introduced. The `readable` function is used to create a readable store in `stores.js`. The `start` function is invoked when the first subscriber arrives, and the `stop` function is called when the last subscriber leaves. This is a convenient place for setup and teardown logic.

```javascript
// stores.js
import { readable } from "svelte/store";

export const time = readable(new Date(), function start(set) {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return function stop() {
    clearInterval(interval);
  };
});
```

## [Derived Stores](https://learn.svelte.dev/tutorial/derived-stores)

Derived stores are created based on the value of one or more other stores using the `derived` function. For example, an `elapsed` store is derived from the `time` store to represent how long the current page has been open.

```javascript
// stores.js
import { derived } from "svelte/store";

export const elapsed = derived(time, ($time) =>
  Math.round(($time - start) / 1000)
);
```

## [Custom Stores](https://learn.svelte.dev/tutorial/custom-stores)

Custom stores allow the creation of domain-specific logic. The example introduces a custom store named `createCount` in `stores.js`. This store includes methods like `increment`, `decrement`, and `reset`, avoiding direct exposure of `set` and `update`.

```javascript
// stores.js
import { writable } from "svelte/store";

export function createCount() {
  const { subscribe, set, update } = writable(0);

  return {
    subscribe,
    increment: () => update((n) => n + 1),
    decrement: () => update((n) => n - 1),
    reset: () => set(0),
  };
}
```

## [Store Bindings](https://learn.svelte.dev/tutorial/store-bindings)

Writable stores can be bound to their values, similar to local component state. The example in `App.svelte` demonstrates binding to a writable store named `name` and a derived store named `greeting`.

```html
<!-- App.svelte -->
<script>
  import { name, greeting } from './stores.js';
</script>

<input bind:value={$name}>
<button on:click={() => $name += '!'}>Add exclamation mark!</button>
```

Assignments like `$name += '!'` are equivalent to `name.set($name + '!')`.
