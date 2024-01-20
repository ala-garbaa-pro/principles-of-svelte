# Actions

## Table of Contents
- [Introduction](#introduction)
- [Trapping Focus](#trapping-focus)
- [Adding Parameters to Actions](#adding-parameters-to-actions)

---

## Introduction

Actions in Svelte are element-level lifecycle functions that can be used for various purposes, such as interfacing with third-party libraries, handling lazy-loaded images, creating tooltips, and adding custom event handlers.

To learn more about actions, visit [Svelte Tutorial - Actions](https://learn.svelte.dev/tutorial/actions).

## Trapping Focus

In the following example, we'll explore how to use the `trapFocus` action to ensure keyboard focus is trapped inside a modal menu.

### App.svelte

```html
<script>
	import Canvas from './Canvas.svelte';
	import { trapFocus } from './actions.js';

	const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet', 'white', 'black'];
	let selected = colors[0];
	let size = 10;

	let showMenu = true;
</script>

<div class="menu" use:trapFocus>
  <!-- Menu content goes here -->
</div>
```

### actions.js

```javascript
focusable()[0]?.focus();

node.addEventListener('keydown', handleKeydown);

return {
	destroy() {
		node.removeEventListener('keydown', handleKeydown);
		previous?.focus();
	}
};
```

By applying the `trapFocus` action, the menu now correctly traps focus, allowing users to cycle through options using the Tab key.

## Adding Parameters to Actions

Actions can accept parameters, just like transitions and animations. In this example, we'll add a tooltip to a button using the Tippy.js library.

### App.svelte

```html
<script>
  import { tippy } from 'tippy.js';
  import 'tippy.js/dist/tippy.css';
  import { tooltip } from './actions.js';

  const content = 'Tooltip Content';

  // ...
</script>

<button use:tooltip={{ content, theme: 'material' }}>
  Hover me
</button>
```

### actions.js

```javascript
function tooltip(node, options) {
	const tooltip = tippy(node, options);

	return {
		update(options) {
			tooltip.setProps(options);
		},
		destroy() {
			tooltip.destroy();
		}
	};
}
```

With the `tooltip` action, the button now displays a tooltip with the specified content and theme. The action also includes an `update` method to handle changes in tooltip content.
