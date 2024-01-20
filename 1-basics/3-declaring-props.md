# Declaring Props

## Table of Contents

- [Declaring Props](#declaring-props)
    - [Default Values for Props](#default-values-for-props)
    - [Spread Props](#spread-props)

## [Declaring Props](https://learn.svelte.dev/tutorial/declaring-props)

### App.svelte

```svelte
<script>
    import Nested from './Nested.svelte';
</script>

<Nested answer={42} />
```

### Nested.svelte

```svelte
<script>
    let answer;
</script>

<p>The answer is {answer}</p>
```

When building components in Svelte, you often need to pass data between them. This is achieved using properties, known as props. In Svelte, the `export` keyword is used to declare props. In the example above, the `answer` prop is declared in `Nested.svelte` to allow external components to pass data into it.

If you're familiar with JavaScript's import and export syntax, you may find the Svelte syntax a bit different. However, everything inside the script tag remains valid JavaScript syntax, facilitating the use of linters and code formatters.


## [Default Values for Props](https://learn.svelte.dev/tutorial/default-values)


### App.svelte

```svelte
<script>
    import Nested from './Nested.svelte';
</script>

<Nested answer={42} />
<Nested />
```

### Nested.svelte

```svelte
<script>
    export let answer = 'a mystery';
</script>

<p>The answer is {answer}</p>
```

Default values for props can be specified within the component where the prop is declared. In the example above, the `answer` prop in `Nested.svelte` has a default value of 'a mystery.' When used in `App.svelte`, the first `Nested` component receives a specific value for `answer`, while the second one uses the default fallback.


## [Spread Props](https://learn.svelte.dev/tutorial/spread-props)

### App.svelte

```svelte
<script>
    import PackageInfo from './PackageInfo.svelte';

    const pkg = {
        name: 'svelte',
        speed: 'blazing',
        version: 4,
        website: 'https://svelte.dev'
    };
</script>

<PackageInfo {...pkg} />
```

### PackageInfo.svelte

```svelte
<script>
    export let name;
    export let version;
    export let speed;
    export let website;

    $: href = `https://www.npmjs.com/package/${name}`;
</script>

<p>
    The <code>{name}</code> package is {speed} fast. Download version {version} from
    <a {href}>npm</a> and <a href={website}>learn more here</a>
</p>
```

In the example above, the `PackageInfo` component receives props from the `pkg` object. The spread syntax (`{...pkg}`) allows all properties of `pkg` to be passed as individual props to the `PackageInfo` component. This provides a convenient shorthand when the object's properties match the required props of the component.

Additionally, the special `$$props` value can be used to reference all props dynamically within the component, providing a read-only way to access them without explicitly listing each prop. Keep in mind that `$$props` cannot be modified directly. SvelteKit automatically handles prop passing in most cases, and `$$props` is mainly used when more dynamic or automated prop handling is required.