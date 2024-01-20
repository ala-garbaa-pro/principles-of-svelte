# Events

# Table of Contents

- [Events](#events)
  - [DOM Events](#dom-events)
    - [Inline Event Handlers](#inline-event-handlers)
    - [Event Modifiers](#event-modifiers)
  - [Component Events](#component-events)
  - [Event Forwarding](#event-forwarding)
  - [DOM Event Forwarding](#dom-event-forwarding)


## [DOM Events](https://learn.svelte.dev/tutorial/dom-events)

```svelte
<script>
	let m = { x: 0, y: 0 };

	function handleMove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}
</script>

<div on:pointermove={handleMove}>
	The pointer is at {m.x} x {m.y}
</div>

<style>
	div {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		padding: 1rem;
	}
</style>
```

In this example, we explore handling DOM events in Svelte using the `on` directive. By attaching the `on:pointermove` event to a `div` element, we update the pointer's coordinates using the `handleMove` function. Additionally, the inline event handler approach is demonstrated.

```svelte
<script>
	let m = { x: 0, y: 0 };
</script>

<div
	on:pointermove={(e) => {
		m = { x: e.clientX, y: e.clientY };
	}}
>
	The pointer is at {m.x} x {m.y}
</div>

<style>
	div {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		padding: 1rem;
	}
</style>
```

## [Inline Event Handlers](https://learn.svelte.dev/tutorial/inline-handlers)


```svelte
<button on:click|once={() => alert('clicked')}>
	Click me
</button>

## [DOM Events](https://learn.svelte.dev/tutorial/dom-events)

```

## [Event modifiers](https://learn.svelte.dev/tutorial/component-events)

DOM event handlers can have modifiers that alter their behavior. For example, a handler with a `once` modifier will only run a single time:

```svelte
<!-- App.svelte -->
<button on:click|once={() => alert('clicked')}>
    Click me
</button>
```

The full list of modifiers:

- `preventDefault` — calls `event.preventDefault()` before running the handler. Useful for client-side form handling, for example.
- `stopPropagation` — calls `event.stopPropagation()`, preventing the event from reaching the next element.
- `passive` — improves scrolling performance on touch/wheel events (Svelte will add it automatically where it's safe to do so).
- `nonpassive` — explicitly set `passive: false`.
- `capture` — fires the handler during the capture phase instead of the bubbling phase.
- `once` — remove the handler after the first time it runs.
- `self` — only trigger the handler if `event.target` is the element itself.
- `trusted` — only trigger the handler if `event.isTrusted` is true, meaning the event was triggered by a user action rather than because some JavaScript called `element.dispatchEvent(...)`.

You can chain modifiers together, e.g. `on:click|once|capture={...}`.

**App.svelte**
```svelte
<script>
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Inner on:message={handleMessage} />
```

**Inner.svelte**
```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

Components in Svelte can also emit events. Here, `Inner.svelte` dispatches a custom event, and `App.svelte` handles it using the `on:message` directive.

## [Component events](https://learn.svelte.dev/tutorial/component-events)

Components can also dispatch events. To do so, they must create an event dispatcher. Update `Inner.svelte`:

```svelte
<!-- Inner.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    function sayHello() {
        dispatch('message', {
            text: 'Hello!'
        });
    }
</script>
```

`createEventDispatcher` must be called when the component is first instantiated — you can't do it later inside e.g. a `setTimeout` callback. This links `dispatch` to the component instance.

Then, add an `on:message` handler in `App.svelte`:

```svelte
<!-- App.svelte -->
<Inner on:message={handleMessage} />
```

You can also try changing the event name to something else. For instance, change `dispatch('message', {...})` to `dispatch('greet', {...})` in `Inner.svelte` and change the attribute name from `on:message` to `on:greet` in `App.svelte`.


## [Event Forwarding](https://learn.svelte.dev/tutorial/event-forwarding)

**App.svelte**
```svelte
<script>
	import Outer from './Outer.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Outer on:message={handleMessage} />
```

**Outer.svelte**
```svelte
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message />
```

**Inner.svelte**
```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

In scenarios where component events do not automatically bubble up, event forwarding is employed. The outer component (`Outer.svelte`) acts as an intermediary, forwarding the `message` event from the inner component (`Inner.svelte`) to the parent component (`App.svelte`).

## [DOM Event Forwarding](https://learn.svelte.dev/tutorial/dom-event-forwarding)

**App.svelte**
```svelte
<script>
	import BigRedButton from './BigRedButton.svelte';
	import horn from './horn.mp3';

	const audio = new Audio();
	audio.src = horn;

	function handleClick() {
		audio.load();
		audio.play();
	}
</script>

<BigRedButton on:click={handleClick} />
```

**BigRedButton.svelte**
```svelte
<button on:click>
	Push
</button>

<style>
	button {
		/* ... Styles ... */
	}
</style>
```

**horn.mp3**
```mp3
SUQzAwAAAAAAWlRFT...
```

This example demonstrates event forwarding for DOM events. The `BigRedButton.svelte` component forwards the `click` event to the parent component (`App.svelte`), where it is handled by the `handleClick` function, triggering the play of an audio file.

---

**Note**: Forwarding is specific to events and does not apply to callbacks. Callbacks must be explicitly passed through components in a traditional manner if not using the event system.