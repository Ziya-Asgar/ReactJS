# Handling Events

We can add a click event to a button in a component like this:

```js
const EventExamples = () => {
  const handleButtonClick = () => {
    alert("handle button click");
  };

  return (
    <section>
      <button onClick={handleButtonClick}>click me</button>
    </section>
  );
};
```

We can also pass event handling function to the event immediately:

```js
const EventExamples = () => {
  return (
    <section>
      <button onClick={() => console.log("hello there")}>click me</button>
    </section>
  );
};
```

---

All events propagate in React except `onScroll`, which only works on the JSX tag you attach it to.

If you want to prevent an event from reaching parent components, you need to call `e.stopPropagation()` like this Button component does:

```js
function Button({ onClick, children }) {
  return (
    <button
      onClick={(e) => {
        e.stopPropagation();
        onClick();
      }}
    >
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div
      className="Toolbar"
      onClick={() => {
        alert("You clicked on the toolbar!");
      }}
    >
      <Button onClick={() => alert("Playing!")}>Play Movie</Button>
      <Button onClick={() => alert("Uploading!")}>Upload Image</Button>
    </div>
  );
}
```

---

---
