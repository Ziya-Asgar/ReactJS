# Suspense and Lazy Loading

- [Suspense and Lazy Loading](#suspense-and-lazy-loading)
  - [Suspense](#suspense)
  - [lazy](#lazy)
  - [Usage](#usage)

---

---

## Suspense

`Suspense` lets you display a fallback until its children have finished loading. The `Suspense` component receives two props, `children` and `fallback`. It then renders `fallback` until all the data required by `children` is available and `children` can be rendered.

By default, the whole tree inside `Suspense` is treated as a single unit. For example, even if only one of these components suspends waiting for some data, all of them together will be replaced by the loading indicator.

Only Suspense-enabled data sources will activate the `Suspense` component. They include:

- Data fetching with Suspense-enabled frameworks like Relay and Next.js
- Lazy-loading component code with `lazy`
- Reading the value of a Promise with `use` (this is still experimental)

`Suspense` does not detect when data is fetched inside an Effect or event handler.

---

---

## lazy

`lazy` lets you defer loading component’s code until it is rendered for the first time. It is also known as on-demand loading because it only loads content visible on users’ screens.

Usually, you import components with the static `import` declaration. To defer loading a component’s code until it’s rendered for the first time, you can use dinamic importing with `lazy` like this:

```js
import { lazy } from "react";

const Component = lazy(() => import("./Component.js"));
```

Using this pattern requires that the lazy component you’re importing was exported as the `default` export.

Now that your component’s code loads on demand, you also need to specify what should be displayed while it is loading. You can do this by wrapping the `lazy` component or any of its parents into a `<Suspense>` boundary.

Do not declare lazy components inside other components. This will cause all state to be reset on re-renders. Instead, always declare them at the top level of your module.

---

---

## Usage

```js
import React, { Suspense } from "react";

// Import the `MyComponent` module lazily, so it doesn't get loaded until it is actually needed.
const MyComponent = React.lazy(() => import("./MyComponent"));

function App() {
  return (
    <div>
      <h1>Welcome to my app</h1>

      {/* Use the `Suspense` component with a fallback prop, 
      which will be displayed while the lazy-loaded component is being fetched */}
      <Suspense fallback={<div>Loading...</div>}>
        <MyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

---

---
