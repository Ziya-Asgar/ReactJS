# Rendering Lists

- [Rendering Lists](#rendering-lists)
  - [`map` method](#map-method)
  - [`filter` method](#filter-method)

---

---

## `map` method

You will rely on JavaScript features like `for` loop and the array `map()` method to render lists of components.

```js
const products = [
  { title: "Cabbage", id: 1 },
  { title: "Garlic", id: 2 },
  { title: "Apple", id: 3 },
];

const listItems = products.map((product) => (
  <li key={product.id}>{product.title}</li>
));

return <ul>{listItems}</ul>;
```

Notice how `<li>` has a `key` attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings. Usually, a `key` should be coming from your data, such as a database ID. React uses your keys to know what happened if you later insert, delete, or reorder the items.

---

---

## `filter` method

Here is an example of using `filter` in ReactJS:

```js
export const people = [
  {
    id: 0,
    name: "Creola Katherine Johnson",
    profession: "mathematician",
  },
  {
    id: 1,
    name: "Mario José Molina-Pasquel Henríquez",
    profession: "chemist",
  },
  {
    id: 2,
    name: "Mohammad Abdus Salam",
    profession: "physicist",
  },
];

export default function PeopleList() {
  const chemists = people.filter((person) => person.profession === "chemist");

  const listItems = chemists.map((person) => (
    <li key={person.id}>
      <p>
        <b>{person.name}:</b>
        {" " + person.profession + "."}
      </p>
    </li>
  ));

  return <ul>{listItems}</ul>;
}
```

---

---
