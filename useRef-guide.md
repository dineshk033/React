# Guide to `useRef` in React

## What is `useRef`?

`useRef` is a React Hook that lets you persist values across renders **without causing a re-render**. It is commonly used to access and interact with DOM elements directly, or to store mutable data which does not affect the UI.

## Syntax

```jsx
const refContainer = useRef(initialValue);
```

- `initialValue`: The initial value for the ref object.  
- The hook returns a ref object with a single property: `.current`.

## Example 1: Accessing DOM Elements

```jsx
import React, { useRef } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
}
```
**Explanation**:  
- `inputRef` is attached to the `<input>` element using the `ref` prop.
- When the button is clicked, `handleFocus` uses `inputRef.current` to call `.focus()` on the input DOM node.

## Example 2: Keeping Mutable Data

```jsx
import React, { useRef, useState } from 'react';

function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef();

  React.useEffect(() => {
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    return () => clearInterval(intervalRef.current);
  }, []);

  return <h1>{count}</h1>;
}
```
**Explanation**:  
- `intervalRef` stores the timer ID.
- It persists between renders without triggering them.

## Use Cases for `useRef`

- **Accessing DOM nodes** directly (e.g., focusing an input, measuring its size).
- **Storing mutable values** that do not cause a re-render when changed (e.g., timers, IDs, previous values).
- **Avoiding re-renders**: Unlike state, changing `.current` does NOT re-render the component.

## Difference Between `useRef` and `useState`

| Feature          | `useRef`                          | `useState`                       |
|------------------|-----------------------------------|----------------------------------|
| Triggers Render  | No (changing `.current`)           | Yes (when state changes)         |
| Value persists   | Yes (across renders)               | Yes                              |
| Use for UI       | No                                 | Yes                              |
| Use for DOM      | Yes                                | No                               |
| Example          | Focus input, timer ID              | Form input values, counters      |

**Why use `useRef` instead of `useState`?**
- If you need to store a value that changes over time but **does not affect the UI**, use `useRef`.  
- If you need a value that, when changed, should cause the component to re-render, use `useState`.

## Limitations of `useRef`

- Changes to `.current` are **not reactive** — the component does not re-render when it changes.
- Not suitable for storing data that drives the UI.
- Should not be used as a replacement for state.

## Summary

- Use `useRef` for **mutable values** that don’t need to trigger re-renders.
- Use `useState` for **reactive data** that affects rendering.
- `useRef` is often used for accessing DOM nodes and timers.

---

**References**  
- [React Docs: useRef](https://react.dev/reference/react/useRef)
- [React Docs: useState](https://react.dev/reference/react/useState)