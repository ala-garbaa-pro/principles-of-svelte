# Lifecycle

# Table of Contents

- [Lifecycle](#lifecycle)
  - [OnMount](#onmount)
  - [BeforeUpdate and AfterUpdate](#beforeupdate-and-afterupdate)
  - [Tick](#tick)



## [OnMount](#onmount)

```svelte
<script>
  import { onMount } from 'svelte';
  import { paint } from './gradient.js';

  onMount(() => {
    const canvas = document.querySelector('canvas');
    const context = canvas.getContext('2d');

    let frame = requestAnimationFrame(function loop(t) {
      frame = requestAnimationFrame(loop);
      paint(context, t);
    });

    return () => {
      cancelAnimationFrame(frame);
    };
  });
</script>

<canvas width={32} height={32} />

<style>
  canvas {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: #666;
    mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
    mask-size: 60vmin;
    -webkit-mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
    -webkit-mask-size: 60vmin;
  }
</style>
```

In this code snippet, the `onMount` lifecycle function is used to animate a canvas element. The `paint` function, defined in the `gradient.js` file, is applied to the canvas in a loop using the `requestAnimationFrame` method. The animation loop is stopped when the component is unmounted to prevent memory leaks.

## [BeforeUpdate and AfterUpdate](https://learn.svelte.dev/tutorial/update)

```svelte
<script>
  import Eliza from 'elizabot';
  import { beforeUpdate, afterUpdate } from 'svelte';

  let div;
  let autoscroll = false;

  beforeUpdate(() => {
    if (div) {
      const scrollableDistance = div.scrollHeight - div.offsetHeight;
      autoscroll = div.scrollTop > scrollableDistance - 20;
    }
  });

  afterUpdate(() => {
    if (autoscroll) {
      div.scrollTo(0, div.scrollHeight);
    }
  });

  const eliza = new Eliza();
  const pause = (ms) => new Promise((fulfil) => setTimeout(fulfil, ms));

  const typing = { author: 'eliza', text: '...' };

  let comments = [];

  async function handleKeydown(event) {
    // ... (code for handling user input)
  }
</script>

<div class="container">
  <div class="phone">
    <div class="chat" bind:this={div}>
      <!-- ... (HTML markup for chat display) -->
    </div>

    <input on:keydown={handleKeydown} />
  </div>
</div>

<style>
  /* ... (styles for the chat interface) */
</style>
```

This code demonstrates the use of `beforeUpdate` and `afterUpdate` lifecycle functions to handle scrolling in a chat interface. The `beforeUpdate` function checks if the chat should be in autoscroll mode, and the `afterUpdate` function scrolls to the bottom of the chat if autoscroll is enabled. These functions ensure proper DOM manipulation after updates.

## [Tick](https://learn.svelte.dev/tutorial/tick)

```svelte
<script>
  import { tick } from 'svelte';

  let text = `Select some text and hit the tab key to toggle uppercase`;

  async function handleKeydown(event) {
    if (event.key !== 'Tab') return;

    event.preventDefault();

    const { selectionStart, selectionEnd, value } = this;
    const selection = value.slice(selectionStart, selectionEnd);

    const replacement = /[a-z]/.test(selection)
      ? selection.toUpperCase()
      : selection.toLowerCase();

    text =
      value.slice(0, selectionStart) +
      replacement +
      value.slice(selectionEnd);

    await tick();
    this.selectionStart = selectionStart;
    this.selectionEnd = selectionEnd;
  }
</script>

<textarea value={text} on:keydown={handleKeydown} />

<style>
  textarea {
    width: 100%;
    height: 100%;
    resize: none;
  }
</style>
```

The `tick` function is utilized in this code to ensure proper handling of text selection changes in a textarea. By awaiting `tick()` after modifying the text content, the subsequent manipulation of the selection range occurs after the DOM has been updated, preserving the user's selection.
