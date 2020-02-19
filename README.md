# typescript-playground-plugin-svelte

Easily create a TypeScript [Playground Plugin](https://www.typescriptlang.org/v2/dev/playground-plugins/) with [Svelte](https://svelte.dev/).

_This project is very much a work in progress. Contributions are welcomed!_

## Table Of Contents

1. [Inspiration](#inspiration)
2. [How To Use This Starter](#how-to-use-this-starter)
3. [Props](#props)
4. [Styling Your Plugin](#styling-your-plugin)
5. [More about TypeScript Playground Plugins](#more-about-typescript-playground-plugins)

## Inspiration

The TypeScript Playground V2 comes packed with all sorts of nice features, including the ability to create plugins. Per the TypeScript docs:

> The new TypeScript Playground allows people to hook into the Playground and extend it in ways in which the TypeScript team don't expect.
>
> The sidebar of the Playground uses the same plugin infrastructure as external plugins, so you have the same level of access as the playground to build interesting projects.
>
> Playground plugins have no fancy frameworks, you're free to inject them at runtime and use them if you need to - but the current plugins are built with the DOM APIs and TypeScript.

Since Svelte can be compiled down to dependency-free JavaScript classes, it makes as a great fit as an alternative to the DOM APIs. This project allows you to use Svelte in addition to the DOM APIs to create a rich, interactive UI for your plugin.

## How To Use This Starter

### Step 1. Clone this repo and navigate to the directory

```sh
git clone git@github.com:gojutin/typescript-playground-plugin-svelte.git
```

```sh
cd typescript-playground-plugin-svelte
```

### Step 2. Download dependencies

```sh
npm install
```

### Step 3. Start the development server

```sh
npm start
```

This will start a development server with live reloading of your plugin.

### Step 4. Configure and use your plugin

You can further customize your plugin by modifying the `customPlugin` object in `src/index.ts`. For instance, change the `displayName` property to change the label of the tab for your plugin.

Visit [https://www.typescriptlang.org/v2/en/play](https://www.typescriptlang.org/v2/en/play).

Select the **Options** tab and tick the box for **Connect to localhost:5000/index.js**.

<img src="./screenshots/screenshot1.png" style="max-width: 80%;"/>

Now, **refresh the browser**. When the playground loads, a new tab with your plugin name should appear.

<img src="./screenshots/screenshot2.png" style="max-width: 80%;"/>

**_Note: In order to see your changes, the browser will need to be reloaded each time you make changes to the plugin._**

## Props

The TypeScript Playground Plugin API provides lifecycle methods that are used to interact with the playground. This library uses writable Svelte store objects to provide the values provided by these lifecycle methods to the Svelte app via props. The following props are provided to your Svelte app and are named based on the lifecycle method that they are generated from:

`didMount`, `modelChanged`, `modelChangedDebounce`, `willUnmount`, and `didUnmount`

You can access them in `App.svelte` like so:

```html
<script>
  export let didMount;
  export let modelChanged;
  export let modelChangedDebounce;
  export let willUnmount;
  export let didUnmount;

  // auto-subscribe
  const { sandbox } = $modelChanged;
</script>
```

### `didMount`, `willUnmount`, and `didUnmount`

Runs once, Returns `{container, sandbox}`

- `container`

  This is the root container element that your Svelte app is mounted to.

- `sandbox`

  This object provides several properties and methods to interact with the playground. See all of the available types in `src/vendor/sandbox.d.ts`.

### `modelChanged`

Runs on model change, Returns `{sandbox, model}`

### `modelChangedDebounce`

Runs with delay on model change, Returns `{sandbox, model}`

The `modelChanged` and `modelChangedDebounce` lifecycle methods provide the updated state of the playground as it changes. The return values of both methods are passed into the Svelte app as props via a writable store instance. You can easily auto-subscribe to these values to be notified when any changes occur.

App.svelte

```html
<script>
  export let modelChanged;
  export let modelChangedDebounce;

  // Auto-subscribe to any changes
  $: {
    const { sandbox, model } = $modelChanged;
    console.log(sandbox.getText(), model);
  }

  $: {
    const { sandbox, model } = $modelChangedDebounce;
    console.log(sandbox.getText(), model);
  }
</script>
```

## Styling your plugin

Style you Svelte components as normal. All styles defined in your Svelte components are automatically injected into the page at render time.

## More about TypeScript Playground Plugins

This repo also contains the `CONTRIBUTING.md` guide provided by the TypeScript team. It includes more information about how to interact with the playground.
