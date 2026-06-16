# Creating and Using Components

This is how we create and use a component.

```js
function Greeting() {
  return <h2>My First Component</h2>;
}

function App() {
  return <Greeting />;
}
```

```js
// arrow function also works
const Greeting = () => {
  return <h2>My First Component</h2>;
};

function App() {
  return <Greeting />;
}
```

---

**Notes**:

- A component must start with capital letter
- A component must return JSX (html)
- When we use the components, we can either use them as a self-closing tag `<Greeting/>` or like this: `<Greeting>...<Greeting/>`

JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`.

For historical reasons, `aria-*` and `data-*` attributes are written as in HTML with dashes.

It's recommended to use a [converter](https://transform.tools/html-to-jsx) to translate the existing HTML and SVG to JSX.

---
