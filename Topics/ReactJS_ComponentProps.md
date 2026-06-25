# Component with Props

- [Component with Props](#component-with-props)
  - [`children` prop](#children-prop)
  - [Prop Types](#prop-types)

---

---

This is how to create a component that accepts _props_ and how to use it:

```js
/* We create a component that accepts props */
const Book = (props) => {
  console.log(props);
  return (
    <article className="book">
      <h2>{props.title}</h2>
      <h4>{props.author}</h4>
    </article>
  );
};

/* We use the component and while using we pass the properties to the component */
function BookList() {
  return (
    <section className="booklist">
      <Book title="Chemistry" />
      <Book title="random title" author="some author />
    </section>
  );
}
```

---

Instead of accepting properties into the component as _props_, we can destructure the _props_ inside the component.

```js
const Book = (props) => {
  const { title, author } = props;
  return (
    <article className="book">
      <h2>{title}</h2>
      <h4>{author}</h4>
    </article>
  );
};
```

We can also destructure in function parameters, like this:

```js
const Book = ({ title, author }) => {
  return (
    <article className="book">
      <h2>{title}</h2>
      <h4>{author} </h4>
    </article>
  );
};
```

If you want to give a default value to a prop, you can do it like this:

```js
function Book({ title, author = "Anonymous" }) {
  // ...
}
```

The default value, in the above example, is only used if the author prop is missing or if you pass `author={undefined}`. But if you pass `author={null}` or `author={0}`, the default value will not be used.

---

We can also provide the props to the component using the spread operator.

Let's say we have this component:

```js
// HouseListing.jsx
export default function HouseListing(props) {
  return (
    <div>
      <p>Host: {props.host}</p>
      <p>Experience: {props.experience}</p>
      <p>Location: {props.location}</p>
      <p>Price: {props.price}</p>
    </div>
  );
}
```

We can declare props one by one:

```js
// Scenario 1: declare every prop

import HouseListing from "./components/HouseListing";

const props = {
  host: "Mario",
  experience: "Walk in the woods",
  location: "Norway",
  price: 29,
};

export default function Listings() {
  return (
    <div>
      <HouseListing
        host={props.host}
        experience={props.experience}
        location={props.location}
        price={props.price}
      />
    </div>
  );
}
```

Or, we can use the spread operator:

```js
// Scenario 2: using the spread operator

import HouseListing from "./components/HouseListing";

const props = {
  host: "Mario",
  experience: "Walk in the woods",
  location: "Norway",
  price: 29,
};

export default function Listings() {
  return (
    <div>
      <HouseListing {...props} />
    </div>
  );
}
```

---

---

## `children` prop

When we use the components like this `<component> ... </component>`, everything between tags can be accessed through the `children` property. In the below example, we create the _book_ component that expects _title_ and `children` as part of props. Then we render them inside an `article` tag. When we use the _book_ component, we provide a value for the _title_, and everything that we put between the opening and closing _book_ tags become `children` of the _book_ component.

```js
const Book = (props) => {
  const { title, children } = props;
  console.log(props);
  return (
    <article className="book">
      <h2>{title}</h2>
      {children}
    </article>
  );
};

function BookList() {
  return (
    <section className="booklist">
      <Book title="Some Book title">
        <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit.</p>
        <button>click me</button>
      </Book>
    </section>
  );
}
```

---

---

## Prop Types

React will ask to set static types for props. If we want React to stop doing that, and if we use Vite, we can add the below code to the `rules` part of the **.eslintrc.cjs** file:

```js
// .eslintrc.cjs
// ...
rules: {
    // ...
    "react/prop-types": "off",
    // ...
  },
```

But in case, we want to set React Prop Types, here is how we can do it using the `prop-types` library (React recommends using TypeScript instead of checking prop types at runtime).

First, we need to install `prop-types`:

```bash
npm i prop-types
```

Then, we import it in a component, where we declare what type of prop the component expects.

To declare the prop types, we populate the `<ComponentName>.propTypes` object. This object outlines all the props for the component.

Here is a simple example, where we set various props and indicate what type of data they must have:

```js
import PropTypes from "prop-types";

const Component = ({
  strProp,
  numProp,
  boolProp,
  funcProp,
  arrProp,
  objProp,
  symProp,
}) => {
  return (
    <div>
      <p>This is a string property: {strProp}</p>
      <p>This is a number property: {numProp}</p>
      <p>This is a boolean property: {boolProp}</p>
      <p>This is a function property: {funcProp}</p>
      <p>This is an array property: {arrProp}</p>
      <p>This is a object property: {objProp}</p>
      <p>This is a symbol property: {symProp}</p>
    </div>
  );
};

Component.propTypes = {
  strProp: PropTypes.string,
  numProp: PropTypes.number,
  boolProp: PropTypes.bool,
  funcProp: PropTypes.func,
  arrProp: PropTypes.array,
  objProp: PropTypes.object,
  symProp: PropTypes.symbol,
};

export default Component;
```

After the above code, if a parent component supplies a value that is in a different type from the indicated ones, then the app will show an error in the console.

But we won't have an error if the props are not supplied. To indicate that a a prop must be supplied, we can use the `PropTypes.<type>.isRequired`:

```js
import PropTypes from "prop-types";

const Component = ({ strProp, numProp }) => {
  return (
    <div>
      <p>This is a string property: {strProp}</p>
      <p>This is a number property: {numProp}</p>
    </div>
  );
};

Component.propTypes = {
  strProp: PropTypes.string.isRequired,
  numProp: PropTypes.number,
};

export default Component;
```

There are also React-specific things that we can indicate. For example, if we want to know if a prop is renderable, then we can use `PropTypes.node`. An object is not renderable, and if we accidentally pass an object as a prop, then the `PropTypes.node` will show an error:

```js
import PropTypes from "prop-types";

const Component = ({ renderableReactNode }) => {
  return (
    <div>
      <p>This is a renderable property: {renderableReactNode}</p>
    </div>
  );
};

Component.propTypes = {
  renderableReactNode: PropTypes.node,
};

export default Component;
```

If we don't want to specify any type for a prop, but want to make it required, then we can chain `any` and `.isRequired`:

```js
import PropTypes from "prop-types";

const Component = ({ anyProp }) => {
  return (
    <div>
      <p>This is a any property: {anyProp}</p>
    </div>
  );
};

Component.propTypes = {
  anyProp: PropTypes.any.isRequired,
};

export default Component;
```

If we want the prop to receive a React component, we can indicate that we expect to receive `PropTypes.element`:

```js
import PropTypes from "prop-types";

const Component = ({ compProp }) => {
  return (
    <div>
      <p>This is a component property: {compProp}</p>
    </div>
  );
};

Component.propTypes = {
  compProp: PropTypes.element,
};

export default Component;
```

Chaining `PropTypes.element` with `.isRequired` makes sure to indicate that we must receive a single component (not more and not less):

```js
import PropTypes from "prop-types";

const Component = ({ singleCompProp }) => {
  return (
    <div>
      <p>This is a single component property: {singleCompProp}</p>
    </div>
  );
};

Component.propTypes = {
  singleCompProp: PropTypes.element.isRequired,
};

export default Component;
```

If we don't want to indicate a single prop type, and instead want a prop to be one of a few different types, then we can use the `oneOfType()` method. This method accepts an array where we can specify the types that we want the prop to be:

```js
import PropTypes from "prop-types";

const Component = ({ strOrNumProp }) => {
  return (
    <div>
      <p>This is either a string or a number property: {strOrNumProp}</p>
    </div>
  );
};

Component.propTypes = {
  strOrNumProp: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),
};

export default Component;
```

If we want the value of our prop to be one of the specified values, we can use the `oneOf()` method. This method accepts an array where we can specify which values the prop can have:

```js
import PropTypes from "prop-types";

const Component = ({ stateProp }) => {
  return (
    <div>
      <p>This property is either in a loading or a ready state: {stateProp}</p>
    </div>
  );
};

Component.propTypes = {
  stateProp: PropTypes.oneOf(["Loading", "Ready"]),
};

export default Component;
```

We noted that we can specify that a prop must be an array using the `PropTypes.array`. However, if we want the array to consist of specific type of data, we can use the `arrayOf()` method.

```js
import PropTypes from "prop-types";

const Component = ({ arrayProp }) => {
  return (
    <div>
      <p>This property is an array of numbers: {arrayProp}</p>
    </div>
  );
};

Component.propTypes = {
  arrayProp: PropTypes.arrayOf(PropTypes.number),
};

export default Component;
```

In addition to using the `propTypes` property of a component, we can also use the `defaultProps` property to set some default values for our props:

```js

```

We can use the `shape` to specify objects:

---

---
