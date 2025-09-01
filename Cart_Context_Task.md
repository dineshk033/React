# React Context Task: Shopping Cart Context Implementation

## Task Overview

**Goal:**  
Create a Shopping Cart Context in a React app to manage cart items globally, allowing components to add, remove, and view items in the cart.

This moderate-level task will help you understand context creation, provider setup, and consumption for managing application state that spans multiple components.

---

## What You Need to Implement

### 1. Create a CartContext

- Define a new context called `CartContext` (e.g., in `CartContext.js`).
- This context should provide:
  - The current list of cart items.
  - An `addItem` function to add an item to the cart.
  - A `removeItem` function to remove an item from the cart by ID.

---

### 2. Create a Provider Component

- Create a `CartProvider` component (e.g., in `CartProvider.js`) that uses React state to manage the cart.
- Wrap your application with `CartProvider` so any component can access and interact with the cart.

---

### 3. Consume Context in Components

- Make an `AddToCartButton` component:
  - Receives item details as props.
  - On click, calls `addItem` from context to add the item.

- Make a `CartList` component:
  - Displays the list of items currently in the cart.
  - Provides a button for each item to remove it from the cart (calls `removeItem`).

---

### 4. Wire Everything Together

- In your `App.js`, wrap everything in the `CartProvider`.
- Render the `AddToCartButton` and `CartList` components.

---

## Example File Structure

```
src/
  CartContext.js
  CartProvider.js
  AddToCartButton.js
  CartList.js
  App.js
```

---

## Step-by-Step Implementation Guide

### Step 1: Create Context

```jsx
// CartContext.js
import React from "react";
// TODO: Create and export CartContext using React.createContext()
// Example: const CartContext = React.createContext();
// export default CartContext;
```

---

### Step 2: Create Provider

```jsx
// CartProvider.js
import React, { useState } from "react";
import CartContext from "./CartContext";

// TODO: Create CartProvider component
// - Use useState to manage array of cart items
// - Implement addItem and removeItem functions
// - Provide { cartItems, addItem, removeItem } via CartContext.Provider
// - Render children inside Provider
```

---

### Step 3: Create Components

```jsx
// AddToCartButton.js
import React, { useContext } from "react";
import CartContext from "./CartContext";

// TODO: Receive item details as props
// - On button click, call addItem from context with item data
```

```jsx
// CartList.js
import React, { useContext } from "react";
import CartContext from "./CartContext";

// TODO: Consume CartContext
// - Display list of cart items
// - For each item, render a remove button that calls removeItem from context
```

---

### Step 4: Wire Up in App.js

```jsx
// App.js
import React from "react";
import { CartProvider } from "./CartProvider";
import AddToCartButton from "./AddToCartButton";
import CartList from "./CartList";

// TODO: Wrap the application in CartProvider
// - Render AddToCartButton and CartList inside CartProvider
function App() {
  // Implement the context provider wrapping and component rendering here
}
export default App;
```

---

## What You Will Learn

- How to create context and provider for global state management
- How to share and update state across components using React Context
- How to avoid prop drilling by using context

---

## Next Steps

1. Implement the above files in your project by replacing the TODO placeholders.
2. Test adding and removing items from the cart.
3. Try extending functionality: add quantities, total price calculation, or persist cart in local storage.

**Ask if you need help or suggestions for improvements!**