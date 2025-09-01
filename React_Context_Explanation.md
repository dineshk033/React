# Understanding Context in React


## Why & When Should We Use Context in React?

### What is Context?
**Context** is a way to pass data through the component tree without having to pass props down manually at every level.

### When to Use Context?
- When you have global data (e.g., theme, user info) needed by many components.
- When "prop drilling" (passing props through many layers) becomes cumbersome.

#### Common Use Cases:
- Theme (light/dark mode)
- Authentication (current user)
- Language settings

---

## 3. Step-by-Step Example: Using React Context

Let’s create a simple theme toggler using Context.

### Step 1: Create Context
```jsx
// ThemeContext.js
import React from "react";
const ThemeContext = React.createContext();
export default ThemeContext;
```

### Step 2: Create a Provider Component
```jsx
// ThemeProvider.js
import React, { useState } from "react";
import ThemeContext from "./ThemeContext";

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => setTheme(theme === "light" ? "dark" : "light");

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

### Step 3: Consume Context in Child Components
```jsx
// ThemedButton.js
import React, { useContext } from "react";
import ThemeContext from "./ThemeContext";

const ThemedButton = () => {
  const { theme, toggleTheme } = useContext(ThemeContext);
  return (
    <button
      style={{
        background: theme === "light" ? "#eee" : "#333",
        color: theme === "light" ? "#333" : "#eee",
      }}
      onClick={toggleTheme}
    >
      Toggle Theme (Current: {theme})
    </button>
  );
};

export default ThemedButton;
```

### Step 4: Wrap Your App with the Provider
```jsx
// App.js
import React from "react";
import { ThemeProvider } from "./ThemeProvider";
import ThemedButton from "./ThemedButton";

function App() {
  return (
    <ThemeProvider>
      <ThemedButton />
    </ThemeProvider>
  );
}

export default App;
```

---

## 4. Summary

- **React Context** simplifies sharing state/data across components without prop drilling.
- Use Context when data is needed by many components at different nesting levels.

---

Feel free to ask more questions or experiment with the boilerplate above!