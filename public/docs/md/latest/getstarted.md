# Get Started

Solid is a declarative JavaScript library for creating user interfaces. It does not use a Virtual DOM. Instead it opts to compile its templates down to real DOM nodes and wrap updates in fine grained reactions. This way when your state updates only the code that depends on it runs.

## The Gist

```jsx
import { render } from 'solid-js/web';

const HelloMessage = (props) => <div>Hello {props.name}</div>;

render(() => <HelloMessage name="Taylor" />, document.getElementById('hello-example'));
```

A Simple Component is just a function that accepts properties. Solid uses a `render` function to create the reactive mount point of your application.

The JSX is then compiled down to efficient real DOM expressions:

```js
import { render, template, insert, createComponent } from 'solid-js/web';

const _tmpl$ = template(`<div>Hello </div>`);

const HelloMessage = (props) => {
  const _el$ = _tmpl$.cloneNode(true);
  insert(_el$, () => props.name);
  return _el$;
};

render(
  () => createComponent(HelloMessage, { name: 'Taylor' }),
  document.getElementById('hello-example'),
);
```

That `_el$` is a real div element and `props.name`, `Taylor` in this case, is appended to its child nodes. Notice that `props.name` is wrapped in a function. That is because that is the only part of this component that will ever execute again. Even if a name is updated from the outside only that one expression will be re-evaluated. The compiler optimizes initial render and the runtime optimizes updates. It's the best of both worlds.

Want to see what code Solid generates:

### [Try it Online](https://playground.solidjs.com/)

## Getting Started

> _`npm init solid <project-type> <project-name>` is available with npm 6+._

You can get started with a simple app with the CLI with by running:

```sh
> npm init solid app my-app
```

Or for a TypeScript starter:

```sh
> npm init solid app-ts my-app
```

Or you can install the dependencies in your own project. To use Solid with JSX (recommended) run:

```sh
> npm install solid-js babel-preset-solid
```

The easiest way to get setup is add `babel-preset-solid` to your .babelrc, or babel config for webpack, or rollup:

```js
"presets": ["solid"]
```

For TypeScript remember to set your TSConfig to handle Solid's JSX by:

```js
"compilerOptions" {
  "jsx": "preserve",
  "jsxImportSource": "solid-js",
}
```

> Check out these two introductory articles by [@aftzl](https://github.com/atfzl):
> [Understanding Solid: Reactivity Basics](https://dev.to/atfzl/understanding-solid-reactivity-basics-39kk)<br> > [Understanding Solid: JSX](https://dev.to/atfzl/understanding-solid-jsx-584p)

## Documentation

- [Reactivity](https://github.com/ryansolid/solid/blob/master/documentation/reactivity.md)
- [State](https://github.com/ryansolid/solid/blob/master/documentation/state.md)
- [JSX Rendering](https://github.com/ryansolid/solid/blob/master/documentation/rendering.md)
- [Components](https://github.com/ryansolid/solid/blob/master/documentation/components.md)
- [Styling](https://github.com/ryansolid/solid/blob/master/documentation/styling.md)
- [Context](https://github.com/ryansolid/solid/blob/master/documentation/context.md)
- [Suspense](https://github.com/ryansolid/solid/blob/master/documentation/suspense.md)
- [API](https://github.com/ryansolid/solid/blob/master/documentation/api.md)
- [FAQ](https://github.com/ryansolid/solid/blob/master/documentation/faq.md)
- [Comparison with other Libraries](https://github.com/ryansolid/solid/blob/master/documentation/comparison.md)
- [Storybook](https://github.com/ryansolid/solid/blob/master/documentation/storybook.md)

## Resources

- [Examples](https://github.com/ryansolid/solid/blob/master/documentation/resources/examples.md)
- [Articles](https://github.com/ryansolid/solid/blob/master/documentation/resources/articles.md)
- [Projects](https://github.com/ryansolid/solid/blob/master/documentation/resources/projects.md)

## No Compilation?

Dislike JSX? Don't mind doing manual work to wrap expressions, worse performance, and having larger bundle sizes? Alternatively in non-compiled environments you can use Tagged Template Literals or HyperScript.

You can run them straight from the browser using SkyPack:

```html
<html>
  <body>
    <script type="module">
      import { createSignal, onCleanup } from 'https://cdn.skypack.dev/solid-js';
      import { render } from 'https://cdn.skypack.dev/solid-js/web';
      import html from 'https://cdn.skypack.dev/solid-js/html';

      const App = () => {
        const [count, setCount] = createSignal(0),
          timer = setInterval(() => setCount(count() + 1), 1000);
        onCleanup(() => clearInterval(timer));
        return html`<div>${count}</div>`;
      };
      render(App, document.body);
    </script>
  </body>
</html>
```

Remember you still need the corresponding DOM Expressions library for these to work with TypeScript. Tagged Template Literals [Lit DOM Expressions](https://github.com/ryansolid/dom-expressions/tree/master/packages/lit-dom-expressions) or HyperScript with [Hyper DOM Expressions](https://github.com/ryansolid/dom-expressions/tree/master/packages/hyper-dom-expressions).

## Browser Support

The last 2 versions of modern evergreen browsers and Node LTS.

## Status

Solid is mostly feature complete for its v1.0.0 release. The next releases will be mostly bug fixes and API tweaks on the road to stability.