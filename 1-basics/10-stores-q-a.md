# Stores Q&A

## Table of Contents
- [Fetching Data in Readable Stores](#fetching-data-in-readable-stores)
- [Persisting Svelte Store Data](#persisting-svelte-store-data)
- [Store Differentiation from Redux](#store-differentiation-from-redux)
- [Comparison to Context in React](#comparison-to-context-in-react)
- [Usage of God Derived Store](#usage-of-god-derived-store)
- [Reactive Statements and Cleanup](#reactive-statements-and-cleanup)
- [Production Examples of Stores](#production-examples-of-stores)

---


## [Fetching Data in Readable Stores](https://learn.svelte.dev/tutorial/readable-stores)

### Q: What if I fetch data directly in a readable store? Is it good or better to fetch and parse data in the store in a component?

A: You can fetch data inside a readable store, especially for scenarios like polling where doing the logic inside the readable store callback is a good pattern. It's recommended to include the polling logic within the readable store callback, and remember to stop polling once the unsubscribe happens.

---

## [Persisting Svelte Store Data](https://learn.svelte.dev/tutorial/readable-stores)

### Q: Is there a recommended way to persist Svelte store data upon page refresh? Why is there no native way to persist stores in Svelte?

A: Svelte aims to be flexible and not impose specific opinions on how to use stores. While local storage and other storage solutions can be implemented with custom logic, there are packages available, such as "Svelte Local Storage Store," for those who prefer a pre-built solution. Svelte allows developers to build their own abstractions on top of stores, providing flexibility for different use cases.

---

## [Store Differentiation from Redux](https://learn.svelte.dev/tutorial/readable-stores)

### Q: How does Svelte store differentiate from Redux?

A: While both use the term "store," they serve different purposes. Redux stores are often application-level God objects defining state for the entire application. In contrast, Svelte stores are atomic pieces of state that can be created inside a component or as application-wide modules. Svelte stores typically represent one specific piece of state, like a count or the current value of a fetch operation, while Redux stores may include various pieces of state.

---

## [Comparison to Context in React](https://learn.svelte.dev/tutorial/readable-stores)

### Q: Would it be more fair to compare Svelte stores to a context in React?

A: Svelte does have a concept of context, but there isn't a direct equivalent in React. While Zustand and Recoil in React share some similarities with Svelte stores, context in Svelte will be discussed later in the tutorial.

---

## [Usage of God Derived Store](https://learn.svelte.dev/tutorial/readable-stores)

### Q: Have you ever found it useful to have a God-derived store with everything, or do you prefer to keep it all as atomic stores?

A: Personally, the preference is for atomic stores rather than a God-derived store. While Redux may have a concept of a single store for all state, Svelte stores typically represent tangible, first-class things, making it more intuitive to manage state.

---

## [Reactive Statements and Cleanup](https://learn.svelte.dev/tutorial/readable-stores)

### Q: Reactive statements seem like the closest thing to React `useEffect`, but there doesn't seem to be any way to clean up functions in Svelte. Is that true? Any thoughts about adding support for cleanup functions in a reactive statement?

A: Svelte provides stores for managing state-level lifecycle, and they are not bound to the component hierarchy in the same way as `useEffect` in React. If cleanup is required, lifecycle functions can be used if bound to the component lifecycle. For state-related cleanup, Svelte stores are the recommended approach.

---

## [Production Examples of Stores](https://learn.svelte.dev/tutorial/readable-stores)

### Q: Can you give a couple of production examples of stores in use?

A: The Svelte REPL (the app being used in the tutorial) itself is an example of a production application using stores extensively. In this app, stores are used to represent various pieces of state, such as the selected file, the application's completion state, and more. For a broader range of production use cases, it's recommended to explore the Svelte Discord community, where developers share their experiences and use cases with stores.