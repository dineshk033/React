# Deep Dive: Using CSS Modules for Classes & Styles in Vite + React

As a React architect, CSS Modules offer local scoping and avoid style collisions, making them excellent for scalable Vite + React projects. Let’s focus on CSS Modules, show how to use media queries, combine selectors, and cover limitations and best practices.

---

## 1. **What is a CSS Module?**

- A CSS file named with `.module.css` (e.g., `Button.module.css`).
- When imported, it returns a JS object mapping your class names to unique, locally-scoped names.
- **Prevents global style collisions.**

---

## 2. **Basic Usage Example**

```css
/* src/components/Card.module.css */
.card {
  box-shadow: 0 2px 8px #0002;
  padding: 1rem;
  border-radius: 8px;
}
```

```jsx
// src/components/Card.jsx
import styles from './Card.module.css';

export default function Card({ children }) {
  return <div className={styles.card}>{children}</div>;
}
```

---

## 3. **Media Queries in CSS Modules**

You can write media queries *inside* your `.module.css` files, just like regular CSS.

```css
/* src/components/Card.module.css */
.card {
  padding: 1rem;
}

@media (max-width: 600px) {
  .card {
    padding: 0.5rem;
  }
}
```
**How it works:**  
- The media query targets the locally-scoped `.card` class.
- When imported in React, the correct styles apply based on viewport.

---

## 4. **Combining CSS Selectors in Modules**

You can use nested selectors, combinators, pseudo-classes, etc.

```css
/* src/components/Button.module.css */
.button {
  background: #1976d2;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
}
.button:hover,
.button:focus {
  background: #1565c0;
}

/* Combining selectors */
.button.primary {
  background: #388e3c;
}

.button.secondary {
  background: #fbc02d;
}
```

**How to use multiple classes in JSX:**
```jsx
import styles from './Button.module.css';

export default function Button({ variant, children }) {
  // Combine base and variant classes
  const classNames = [styles.button, variant && styles[variant]].join(' ');
  return <button className={classNames}>{children}</button>;
}

// Usage:
// <Button variant="primary">Primary</Button>
```

---

## 5. **Example: Responsive & Combined Selectors**

```css
/* src/components/ProfileCard.module.css */
.card {
  display: flex;
  flex-direction: row;
  gap: 1rem;
  padding: 2rem;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px #0001;
}

.card.compact {
  flex-direction: column;
  padding: 1rem;
}

.avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
}

@media (max-width: 600px) {
  .card {
    flex-direction: column;
    padding: 1rem;
  }
}
```
```jsx
import styles from './ProfileCard.module.css';

export default function ProfileCard({ compact }) {
  const cardClass = compact ? `${styles.card} ${styles.compact}` : styles.card;
  return (
    <div className={cardClass}>
      <img src="avatar.png" className={styles.avatar} alt="Avatar" />
      {/* ...other content... */}
    </div>
  );
}
```

---

## 6. **Limitations of CSS Modules**

- **No dynamic class names:** You cannot create new classes on the fly; you use only those defined in the file.
- **No global selectors:** You can’t style elements globally (e.g., `body {}` won’t work from a module).
- **No parent selectors:** Advanced selectors like `:has()` or nesting parent styles require plain CSS or a preprocessor.

---

## 7. **Best Practices**

- **Use modules for all local component styles.**
- For global resets or typography, import a global CSS file in your entry (e.g., `main.jsx`).
- **Combine classes with template strings or array joins in React.**
- Name classes clearly (`.primary`, `.secondary`, `.card`).
- Use media queries and combinators as you would in normal CSS.

---

## **Summary Table**

| CSS Module Feature             | Supported? | Example Syntax                                 |
|-------------------------------|------------|------------------------------------------------|
| Media Queries                 | Yes        | `@media (max-width: 600px) { .card { ... }}`  |
| Combined Selectors            | Yes        | `.button.primary { ... }`                      |
| Pseudo-classes                | Yes        | `.button:hover { ... }`                        |
| Global Styles/Selectors       | No         | Use separate global CSS file                   |
| Dynamic Class Names           | No         | Only classes defined in file                   |

---

## **Further Reading**

- [CSS Modules Documentation](https://github.com/css-modules/css-modules)
- [Vite + React Guide](https://vitejs.dev/guide/)
- [React Docs: Styling and CSS](https://react.dev/learn/styles-and-css)

---

**Mentorship Tip:**  
Master CSS Modules for robust component-level styling. Use global CSS only where necessary. For media queries and selector combinations, CSS Modules work exactly like regular CSS—just remember class names are locally scoped!