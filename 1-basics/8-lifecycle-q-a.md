# Lifecycle Q&A Continued

1. [**Are Bindings Synchronous?**](#are-bindings-synchronous)
2. [**Using `onMount` with Asynchronous Code**](#using-onmount-with-asynchronous-code)
3. [**Performance Impact of `onMount` in Waterfall Scenarios**](#performance-impact-of-onmount-in-waterfall-scenarios)
4. [**Handling Multiple `await` Blocks in HTML**](#handling-multiple-await-blocks-in-html)
5. [**Understanding `tick` Function**](#understanding-tick-function)
6. [**Reference to `this` in Event Handlers**](#reference-to-this-in-event-handlers)
7. [**Multiple Calls to `await tick` in the Same Code Block**](#multiple-calls-to-await-tick-in-the-same-code-block)
8. [**Comparison of `tick` with React's `useLayoutEffect`**](#comparison-of-tick-with-reacts-uselayouteffect)


## Are bindings synchronous?

Bindings update the state synchronously. When you have a bind value on a text input, it is akin to having an `on:input` event handler, and the assignment to the value occurs inside that event handler. However, like all state updates, it will be batched, and the DOM won't update until the current microtask is complete.

## Using `onMount` with asynchronous code

It's important to note that `onMount` callbacks should be synchronous functions and should not return a promise. If you need to use asynchronous code inside `onMount`, you can create an immediately invoked function expression (IIFE) that is asynchronous and call it immediately inside `onMount`. However, this is generally discouraged, and it's recommended to use SvelteKit for proper data fetching.

## Performance impact of `onMount` in waterfall scenarios

If you perform asynchronous work inside `onMount` and have a waterfall effect with nested components, it can lead to suboptimal performance. Avoid doing data fetching in `onMount` to prevent waterfall problems. Asynchronous data fetching is better handled in SvelteKit, which also works with server-side rendering.

## Handling multiple `await` blocks in HTML

If you have multiple `await` blocks in your HTML, it might be better to organize and handle complex asynchronous logic in your script block or an external module. While you can use inline `Promise.all` for multiple `await` blocks, it's advisable to keep intricate async logic outside the HTML.

## Understanding `tick` function

The `tick` function is used to ensure proper handling of text selection changes in a textarea. When you assign to a value in a Svelte component, the compiler instruments the assignment with an internal function called `invalidate`. `tick` returns a promise that resolves when any pending state updates are applied to the DOM, allowing you to control the timing of code execution in relation to the DOM updates.

## Reference to `this` in event handlers

In event handlers like `handleKeydown`, `this` refers to the element that the event handler is bound to. It is the event target, providing a convenient way to interact with the DOM imperatively.

## Multiple calls to `await tick` in the same code block

Typically, you should not need to call `await tick` multiple times in the same code block. However, in complex scenarios where you have state updates and need to wait for specific conditions, it might be necessary. It's not a common use case and should be approached with caution.

## Comparison of `tick` with React's `useLayoutEffect`

`tick` in Svelte is somewhat comparable to React's `useLayoutEffect` in terms of taking some measurement of the DOM or responding to DOM updates. However, they have differences in how they guarantee the timing of callback execution after the DOM updates. `tick` ensures that the callback happens immediately after the DOM update, similar to `useLayoutEffect`.