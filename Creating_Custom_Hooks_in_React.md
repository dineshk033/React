# How to Create Custom Hooks in React

Custom hooks in React are a powerful way to reuse stateful logic between components. This guide provides a detailed, step-by-step explanation on how to create custom hooks, the limitations involved, syntax examples, and practical use cases.

---

## 1. What is a Custom Hook?

A **custom hook** is a JavaScript function whose name starts with `use` and that can call other hooks (such as `useState`, `useEffect`, etc.). It lets you extract and share logic between components without sharing state.

---

## 2. Why Use Custom Hooks?

- **Reuse logic:** Share code between components.
- **Organize code:** Keep components clean.
- **Encapsulation:** Hide complex logic behind simple APIs.

---

## 3. Steps to Create a Custom Hook

### Step 1: Identify Reusable Logic

Find logic in your components that repeats, such as fetching data, handling forms, toggling modals, etc.

### Step 2: Define a Function Starting with `use`

Custom hooks must start with `use` (e.g., `useFetch`, `useToggle`).

### Step 3: Use Built-in Hooks Inside Your Custom Hook

Use React’s built-in hooks (`useState`, `useEffect`, etc.) inside your custom hook to manage state, side effects, etc.

### Step 4: Return Values/Functions

Return any state, functions, or values that your component will need.

---

## 4. Syntax: Example of a Custom Hook

### Example: `useToggle` Hook

```javascript
import { useState } from 'react';

// Custom Hook: useToggle
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = () => setValue(v => !v);

  return [value, toggle];
}

export default useToggle;
```

### How to Use in a Component

```javascript
import useToggle from './useToggle';

function ToggleComponent() {
  const [isOn, toggleIsOn] = useToggle();

  return (
    <button onClick={toggleIsOn}>
      {isOn ? 'ON' : 'OFF'}
    </button>
  );
}
```

---

## 5. Step-by-Step Example: Data Fetching Hook

### Custom Hook: `useFetch`

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}
```

### Usage

```javascript
function Users() {
  const { data, loading, error } = useFetch('https://api.example.com/users');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;

  return (
    <ul>
      {data.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

---

## 6. Limitations of Custom Hooks

1. **Hooks must be called at the top level**  
   - Can't be called inside loops, conditions, or nested functions.

2. **Hooks can only be called in React function components or other custom hooks**  
   - Not in regular JavaScript functions or classes.

3. **No direct access to component instance**  
   - Custom hooks can't access `this` or class instance variables.

4. **No side effects outside React’s lifecycle**  
   - Custom hooks rely on React's lifecycle; they can't affect DOM outside of it.

5. **Sharing state, not instances**  
   - Custom hooks share logic, not actual state.

6. **Testing complexity**  
   - Custom hooks may require dedicated testing strategies.

---

## 7. Use Cases for Custom Hooks

- **Form handling:** `useForm`, `useInput`
- **Data fetching:** `useFetch`, `useApi`
- **Animation:** `useAnimationFrame`
- **Auth:** `useAuth`
- **Dark mode toggle:** `useDarkMode`
- **Window events:** `useWindowSize`, `useScrollPosition`
- **Debouncing:** `useDebounce`
- **Local storage:** `useLocalStorage`

---

## 8. Tips and Best Practices

- Always start your custom hook’s name with `use` for linting and convention.
- Keep hooks focused on one responsibility.
- Document the hook’s API and expected usage.
- Handle cleanup logic (e.g., inside `useEffect`) to prevent memory leaks.

---

## 9. Summary

Custom hooks are a powerful abstraction for reusable, stateful logic in React. They help keep your components clean and DRY. Follow the rules of hooks, document your API, and consider the limitations when designing custom hooks.
