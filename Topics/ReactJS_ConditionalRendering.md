# Conditional Rendering

- [Conditional Rendering](#conditional-rendering)
  - [`if`](#if)
  - [logical and - `&&`](#logical-and---)
  - [logical or - `||`](#logical-or---)
  - [ternary operator - `? :`](#ternary-operator----)

---

---

In React, you can conditionally render JSX using JavaScript syntax like `if` statements, `&&,` `||`, and `? :` operators.

## `if`

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li>{name} ✔</li>;
  }
  return <li>{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item isPacked={true} name="Space suit" />
        <Item isPacked={true} name="Helmet" />
        <Item isPacked={false} name="Photo of Tam" />
      </ul>
    </section>
  );
}
```

---

---

## logical and - `&&`

Remember that the `&&` operator (logical AND) returns the first operand if it is "falsy", or the second operand if the first operand is "truthy".

**Example**:

```js
import { useState } from "react";

const RandomComponent = () => {
  return <h1>hello there</h1>;
};

const Cleanup = () => {
  const [toggle, setToggle] = useState(false);

  return (
    <div>
      <button onClick={() => setToggle(!toggle)}>toggle component</button>
      {toggle && <RandomComponent />}
    </div>
  );
};

export default Cleanup;
```

---

---

## logical or - `||`

The `||` operator (logical OR) returns the first operand if it is "truthy", or the second operand if the first operand is "falsy".

**Example**:

```js
function DisplayName({ name }) {
  return <h3>{name || "Anonymous"}</h3>;
}

export default function Names() {
  return (
    <>
      <DisplayName name="Ziya" />
      <DisplayName />
    </>
  );
}
```

---

---

## ternary operator - `? :`

In JavaScript, the ternary operator is a way to concisely express a simple conditional statement. It is often called the "conditional operator" or the "ternary conditional operator".

Here is the basic syntax for using the ternary operator:

```js
condition ? expression1 : expression2;
```

**Example**:

```js
import { useState } from "react";

const User = () => {
  const [user, setUser] = useState(null);

  const handleLogin = () => {
    // normally connect to db or api
    setUser({ name: "Username" });
  };
  const handleLogout = () => {
    setUser(null);
  };

  return (
    <div>
      {user ? (
        <div>
          <h4>hello there, {user.name}</h4>
          <button onClick={handleLogout}>logout</button>
        </div>
      ) : (
        <div>
          <h4>Please Login</h4>
          <button onClick={handleLogin}>login</button>
        </div>
      )}
    </div>
  );
};

export default UserChallenge;
```

---

---
