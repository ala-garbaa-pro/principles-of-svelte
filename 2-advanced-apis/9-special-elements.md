# Special

## Table of Contents
- [Svelte Self](#svelte-self)
- [Svelte Component](#svelte-component)
- [Svelte Element](#svelte-element)
- [Svelte Window](#svelte-window)
- [Svelte Window Bindings](#svelte-window-bindings)
- [Svelte Body](#svelte-body)
- [Svelte Document](#svelte-document)
- [Svelte Head](#svelte-head)
- [Svelte Options](#svelte-options)
- [Svelte Fragment](#svelte-fragment)

---

## Svelte Self

[Svelte Self Tutorial](https://learn.svelte.dev/tutorial/svelte-self)

Svelte provides a variety of built-in elements. The first, `<svelte:self>`, allows a component to contain itself recursively.

It's useful for things like this folder tree view, where folders can contain other folders. In `Folder.svelte` we want to be able to do this...

```svelte
{#if file.files}
    <svelte:self {...file}/>
{:else}
    <File {...file}/>
{/if}
```

We're coming to the end of part two of the tutorial. This section is a little bit of a mishmash, we've got a bunch of different special elements that Svelte provides to your components. And they don't really have anything in common with each other, other than the fact that they belong to the Svelte namespace.

So the first of these is Svelte self, and that is something that allows a component to reference itself recursively. And it's useful things like this folder tree view, where a folder can contain another folder. So inside our `Folder.svelte` component, we would wanna be able to do something like this...

```svelte
{#if file.files}
    <svelte:self {...file}/>
{:else}
    <File {...file}/>
{/if}
```

## Svelte Component

[Svelte Component Tutorial](https://learn.svelte.dev/tutorial/svelte-component)

A component can change its type altogether with `<svelte:component>`. In this exercise, we want to show `RedThing.svelte` if the color is red, `GreenThing.svelte` if it's green, and so on.

```svelte
<select bind:value={selected}>
    {#each options as option}
        <option value={option}>{option.color}</option>
    {/each}
</select>

<svelte:component this={selected.component}/>
```

Now, when we change the value of that via the select, we're rendering a different component.

## Svelte Element

[Svelte Element Tutorial](https://learn.svelte.dev/tutorial/svelte-element)

Similarly, we don't always know in advance what kind of DOM element to render. `<svelte:element>` comes in handy here. As with the previous exercise, we can replace a long sequence of if blocks with a single dynamic element:

```svelte
<select bind:value={selected}>
    {#each options as option}
        <option value={option}>{option}</option>
    {/each}
</select>

<svelte:element this={selected}>
    I'm a <code>&lt;{selected}&gt;</code> element
</svelte:element>
```

## Svelte Window

[Svelte Window Tutorial](https://learn.svelte.dev/tutorial/svelte-window)

Just as you can add event listeners to any DOM element, you can add event listeners to the window object with `<svelte:window>`.

```svelte
<svelte:window on:keydown={handleKeydown} />
```

We've already seen how you can add event listeners to DOM elements. We can also add event listeners to the window object using the special Svelte window element. In this exercise, whenever we focus the window and press a key, we would like to show what key was pressed. We're gonna assign these key and key code values, and then we're gonna show that inside the app.

All right, and this is just a more convenient way of using window.addEventListener. It will automatically remove event listeners when this component is unmounted. And just like with DOM elements, you can use event modifiers like preventDefault.

## Svelte Window Bindings

[Svelte Window Bindings Tutorial](https://learn.svelte.dev/tutorial/svelte-window-bindings)

We can also bind to certain properties of window, such as `scrollY`:

```svelte
<svelte:window bind:scrollY={y} />
```

The list of properties you can bind to is as follows:

- innerWidth
- innerHeight
- outerWidth
- outerHeight
- scrollX
- scrollY
- online — an alias for window.navigator.onLine

All except `scrollX` and `scrollY` are readonly.

As well as adding events to Svelte window, we bind to certain properties of it.

```svelte
<svelte:window bind:scrollY={y} />
```

This is useful for displaying a message like, "you lost the Internet, you need to reconnect to WiFi" or something like that.

## Svelte Body

[Svelte Body Tutorial](https://learn.svelte.dev/tutorial/svelte-body)

Similar to `<svelte:window>`, the `<svelte:body>` element allows you to listen for events that fire on `document.body`. This is useful with the `mouseenter` and `mouseleave` events, which don't fire on window.

```svelte
<svelte:body
    on:mouseenter={() => hereKitty = true}
    on:mouseleave={() => hereKitty = false}
/>
```

As well as the window, we have a way of adding event listeners to the body element. Which you typically are not rendering yourself because normally you would mount your cell components inside the body, you don't control the actual body.

So we have svelte body to listen to events like mouseenter and mouseleave. And mouseenter, we want this hereKitty value to become true. And we'll just copy that line and reverse it on mouseleave, it becomes false. And then now, if we enter the iframe, kitty comes out to play.

## Svelte Document

[Svelte Document Tutorial](https://learn.svelte.dev/tutorial/svelte-document)

The `<svelte:document>` element allows you to listen for events that fire on `document`. This is useful with events like `selectionchange`, which doesn't fire on window.

```svelte
<svelte:document on:selectionchange={handleSelectionChange} />
```

Similarly, we have the Svelte document element, which is useful for the `selectionchange` handler in particular which does not fire on the window.

## Svelte Head

[Svelte Head Tutorial](https://learn.svelte.dev/tutorial/svelte-head)

The `<svelte:head>` element allows you to insert elements inside the `<head>` of your document. This is useful for things like `<title>` and `<meta>` tags, which are critical for good SEO.

```svelte
<svelte:head>
    <link rel="stylesheet" href="/stylesheets/{selected}.css" />
</svelte:head>
```

And then, we can also add a Svelte head element. Which is very useful for things like SEO, you can add a document title. You can add descriptions and things like that that will appear in search results pages. That

's what it's mostly useful for, although it's not visible in the context of this tutorial.

So we're gonna use something a little bit different. We're gonna use it to load a stylesheet. Add the Svelte head element. And then inside the link rel equals stylesheet. And we have a selection of stylesheets up here that we can pick from. Beginning with the Margaritaville theme, and you can pick whichever of these you must enjoy.

## Svelte Options

[Svelte Options Tutorial](https://learn.svelte.dev/tutorial/svelte-options)

The `<svelte:options>` element allows you to specify compiler options.

```svelte
<Todo.svelte>

<svelte:options immutable={true} />
```

Now, when you toggle todos by clicking on them, only the updated component flashes.

The options that can be set here are:

- `immutable={true}` — you never use mutable data, so the compiler can do simple referential equality checks to determine if values have changed
- `immutable={false}` — the default. Svelte will be more conservative about whether or not mutable objects have changed
- `accessors={true}` — adds getters and setters for the component's props
- `accessors={false}` — the default
- `namespace="..."` — the namespace where this component will be used, most commonly "svg"
- `customElement="..."` — the name to use when compiling this component as a custom element

Consult the API reference for more information on these options.

## Svelte Fragment

[Svelte Fragment Tutorial](https://learn.svelte.dev/tutorial/svelte-fragment)

The `<svelte:fragment>` element allows you to place content in a named slot without wrapping it in a container DOM element.

```svelte
<svelte:fragment slot="game">
    {#each squares as square, i}
        <button
            class="square"
            class:winning={winningLine?.includes(i)}
            disabled={square}
            on:click={() => {
                squares[i] = next;
                next = next === 'x' ? 'o' : 'x';
            }}
        >
            {square}
        </button>
    {/each}
</svelte:fragment>
```

Okay, earlier we learned about using slots to pass content into a component. In this exercise, we're making a Tic-Tac-Toe game.

And we have a board component, and inside.boardcomponent, we're passing some content which is supposed to appear in each of the grid cells defined by the board. So if you look inside the board component, we are creating a grid, which has a certain number of columns and a certain number of rows.

But what's happening here is because we have, a div with the slot equals game attribute, we're putting all of the buttons inside the first element and it's not being spread out into the rest of the grid. What we wanna have is these buttons as direct children of the div class = board.

And we can do that by replacing that div with a Svelte fragment. All right, and so now the buttons that we're declaring here are direct children of the div class equals board.