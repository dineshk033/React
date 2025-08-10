# React Conditional & List Rendering — Beginner-Friendly Guide

---

## 📌 1. Conditional Rendering

React can show different UI based on conditions using JavaScript logic like `if`, `&&`, or the ternary (`? :`) operator.

### Example: Using `&&` for Conditional Rendering

```jsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && "✅"}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item isPacked={true} name="Space suit" />
        <Item isPacked={false} name="Photo of Tam" />
      </ul>
    </section>
  );
}
```

### How it works:

1. **`PackingList`** renders multiple `Item` components.
2. Each `Item` gets a `name` and an `isPacked` prop.
3. Inside `Item`, the checkmark (`✅`) is shown only if `isPacked` is `true`.
4. React automatically adjusts the UI when props change.

---

## 📌 2. List Rendering

React can render arrays of data using `.map()`. Always provide a **unique `key`** prop to help React track items.

### Example: Rendering a List

```jsx
import { people } from "./data.js";

export default function List() {
  const listItems = people.map((person) => (
    <li key={person.id}>
      <img src={person.image} alt={person.name} />
      <p>
        <b>{person.name}:</b> {person.profession}
      </p>
    </li>
  ));
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

### How it works:

1. `.map()` loops over the `people` array.
2. For each `person`, we return a `<li>` element.
3. A **unique `key`** (often an ID) is provided for efficient re-rendering.
4. The array of `<li>` items is rendered inside `<ul>`.

---

## 📌 3. Visualizing the Flow

### Conditional Rendering Flow

```
Parent Component → passes props → Child Component → decides UI based on condition
```

### List Rendering Flow

```
Array of Data → map() → JSX elements with keys → Rendered List
```

---

## 📌 4. Quick Reference Table

| Rendering Technique     | Syntax Example                          | Use Case                      |
| ----------------------- | --------------------------------------- | ----------------------------- |
| Conditional with `&&`   | `{isPacked && 'Packed'}`                | Show content only when true   |
| Conditional with `?:`   | `{isLoggedIn ? 'Hi' : 'Please log in'}` | Choose between two outputs    |
| List rendering with map | `people.map(person => <li key=.../>)`   | Render multiple similar items |

---

## 📌 5. Best Practices

- Keep conditions **simple** — avoid deeply nested logic.
- Use descriptive variable names for conditions (e.g., `isLoggedIn`, `hasError`).
- Always use a **stable unique key** when rendering lists.
- Avoid using array indexes as keys unless the list is static and never reorders.
- Keep render functions pure — don’t change state inside `.map()` or conditions.

---

💡 **Tip:** Combine **props** and **state** with conditional & list rendering for dynamic, interactive UIs.
