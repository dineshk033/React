# 📌 Props vs State in React (Function Components) — Complete Guide

## 1. Definition
- **Props (short for "properties")**
  - Data passed **from a parent component to a child component**.
  - Read-only → The child component **cannot** change them.
  - Think of them like arguments you pass to a function.

- **State**
  - Data **owned and managed by the component itself**.
  - Can change over time → when it changes, React **re-renders** the component.
  - Think of it like variables inside a function that can update and trigger changes in the UI.

---

## 2. Example: Using Props
```jsx
import React from "react";

function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default function App() {
  return <Greeting name="Dinesh" />;
}
```

**How it works (step-by-step):**
1. **`App`** calls the `Greeting` component and passes `"Dinesh"` as the `name` prop.
2. Inside `Greeting`, `name` is accessed directly (via **destructuring** `{ name }`).
3. The `h1` tag displays `"Hello, Dinesh!"`.
4. Props are **read-only** — `Greeting` cannot modify `name`.

---

## 3. Example: Using State
```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // Initial value = 0

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}

export default Counter;
```

**How it works (step-by-step):**
1. `useState(0)` creates a **state variable** `count` with an initial value of `0`.
2. `setCount` is a function to update `count`.
3. When the button is clicked:
   - `setCount(count + 1)` updates the value.
   - React automatically **re-renders** the component.
4. The new `count` value is shown in the `<p>` tag.

---

## 4. Key Differences
| Feature       | Props                          | State                          |
|---------------|--------------------------------|--------------------------------|
| **Who owns it?** | Parent component               | The component itself           |
| **Can modify?**  | ❌ No (read-only)               | ✅ Yes (with `setState` / `useState`) |
| **Purpose**      | Pass data down the tree        | Manage local, changeable data  |
| **Triggers Re-render?** | ✅ Yes (if parent changes) | ✅ Yes (when updated via setter) |

---

## 5. ✅ Checklist for Using Props & State
- [ ] Use **props** for **static or parent-controlled data**.
- [ ] Use **state** for **data that changes inside the component**.
- [ ] Keep state **minimal** → don’t store derived/computable values in state.
- [ ] Never mutate state directly → always use the setter function.
- [ ] Pass only **necessary props** to child components (avoid prop bloat).
- [ ] Use **prop types** (via `PropTypes` or TypeScript) for type safety.
- [ ] Lift state up when multiple components need to share it.

---

## 6. 📜 Rules for Props & State
- **Props**
  - Are **immutable** in the child component.
  - Can be any JavaScript value (string, number, array, object, function).
  - Passed **top-down** in the component tree.

- **State**
  - Can only be updated using the **setter function** from `useState`.
  - Must be kept **as small as possible** to reduce re-renders.
  - Can hold any serializable value (avoid storing large objects when possible).

---

## 7. ⚠️ Limitations & Pitfalls
- **Props**
  - If props are deeply nested, passing them down can lead to **prop drilling**.
  - Solution: Use **Context API** or state management libraries (Redux, Zustand).

- **State**
  - Updating state is **asynchronous** — changes won’t be visible immediately after calling the setter.
  - Too much state in one component can make it **hard to maintain** → split into smaller components.
  - Overusing local state can lead to **inconsistent UI** → centralize when needed.

---

## 8. 🏛 Architectural Patterns
### **Smart (Container) vs Dumb (Presentational) Components**
- **Container components** (Smart)
  - Manage **state** and handle logic.
  - Pass data and callbacks to child components.
- **Presentational components** (Dumb)
  - Receive **props** and display UI.
  - No own state (or minimal state for UI-specific toggles).

**Example:**
```jsx
// Container
function CounterContainer() {
  const [count, setCount] = useState(0);
  return <CounterDisplay count={count} onIncrement={() => setCount(count + 1)} />;
}

// Presentational
function CounterDisplay({ count, onIncrement }) {
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={onIncrement}>+</button>
    </div>
  );
}
```

---

## 9. 💡 Best Practices
- Keep **state as close as possible** to where it’s used.
- Use **props for configuration** and **state for behavior**.
- Avoid deeply nested state — flatten data when possible.
- Always consider **reusability** when designing components.
- Follow **Single Responsibility Principle** → each component should do one thing well.

---

## 10. 📌 Summary
| Use Case                     | Choose Props | Choose State |
|------------------------------|--------------|--------------|
| Pass data from parent → child | ✅           | ❌           |
| Component manages its own data| ❌           | ✅           |
| Data never changes            | ✅           | ❌           |
| Data changes dynamically      | ❌           | ✅           |
