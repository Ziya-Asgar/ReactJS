# Exporting and Importing

How you export your component dictates how you must import it.

- **Default**

```js
export default function Button() {}

import Button from "./Button.js";
```

- **Named**

```js
export function Button() {}

import { Button } from "./Button.js";
```

When you write a default `import`, you can put any name you want after `import`. For example, you could write `import Banana from './Button.js'` instead and it would still provide you with the same `default export`. In contrast, with named imports, the name has to match on both sides. That’s why they are called named imports!

---

---
