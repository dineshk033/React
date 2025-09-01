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
// TODO: Create and export UserContext using React.createContext()
// Example: const UserContext = React.createContext();
// export default UserContext;
```

---

### Step 2: Create Provider

```jsx
// UserProvider.js
import React, { useState } from "react";
import UserContext from "./UserContext";

// TODO: Create UserProvider component
// - Use useState to manage the user object
// - Implement login and logout functions
// - Provide { user, login, logout } via UserContext.Provider
// - Render children inside Provider
```

---

### Step 3: Create Components

```jsx
// LoginButton.js
import React, { useContext } from "react";
import UserContext from "./UserContext";

// TODO: Consume UserContext
// - If user is null, show "Login" button and call login on click
// - If user exists, show welcome message and "Logout" button, call logout on click
```

```jsx
// UserProfile.js
import React, { useContext } from "react";
import UserContext from "./UserContext";

// TODO: Consume UserContext
// - If user exists, display their name and email
// - If user is null, show "User not logged in."
```

---

### Step 4: Wire Up in App.js

```jsx
// App.js
import React from "react";
import { UserProvider } from "./UserProvider";
import LoginButton from "./LoginButton";
import UserProfile from "./UserProfile";

// TODO: Wrap the application in UserProvider
// - Render LoginButton and UserProfile inside UserProvider
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
2. Test the login/logout flow and user profile display.
3. Try extending functionality: add more fields, persist user state, or integrate with real authentication.

**Ask if you need help or suggestions for improvements!**