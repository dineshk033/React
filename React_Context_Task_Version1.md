# React Context Task: User Authentication Context Implementation

## Task Overview

**Goal:**  
Create a User Authentication Context in a React app that manages the current user's login state and provides login/logout functionality to any component that needs it.

This is a moderate-level task designed to help you understand how to create, provide, and consume context for real-world application state management.

---

## What You Need to Implement

### 1. Create a UserContext

- Define a new context called `UserContext` (e.g., in `UserContext.js`).
- This context should provide:
  - The current user object (or `null` if not logged in).
  - A `login` function that sets the current user.
  - A `logout` function that clears the current user.

---

### 2. Create a Provider Component

- Create a `UserProvider` component (e.g., in `UserProvider.js`) that uses React state to manage the user.
- Wrap your application with `UserProvider` so any component can access user state.

---

### 3. Consume Context in Components

- Make a `LoginButton` component:
  - If no user is logged in, show a "Login" button.
  - When clicked, call `login` (simulate a login with hardcoded user data).
  - If logged in, display the username and a "Logout" button.
  - The "Logout" button should call `logout`.

- Make a `UserProfile` component:
  - Show the current user's name and email when logged in.
  - If not logged in, show a message: "User not logged in."

---

### 4. Wire Everything Together

- In your `App.js`, wrap everything in the `UserProvider`.
- Render the `LoginButton` and `UserProfile` components.

---

## Example File Structure

```
src/
  UserContext.js
  UserProvider.js
  LoginButton.js
  UserProfile.js
  App.js
```

---

## Step-by-Step Implementation Guide

### Step 1: Create Context

```jsx
// UserContext.js
import React from "react";
const UserContext = React.createContext();
export default UserContext;
```

---

### Step 2: Create Provider

```jsx
// UserProvider.js
import React, { useState } from "react";
import UserContext from "./UserContext";

export const UserProvider = ({ children }) => {
  const [user, setUser] = useState(null);

  const login = () => setUser({ name: "Alice", email: "alice@example.com" });
  const logout = () => setUser(null);

  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {children}
    </UserContext.Provider>
  );
};
```

---

### Step 3: Create Components

```jsx
// LoginButton.js
import React, { useContext } from "react";
import UserContext from "./UserContext";

const LoginButton = () => {
  const { user, login, logout } = useContext(UserContext);

  return user ? (
    <div>
      <span>Welcome, {user.name}!</span>
      <button onClick={logout}>Logout</button>
    </div>
  ) : (
    <button onClick={login}>Login</button>
  );
};

export default LoginButton;
```

```jsx
// UserProfile.js
import React, { useContext } from "react";
import UserContext from "./UserContext";

const UserProfile = () => {
  const { user } = useContext(UserContext);

  return user ? (
    <div>
      <h2>User Profile</h2>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  ) : (
    <p>User not logged in.</p>
  );
};

export default UserProfile;
```

---

### Step 4: Wire Up in App.js

```jsx
// App.js
import React from "react";
import { UserProvider } from "./UserProvider";
import LoginButton from "./LoginButton";
import UserProfile from "./UserProfile";

function App() {
  return (
    <UserProvider>
      <LoginButton />
      <UserProfile />
    </UserProvider>
  );
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

1. Implement the above files in your project.
2. Test the login/logout flow and user profile display.
3. Try extending functionality: add more fields, persist user state, or integrate with real authentication.

**Ask if you need help or suggestions for improvements!**