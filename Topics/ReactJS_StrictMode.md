# `<React.StrictMode>`

`<StrictMode>` or `<React.StrictMode>` lets you find common bugs in your components early during development. Use `StrictMode` to enable additional development behaviors and warnings for the component tree inside:

Strict Mode enables the following development-only behaviors:

- Your components will re-render an extra time to find bugs caused by impure rendering.
- Your components will re-run Effects an extra time to find bugs caused by missing Effect cleanup.
- Your components will be checked for usage of deprecated APIs.

**Example**:

```html
<!-- index.html -->
<body>
  <div id="root"></div>
</body>
```

```js
/* index.js */
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

---

You can also enable Strict Mode for any part of your application:

```js
import { StrictMode } from "react";

function App() {
  return (
    <>
      <Header />

      <StrictMode>
        <main>
          <Sidebar />
          <Content />
        </main>
      </StrictMode>

      <Footer />
    </>
  );
}
```

---

---
