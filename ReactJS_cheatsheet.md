# React JS

- [React JS](#react-js)
  - [Getting Started with ReactJS](#getting-started-with-reactjs)
  - [Strict Mode](#strict-mode)
  - [Creating and Using Components](#creating-and-using-components)
  - [Components with Props](#components-with-props)
  - [Handling Events](#handling-events)
  - [Conditional Rendering](#conditional-rendering)
  - [Rendering Lists](#rendering-lists)
  - [Exporting and Importing](#exporting-and-importing)
  - [Inline Styling](#inline-styling)
  - [Hooks](#hooks)
    - [`useState`](#usestate)
    - [`useReducer`](#usereducer)
    - [`useEffect`](#useeffect)
    - [`useRef`](#useref)
    - [`forwardRef`](#forwardref)
    - [`useImperativeHandle`](#useimperativehandle)
    - [`useMemo`](#usememo)
    - [`useCallback`](#usecallback)
    - [Context API](#context-api)
      - [How to use Context and set value of context in child components](#how-to-use-context-and-set-value-of-context-in-child-components)
  - [Suspense component and lazy](#suspense-component-and-lazy)
    - [Suspense](#suspense)
    - [lazy](#lazy)
    - [Usage](#usage)

<hr>

## Getting Started with ReactJS

[Getting Started with ReactJS](./ReactJS_GettingStarted.md)

<hr>

## Strict Mode

[Strict Mode](./ReatJS_StrictMode.md)

<hr>

## Creating and Using Components

[Creating and Using Components](./ReactJS_CreatingUsingComponents.md)

<hr>

## Components with Props

[Components with Props](./ReactJS_ComponentProps.md)

<hr>

## Handling Events

[Handling Events](./ReactJS_HandlingEvents.md)

<hr>

## Conditional Rendering

[Conditional Rendering](./ReactJS_ConditionalRendering.md)

<hr>

## Rendering Lists

[Rendering Lists](./ReactJS_RenderingLists.md)

<hr>

## Exporting and Importing

[Exporting and Importing](./ReactJS_ExportingImporting.md)

<hr>

## Inline Styling

[Inline Styling](./ReactJS_InlineStyling.md)

<hr>

## Hooks

### `useState`

`useState` hook returns an array with two elements:

- the current state value, and
- a function that we can use to update the state.

While initiating the current state value, it accepts a default value as an argument. State update triggers re-render.

**Example 1**:

```js
import { useState } from "react";

const UseStateBasics = () => {
  const [count, setCount] = useState(0);
  const handleClick = () => {
    setCount(count + 1);
    // be careful, we can set any value
    // setCount('pants');
  };

  return (
    <div>
      <h4>You clicked {count} times</h4>
      <button className="btn" onClick={handleClick}>
        Click me
      </button>
    </div>
  );
};

export default UseStateBasics;
```

<hr>

You may want to update only one property in an object, but keep the previous values for all other properties.

```js
import { useState } from "react";

export default function Form() {
  const [person, setPerson] = useState({
    firstName: "Barbara",
    lastName: "Hepworth",
    email: "bhepworth@sculpture.com",
  });

  function handleFirstNameChange(e) {
    setPerson({
      ...person,
      firstName: e.target.value,
    });
  }

  function handleLastNameChange(e) {
    setPerson({
      ...person,
      lastName: e.target.value,
    });
  }

  function handleEmailChange(e) {
    setPerson({
      ...person,
      email: e.target.value,
    });
  }

  return (
    <>
      <label>
        First name:
        <input value={person.firstName} onChange={handleFirstNameChange} />
      </label>
      <label>
        Last name:
        <input value={person.lastName} onChange={handleLastNameChange} />
      </label>
      <label>
        Email:
        <input value={person.email} onChange={handleEmailChange} />
      </label>
      <p>
        {person.firstName} {person.lastName} ({person.email})
      </p>
    </>
  );
}
```

Note that the `...` spread syntax is “shallow”—it only copies things one level deep. This makes it fast, but it also means that if you want to update a nested property, you’ll have to use it more than once.

```js
import { useState } from "react";

export default function Form() {
  const [person, setPerson] = useState({
    name: "Niki de Saint Phalle",
    artwork: {
      title: "Blue Nana",
      city: "Hamburg",
    },
  });

  function handleNameChange(e) {
    setPerson({
      ...person,
      name: e.target.value,
    });
  }

  function handleTitleChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value,
      },
    });
  }

  function handleCityChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value,
      },
    });
  }

  return (
    <>
      <label>
        Name:
        <input value={person.name} onChange={handleNameChange} />
      </label>
      <label>
        Title:
        <input value={person.artwork.title} onChange={handleTitleChange} />
      </label>
      <label>
        City:
        <input value={person.artwork.city} onChange={handleCityChange} />
      </label>

      <p>
        <i>{person.artwork.title}</i>
        {" by "}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
    </>
  );
}
```

<hr>

Every time you want to update an array, you’ll want to pass a new array to your state setting function. To do that, you can create a new array from the original array in your state by calling its non-mutating methods like `filter()` and `map()`.

```js
import { useState } from "react";

let nextId = 0;

export default function List() {
  const [name, setName] = useState("");
  const [artists, setArtists] = useState([]);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button
        onClick={() => {
          setArtists([...artists, { id: nextId++, name: name }]);
        }}
      >
        Add
      </button>
      <ul>
        {artists.map((artist) => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}
```

The array spread syntax also lets you prepend an item by placing it before the original `...artists`:

```js
setArtists([
  { id: nextId++, name: name },
  ...artists, // Put old items at the end
]);
```

<hr>

The easiest way to remove an item from an array is to filter it out. In other words, you will produce a new array that will not contain that item. To do this, use the `filter` method, for example:

```js
import { useState } from "react";

let initialArtists = [
  { id: 0, name: "Marta Colvin Andrade" },
  { id: 1, name: "Lamidi Olonade Fakeye" },
  { id: 2, name: "Louise Nevelson" },
];

export default function FilterList() {
  const [artists, setArtists] = useState(initialArtists);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <ul>
        {artists.map((artist) => (
          <li key={artist.id}>
            {artist.name}{" "}
            <button
              onClick={() => {
                setArtists(artists.filter((a) => a.id !== artist.id));
              }}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </>
  );
}
```

<hr>

It is particularly common to want to replace one or more items in an array. Assignments like `arr[0] = 'bird'` are mutating the original array, so instead you’ll want to use `map` for this as well.

To replace an item, create a new array with `map`. Inside your `map` call, you will receive the item index as the second argument. Use it to decide whether to return the original item (the first argument) or something else:

```js
import { useState } from "react";

let initialCounters = [0, 0, 0];

export default function CounterList() {
  const [counters, setCounters] = useState(initialCounters);

  function handleIncrementClick(index) {
    const nextCounters = counters.map((c, i) => {
      if (i === index) {
        // Increment the clicked counter
        return c + 1;
      } else {
        // The rest haven't changed
        return c;
      }
    });
    setCounters(nextCounters);
  }

  return (
    <ul>
      {counters.map((counter, i) => (
        <li key={i}>
          {counter}
          <button
            onClick={() => {
              handleIncrementClick(i);
            }}
          >
            +1
          </button>
        </li>
      ))}
    </ul>
  );
}
```

<hr>

Sometimes, you may want to insert an item at a particular position that’s neither at the beginning nor at the end. To do this, you can use the `...` array spread syntax together with the `slice()` method. The `slice()` method lets you cut a “slice” of the array. To insert an item, you will create an array that spreads the slice before the insertion point, then the new item, and then the rest of the original array.

In this example, the Insert button always inserts at the index 1:

```js
import { useState } from "react";

let nextId = 3;
const initialArtists = [
  { id: 0, name: "Marta Colvin Andrade" },
  { id: 1, name: "Lamidi Olonade Fakeye" },
  { id: 2, name: "Louise Nevelson" },
];

export default function Replace() {
  const [name, setName] = useState("");
  const [artists, setArtists] = useState(initialArtists);

  function handleClick() {
    const insertAt = 1; // Could be any index
    const nextArtists = [
      // Items before the insertion point:
      ...artists.slice(0, insertAt),
      // New item:
      { id: nextId++, name: name },
      // Items after the insertion point:
      ...artists.slice(insertAt),
    ];
    setArtists(nextArtists);
    setName("");
  }

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={handleClick}>Insert</button>
      <ul>
        {artists.map((artist) => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}
```

<hr>

### `useReducer`

There is a different way of handling state updates, and that's by using reducers. You can add a reducer to your component using the `useReducer` hook. Import the `useReducer` method from the library like this:

```js
import { useReducer } from "react";
```

The `useReducer` method accepts

- a reducer function as its first parameter and
- the initial state as the second.

It returns a state variable and a dispatch method to make state changes.

You can define the state in the following way:

```js
const [state, dispatch] = useReducer(reducerFunction, initialValue);
```

The reducer function itself accepts two parameters and returns one value. The first parameter is the current `state`, and the second is the `action`.

- The `state` is the data we are manipulating.
- The `action` helps to identify the type of the `action` and change the state accordingly. It is executed by a `dispatch` function.

```js
function reducer(state, action) {}
```

**Example**:

```js
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </>
  );
}
```

**Example 2**:

```js
import React, { useReducer } from 'react';

const Counter = () => {
  // set initial state
  const initialState = 0;

  // implement reducer function
  const reducer = (state, action) => {
    switch (action.type) {
      case 'add':
        return state + action.payload;
      case 'subtract':
        return state - action.payload;
      case 'reset':
        return initialState;
      default:
        throw new Error();
    }
  };

  // write useReducer
  const [state, dispatch] = useReducer(reducer, initialState);

  // display state in component and trigger dispatch with buttons
  return (
    <div>
      <h3>Counter</h3>
      <h1>{state}</h1>
      <button onClick={() > dispatch({ type: 'subtract', payload: 1 })}>
        Decrease
      </button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
      <button onClick={() => dispatch({ type: 'add', payload: 2 })}>
        Increase
      </button>
    </div>
  );
};

export default Counter;
```

<hr>

### `useEffect`

The `useEffect` Hook allows you to perform side effects in your components. The word effect refers to a functional programming term called a "side effect".

Most React components are pure functions, meaning they receive an input and produce a predictable output of JSX. Side effects are not predictable because they are actions which are performed with the "outside world."

Common side effects include:

- Making a request to an API for data from a backend server
- To interact with browser APIs (that is, to use document or window directly)
- Using unpredictable timing functions like `setTimeout` or `setInterval`

If we perform a side effect directly in our component body, it gets in the way of our React component's rendering.

Side effects should be separated from the rendering process. If we need to perform a side effect, it should strictly be done after our component renders.

This is what `useEffect` gives us.

The basic syntax of `useEffect` is as follows:

```js
// 1. import useEffect
import { useEffect } from "react";

function MyComponent() {
  // 2. call it above the returned JSX
  // 3. pass two arguments to it: a function and an optional dependencies array
  useEffect(() => {}, []);

  // return ...
}
```

**Example**:

```js
import { useEffect } from "react";

function User({ name }) {
  useEffect(() => {
    document.title = name;
  }, [name]);

  return <h1>{name}</h1>;
}
```

The function passed to `useEffect` is a callback function. This will be called after the component renders.

The second argument is an array, called the **dependencies** array. This array should include all of the values that our side effect relies upon. This array will check and see if a value has changed between renders. If so, it will execute our `useEffect` function again.

**Example**:

```js
import { useState, useEffect } from "react";

function Count() {
  const [count, setCount] = useState(0);
  const [calculation, setCalculation] = useState(0);

  useEffect(() => {
    setCalculation(() => count * 2);
  }, [count]); // <- add the count variable here

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount((c) => c + 1)}>+</button>
      <p>Calculation: {calculation}</p>
    </>
  );
}
```

If you do not provide the dependencies array at all and only provide a function to `useEffect`, it will run after every render.

With an empty dependecies array, `useEffect` runs only on the first render:

```js
useEffect(() => {
  // With an empty dependecies array, runs only on the first render
}, []);
```

If you are updating state within your `useEffect`, make sure to provide an empty dependencies array. If you do provide an empty array, which I recommend you do by default for any time that you use `useEffect`, this will cause the effect function to only run once after the component has rendered the first time.

```js
function MyComponent() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetchData().then((myData) => setData(myData));
    // Correct! Runs once after render with empty array
  }, []);

  return (
    <ul>
      {data.map((item) => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
}
```

<hr>

Some effects require cleanup to reduce memory leaks. Timeouts, subscriptions, event listeners, and other effects that are no longer needed should be disposed.

We do this by including a `return` function at the end of the `useEffect` Hook.

```js
function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    let interval = setInterval(() => setTime(1), 1000);

    return () => {
      // setInterval clears when component unmounts
      clearInterval(interval);
    };
  }, []);
}
```

Components are unmounted when the parent component is no longer rendered or the parent component performs an update that does not render this instance. `ReactDOM.unmountComponentAtNode` will also trigger an unmount.

<hr>

### `useRef`

`useRef` is a React Hook that lets you reference a value that’s not needed for rendering.

`useRef(<initialValue>)` is a built-in React hook that accepts one argument as the initial value and returns a reference (aka ref). A reference is an object having a special property `current`.

By using a ref, you ensure that:

- You can store information between re-renders (unlike regular variables, which reset on every render).
- Changing it does not trigger a re-render (unlike state variables, which trigger a re-render).
- The information is local to each copy of your component (unlike the variables outside, which are shared)

**Example 1**:

The component LogButtonClicks uses a reference to store the number of clicks on a button:

```js
import { useRef } from "react";

function LogButtonClicks() {
  const countRef = useRef(0);

  const handleClick = () => {
    countRef.current++;
    console.log(`Clicked ${countRef.current} times`);
  };

  console.log("I rendered!");

  return <button onClick={handleClick}>Click me</button>;
}
```

Updating the reference value `countRef.current++` doesn't trigger component re-rendering. This is demonstrated by the fact that 'I rendered!' is logged to the console just once, at initial rendering, and no re-rendering happens when the reference is updated.

**Example 2**:

```js
import { useRef, useState, useEffect } from "react";

function Stopwatch() {
  const timerIdRef = useRef(0);
  const [count, setCount] = useState(0);

  const startHandler = () => {
    if (timerIdRef.current) {
      return;
    }
    timerIdRef.current = setInterval(() => setCount((c) => c + 1), 1000);
  };

  const stopHandler = () => {
    clearInterval(timerIdRef.current);
    timerIdRef.current = 0;
  };

  useEffect(() => {
    return () => clearInterval(timerIdRef.current);
  }, []);

  return (
    <div>
      <div>Timer: {count}s</div>
      <div>
        <button onClick={startHandler}>Start</button>
        <button onClick={stopHandler}>Stop</button>
      </div>
    </div>
  );
}
```

`startHandler()` function, which is invoked when the Start button is clicked, starts the timer and saves the timer id in the reference `timerIdRef.current = setInterval(...)`.

To stop the stopwatch user clicks Stop button. The Stop button handler `stopHandler` accesses the timer id from the reference and stops the timer `clearInterval(timerIdRef.current)`.

Additionally, if the component unmounts while the stopwatch is active, the cleanup function of `useEffect()` is going to stop the timer too.

**Example 3**:

Another useful application of the `useRef()` hook is to access DOM elements directly. This is performed in 3 steps:

- Define the reference to access the element `const elementRef = useRef()`;
- Assign the reference to `ref` attribute of the element: `<div ref={elementRef}></div>`;
- After mounting, `elementRef.current` points to the DOM element.

```js
import { useRef, useEffect } from "react";

function AccessingElement() {
  const elementRef = useRef();

  useEffect(() => {
    const divElement = elementRef.current;
    console.log(divElement); // logs <div>I'm an element</div>
  }, []);

  return <div ref={elementRef}>I'm an element</div>;
}
```

**Example 4**:

You would need to access DOM elements, for example, to focus on the input field when the component mounts.

```js
import { useRef, useEffect } from "react";

function InputFocus() {
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

During initial rendering React still determines the output of the component, so there's no DOM structure created yet. That's why `inputRef.current` evaluates to undefined during initial rendering.

`useEffect(callback, [])` hook invokes the callback right after mounting when the input element has already been created in DOM.

callback function of the `useEffect(callback, [])` is the right place to access `inputRef.current` because it is guaranteed that the DOM is constructed.

<hr>

### `forwardRef`

To access a DOM element rendered in the component's body you can use a ref created by `useRef()` hook. But what if you need to access a DOM element of a child component? Then a simple ref is not enough and you have to combine refs with `React.forwardRef()`: a technique called refs forwarding.

`forwardRef()` is a higher-order component that wraps a React component. The wrapped component works the same way as the original component but also receives as the second parameter a ref: the forwarded reference from the parent component.

```js
import { useRef, useEffect, forwardRef } from "react";

export function Parent() {
  const elementRef = useRef();

  useEffect(() => {
    // Works!
    console.log(elementRef.current); // logs <div>Hello, World!</div>
  }, []);

  return <Child ref={elementRef} />; // assign the ref
}

const Child = forwardRef(function (props, ref) {
  return <div ref={ref}>Hello, World!</div>;
});
```

The parent component assigns `elementRef` to the child component `<Child ref={elementRef} />`. Then, thanks to being wrapped into `forwardRef()`, the child component gets the ref from the second parameter and uses it on its element `<div ref={ref}>`.

After mounting, `elementRef.current`, in the parent component, contains the DOM element from child component.

That's the purpose of `forwardRef()`: give the parent component access to DOM elements in the child component.

You can forward refs more than 1 level down in the component hierarchy. Just wrap each child, grandchild, and so on components in `forwardRef()`, and pass the ref down until you reach the target DOM element.

```js
import { forwardRef, useRef, useEffect } from "react";

export function Parent() {
  const elementRef = useRef();

  useEffect(() => {
    console.log(elementRef.current); // logs <div>Deep!</div>
  }, []);

  return <Child ref={elementRef} />;
}

const Child = forwardRef(function (props, ref) {
  return <GrandChild ref={ref} />;
});

const GrandChild = forwardRef(function (props, ref) {
  return <div ref={ref}>Deep!</div>;
});
```

<hr>

### `useImperativeHandle`

In essence the only thing this hook does is let you create a completely custom value for the ref you return from a custom component. This means you can do more than just assign a single element to your ref for example.

```js
function CustomInput(props, ref) {
  useImperativeHandle(ref, () => {
    return { alertHi: () => alert("Hi") };
  });

  return <input style={{ backgroundColor: "red" }} {...props} />;
}

export default React.forwardRef(CustomInput);
```

Here we have the most basic use case of `useImperativeHandle` which takes 2 parameters. The first parameter is just the ref you are overriding and the second parameter is a function which returns the new value for your ref. In our case the ref for our `CustomInput` is now an object that contains the single function `alertHi`.

```js
function App() {
  const [value, setValue] = useState("")
  const inputRef = useRef()

  return (
    <>
      <CustomInput
        ref={inputRef}
        value={value}
        onChange={e => setValue(e.target.value)}
      >
      <button onClick={() => inputRef.current.alertHi()}>Alert</button>
    </>
  )
}
```

`useImperativeHandle` takes an optional third parameter which is a dependency array. This works just like `useEffect` in that anytime any of the values in the dependency array change, the function passed to `useImperativeHandle` will be rerun and update the value of your ref. This is useful if you depend on certain values like the `props.value` in our example. If you do not pass this third parameter to `useImperativeHandle`, it will rerun the function you pass to `useImperativeHandle` and update your ref every time the component is rendered.

```js
function CustomInput(props, ref) {
  useImperativeHandle(ref, () => {
    return { alertValue: () => alert(props.value) };
  }, [props.value]);

  return <input ref={ref} style={{ backgroundColor: "red" }} {...props} />;
}

export default React.forwardRef(CustomInput);
```

<hr>

### `useMemo`

Memoization is an optimization technique that passes a complex function to be memoized. In memoization, the result is “remembered” when the same parameters are passed-in subsequently.

If we have a function compute 1 + 1, it will return 2. But if it uses memoization, the next time we run 1’s through the function, it won’t add them up; it will just remember the answer is 2 without executing the adding function.

`useMemo` takes in a function and an array of dependencies.

```js
const memoizedValue = React.useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

The dependencies act similar to arguments in a function. The dependency’s list are the elements `useMemo` watches: if there are no changes, the function result will stay the same.

If your dependencies array is not provided, there is no possibility of memoization, and it will compute a new value on every render. You could use the `useRef` hook in that instance. The advantage `useMemo` offers over `useRef` is a re-memoizing if the dependencies change.

**Example**:

```js
import { useState, useMemo } from "react";

export default function Component() {
  const [number, setNumber] = useState(1);
  const [inc, setInc] = useState(0);

  const factorial = useMemo(() => factorialOf(number), [number]);

  const handleChange = (event) => {
    setNumber(Number(event.target.value));
  };
  const handleClick = () => setInc((i) => i + 1);

  return (
    <div>
      Factorial of
      <input type="number" value={number} onChange={handleChange} />
      if {factorial}
      <button onClick={handleClick}>Re-render</button>
    </div>
  );
}

function factorialOf(n) {
  console.log("factorialOf(n) called!");
  return n <= 0 ? 1 : n * factorialOf(n - 1);
}
```

Every time you change the value of the number, 'factorialOf(n) called!' is logged to console. That's expected.

However, if you click Re-render button, 'factorialOf(n) called!' isn't logged to console because `useMemo(() => factorialOf(number), [number])` returns the memoized factorial calculation. Great!

<hr>

### `useCallback`

Before diving into `useCallback()` use, let's distinguish the problem `useCallback()` solves — the functions equality check.

Functions in JavaScript are regular objects. Let's write a function `factory()` that returns functions that sum numbers:

```js
function factory() {
  return (a, b) => a + b;
}

const sumFunc1 = factory();
const sumFunc2 = factory();

console.log(sumFunc1(1, 2)); // => 3
console.log(sumFunc2(1, 2)); // => 3

console.log(sumFunc1 === sumFunc2); // => false
console.log(sumFunc1 === sumFunc1); // => true
```

The functions `sumFunc1` and `sumFunc2` share the same code source, but they are different function objects. Comparing them (`sumFunc1 === sumFunc2`) evaluates to `false`. That's just how JavaScript objects work. An object (including a function object) equals only to itself.

Different function objects sharing the same code are often created inside React components.

That's when `useCallback(<callbackFunc>, <deps>)` is helpful: given the same dependency values, the hook returns the same function instance between renderings (aka memoization):

```js
import { useCallback } from "react";

function MyComponent() {
  // handleClick is the same function object
  const handleClick = useCallback(() => {
    console.log("Clicked!");
  }, []);

  // ...
}
```

<hr>

### Context API

React context allows us to share data (state) across our components more easily. You can think of React context as the equivalent of global variables for our React components.

React context helps us avoid the problem of props drilling. _Props drilling_ is a term to describe when you pass props down multiple levels to a nested component, through components that don't need it.

There are four steps to using React context:

- Create context using the `createContext` method.
- Take your created context and wrap the context provider around your component tree.
- Put any value you like on your context provider using the `value` prop.
- Read that `value` within any component by using the context consumer.

**Example**:

In our App, let's pass down our own name using Context and read it in a nested component: User.

```js
// App.js
import React from "react";
import User from "./components/User/User";

export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Name">
      <User />
    </UserContext.Provider>
  );
}
```

```js
// User.js
import { UserContext } from "../../App";

function User() {
  return (
    <UserContext.Consumer>{(value) => <h1>{value}</h1>}</UserContext.Consumer>
  );
}

export default User;
```

Instead of using `Consumer`, we can pass the entire context object to a hook called `React.useContext()`.

Here is the example above using the `useContext` hook:

```js
// App.js
import React from "react";
import User from "./components/User/User";

export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Name">
      <User />
    </UserContext.Provider>
  );
}
```

```js
// User.js
import { UserContext } from "../../App";

function User() {
  const value = React.useContext(UserContext);

  return <h1>{value}</h1>;
}

export default User;
```

**Example 2**:

We can add a default value to `createContext`. However, the default will work when a parent component doesn't have the `Provider`.

```js
// context.js
import { createContext } from "react";

export const Context = createContext("Default Value");
```

```js
// Providing the Context
import { Context } from "./context";

function Main() {
  const value = "My Context Value";
  return (
    <Context.Provider value={value}>
      <MyComponent />
    </Context.Provider>
  );
}
```

```js
// Consuming the context
import { useContext } from "react";
import { Context } from "./context";

function MyComponent() {
  const value = useContext(Context);

  return <span>{value}</span>;
}
```

The hook returns the value of the context: `value = useContext(Context)`. The hook also makes sure to re-render the component when the context value changes.

**Example 3**:

Context API works even if a component is nested within several components.

```js
// App.js
import { useContext, createContext } from "react";
import { Layout } from "./components";

const UserContext = createContext("Unknown");

function Application() {
  const userName = "John Smith";
  return (
    <UserContext.Provider value={userName}>
      <Layout>
        <h1>Main content</h1>
      </Layout>
    </UserContext.Provider>
  );
}
```

```js
// Layout.js
import Header from "../Header/Header";

export default function Layout({ children }) {
  return (
    <div>
      <Header />
      <main>{children}</main>
    </div>
  );
}
```

```js
// Header.js
import UserInfo from "../UserInfo/UserInfo";

export default function Header() {
  return (
    <header>
      <UserInfo />
    </header>
  );
}
```

```js
// UserInfo.js
import { UserContext } from "../../App";

export default function UserInfo() {
  const userName = useContext(UserContext);
  return <span>{userName}</span>;
}
```

#### How to use Context and set value of context in child components

```js
//Context.js
import React from "react";
export const Context = React.createContext();
```

```js
//App.js
import React, { useState } from "react";
import { Context } from "./Context.js";

import ComponentA from "./ComponentA";
import ComponentB from "./ComponentB";

export default function App() {
  const [context, setContext] = useState("default context value");
  return (
    <Context.Provider value={[context, setContext]}>
      <ComponentA />
      <ComponentB />
    </Context.Provider>
  );
}
```

```js
//ComponentA.js
import React, { useContext } from "react";
import { Context } from "./Context";

export default function ComponentA() {
  const [context, setContext] = useContext(Context);
  return (
    <div>
      ComponentA:
      <button onClick={() => setContext("New Value")}>
        Change Context Value
      </button>
    </div>
  );
}
```

```js
//ComponentB.js
import React, { useContext } from "react";
import { Context } from "./Context";

export default function ComponentB() {
  const [context, setContext] = useContext(Context);
  return <div>ComponentB: {context}</div>;
}
```

<hr>

## Suspense component and lazy

### Suspense

`Suspense` lets you display a fallback until its children have finished loading. The `Suspense` component receives two props, `children` and `fallback`. It then renders `fallback` until all the data required by `children` is available and `children` can be rendered.

By default, the whole tree inside `Suspense` is treated as a single unit. For example, even if only one of these components suspends waiting for some data, all of them together will be replaced by the loading indicator.

Only Suspense-enabled data sources will activate the `Suspense` component. They include:

- Data fetching with Suspense-enabled frameworks like Relay and Next.js
- Lazy-loading component code with `lazy`
- Reading the value of a Promise with `use` (this is still experimental)

`Suspense` does not detect when data is fetched inside an Effect or event handler.

<hr>

### lazy

`lazy` lets you defer loading component’s code until it is rendered for the first time. It is also known as on-demand loading because it only loads content visible on users’ screens.

Usually, you import components with the static `import` declaration. To defer loading a component’s code until it’s rendered for the first time, you can use dinamic importing with `lazy` like this:

```js
import { lazy } from "react";

const Component = lazy(() => import("./Component.js"));
```

Using this pattern requires that the lazy component you’re importing was exported as the `default` export.

Now that your component’s code loads on demand, you also need to specify what should be displayed while it is loading. You can do this by wrapping the `lazy` component or any of its parents into a `<Suspense>` boundary.

Do not declare lazy components inside other components. This will cause all state to be reset on re-renders. Instead, always declare them at the top level of your module.

<hr>

### Usage

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
