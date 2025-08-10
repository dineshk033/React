# Bootstrap Integration in React

This guide demonstrates how to install Bootstrap in a React project and how to import Bootstrap CSS and JavaScript. It also provides a simple example of using Bootstrap components in a React application.

## 1. Install Bootstrap

First, ensure you have a React project set up. If you don't have one, create it.

Install Bootstrap and its peer dependency, `@popperjs/core`, using npm or yarn:

```sh
npm install bootstrap @popperjs/core
# or
yarn add bootstrap @popperjs/core
```

## 2. Import Bootstrap CSS and JS

You can import Bootstrap CSS and JS directly into your `src/index.js` or `src/index.jsx` file.

```js
// src/index.js or src/index.jsx

import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.bundle.min.js'; // Includes Popper
```

Alternatively, you can import only the CSS in your `App.js` and load JS only if you use components that require JavaScript (like modals or dropdowns).

## 3. Using Bootstrap Components in React

You can now use Bootstrap classes in your React components. Here’s a simple example:

```jsx
// src/App.js

import React from 'react';

function App() {
  return (
    <div className="container mt-5">
      <h1 className="text-primary">Bootstrap in React</h1>
      <button className="btn btn-success" type="button">
        Click Me!
      </button>
      <div className="alert alert-warning mt-3" role="alert">
        This is a Bootstrap alert!
      </div>
    </div>
  );
}

export default App;
```

## 4. Run Your Project

Start your React application:

```sh
npm start
# or
yarn start
```

You should see your app styled with Bootstrap.

## 5. Additional Notes

- For advanced Bootstrap components (like modals, tooltips, etc.), ensure JavaScript is imported (as shown above).
- For more react-friendly usage, consider using the [react-bootstrap](https://react-bootstrap.github.io/) library, which provides Bootstrap components as React components.

---

**References:**
- [Bootstrap Documentation](https://getbootstrap.com/docs/5.0/getting-started/introduction/)
- [React Bootstrap](https://react-bootstrap.github.io/)
