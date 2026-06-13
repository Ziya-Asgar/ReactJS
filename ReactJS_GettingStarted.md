# Getting Started with React JS

- [Getting Started with React JS](#getting-started-with-react-js)
  - [Adding relevant script tags](#adding-relevant-script-tags)
  - [Using `create-react-app`](#using-create-react-app)
    - [Upgrading `create-react-app`](#upgrading-create-react-app)
    - [Deployment with `create-react-app`](#deployment-with-create-react-app)
  - [Using `Vite`](#using-vite)

---

---

## Adding relevant script tags

One easy way to use React in our projects is to go to the [React link](https://legacy.reactjs.org/docs/cdn-links.html), and from there copy 2 links and paste into the head tags of our html file.

```html
<!-- These are those 2 links at the time of writing this -->
<script
  crossorigin
  src="https://unpkg.com/react@18/umd/react.development.js"
></script>
<script
  crossorigin
  src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"
></script>
```

The link for react-dom that we copied over enables us to access the global variable `ReactDOM` in our js file.

These 2 links are limiting, if we don't use babel. So, let's head to this [link](https://legacy.reactjs.org/docs/add-react-to-a-website.html#quickly-try-jsx) and copy the link for _babel_ and paste into the head tags in our html file.

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

After adding the _babel_ we can use JSX in any `<script>` tag by adding `type="text/babel"` attribute to it.

```html
<script src="index.js" type="text/babel"></script>
```

JSX stands for JavaScript XML.

---

---

## Using `create-react-app`

To create a react app, we need some tools. One of the tools that could be used is "[create-react-app](https://create-react-app.dev/)". To create a project called `my-app` using `create-react-app`, run this command:

```
npx create-react-app my-app
```

After running this, we get into the "my-app" folder and run `npm start`. Then open http://localhost:3000/ in a browser to see our app. This runs the app in the development mode.

You don’t need to install or configure tools like webpack or Babel. They are pre-configured and hidden so that you can focus on the code.

---

### Upgrading `create-react-app`

When new versions of Create React App are released, you can upgrade using a single command:

```
npm install react-scripts@latest
```

---

### Deployment with `create-react-app`

Before deploying your app you need to run this code:

```
npm run build or yarn build
```

This code builds the app for production to the build folder. It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.

Your app is ready to be deployed.

---

---

## Using `Vite`

We can also create a react app using [Vite](https://vitejs.dev/). If we go to the link and then click "Get Started", we can find different codes to start a project. We use the below code for React:

```
npm create vite@latest my-react-app -- --template react
```

Note: When we use "create-react-app", we can name our files either with "js" or "jsx" extension. In Vite, we must use "jsx".

After the app is created, we need to use the below code in the terminal. This installs the dependencies:

```
npm install
```

Then to run the project, use this command:

```
npm run dev
```

By default, Vite can run the React app on port 5173, but we can change it using the **vite.config.js** file. We just need to add `server: {port: <port_number>}`.

```js
// vite.config.js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000,
  },
});
```

---

---
