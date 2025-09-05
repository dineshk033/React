## Table of Contents

1. [What is React Router?](#1-what-is-react-router)
2. [Installing React Router](#2-installing-react-router)
3. [Basic Setup](#3-basic-setup)
4. [Defining Routes](#4-defining-routes)
5. [Navigation in React Router](#5-navigation-in-react-router)
6. [Passing Parameters in Routes](#6-passing-parameters-in-routes)
7. [Programmatic Navigation](#7-programmatic-navigation)
8. [Handling Private Routes (Authentication)](#8-handling-private-routes-authentication)
    - [Step 1: Create an Auth Context](#step-1-create-an-auth-context)
    - [Step 2: PrivateRoute Component](#step-2-privateroute-component)
    - [Step 3: Wrap App with AuthProvider](#step-3-wrap-app-with-authprovider)
    - [Step 4: Sample Login Component](#step-4-sample-login-component)
    - [How Private Routes Work](#how-private-routes-work)
    - [Tips for Private Routes](#tips-for-private-routes)
9. [Mentoring Tips](#9-mentoring-tips)
10. [References](#10-references)

---

## 1. What is React Router?

**React Router** is a powerful library for handling routing and navigation in React applications. It allows you to create single-page applications (SPAs) with multiple views and smooth navigation between them, all without full page reloads.

---

## 2. Installing React Router

To install React Router, run the following command in your project's root directory:

```bash
npm i react-router-dom
```

- `npm` is the Node Package Manager.
- `i` is shorthand for `install`.
- `react-router-dom` is the package for web applications (for React Native, use `react-router-native`).

---

## 3. Basic Setup

### Importing React Router Components

At the top of your main application file (usually `App.js`):

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';
```

### Wrapping Your App with `BrowserRouter`

This enables routing in your app:

```jsx
function App() {
  return (
    <BrowserRouter>
      {/* Your routes will go here */}
    </BrowserRouter>
  );
}
```

---

## 4. Defining Routes

Inside `BrowserRouter`, use `Routes` to define all possible navigable paths:

```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Each route renders a specific component based on the URL.

---

## 5. Navigation in React Router

### Using `<Link>` for Navigation

Replace traditional anchor (`<a>`) tags with React Router's `<Link>`:

```jsx
<Link to="/about">About Us</Link>
<Link to="/contact">Contact</Link>
```

- `<Link>` updates the URL and navigates without page reload.

### Example Navigation Bar

```jsx
function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link> | 
      <Link to="/about">About</Link> | 
      <Link to="/contact">Contact</Link>
    </nav>
  );
}
```

Include `<Navbar />` in your `App` to navigate.

### Navigation Process (Step-by-Step)

1. **User clicks a `<Link>`**  
   → The URL in the browser updates.
2. **React Router intercepts the change**  
   → No page reload occurs.
3. **The correct `<Route>` is matched**  
   → The corresponding component is rendered.
4. **Application state and data remain intact**  
   → Only the view changes.

---

## 6. Passing Parameters in Routes

You can pass parameters in URLs, e.g., `/user/:id`:

```jsx
<Route path="/user/:id" element={<UserProfile />} />
```

In `UserProfile`, use the `useParams` hook to get the parameter:

```jsx
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { id } = useParams();
  // Fetch user data using id
}
```

---

## 7. Programmatic Navigation

Use the `useNavigate` hook for navigation in code (e.g., after a form submit):

```jsx
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  function goToHome() {
    navigate('/');
  }
}
```

---

## 8. Handling Private Routes (Authentication)

Restrict access to certain routes (e.g., dashboard) so only authenticated users can view them.

### Step 1: Create an Auth Context

```jsx
// src/contexts/AuthContext.js
import { createContext, useContext, useState } from "react";

const AuthContext = createContext();

export function AuthProvider({ children }) {
  // For demo: use a simple state. Replace with real auth logic.
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const login = () => setIsAuthenticated(true);
  const logout = () => setIsAuthenticated(false);

  return (
    <AuthContext.Provider value={{ isAuthenticated, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}
```

### Step 2: PrivateRoute Component

```jsx
// src/components/PrivateRoute.js
import { Navigate } from "react-router-dom";
import { useAuth } from "../contexts/AuthContext";

export default function PrivateRoute({ children }) {
  const { isAuthenticated } = useAuth();
  return isAuthenticated ? children : <Navigate to="/login" replace />;
}
```

- `Navigate` redirects users to another route.
- `replace` avoids back navigation to the protected page.

### Step 3: Wrap App with AuthProvider

```jsx
// src/App.js
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { AuthProvider } from "./contexts/AuthContext";
import PrivateRoute from "./components/PrivateRoute";
import Home from "./pages/Home";
import Dashboard from "./pages/Dashboard";
import Login from "./pages/Login";

function App() {
  return (
    <AuthProvider>
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/login" element={<Login />} />
          {/* Protect this route */}
          <Route
            path="/dashboard"
            element={
              <PrivateRoute>
                <Dashboard />
              </PrivateRoute>
            }
          />
        </Routes>
      </BrowserRouter>
    </AuthProvider>
  );
}

export default App;
```

### Step 4: Sample Login Component

```jsx
// src/pages/Login.js
import { useAuth } from "../contexts/AuthContext";
import { useNavigate } from "react-router-dom";

export default function Login() {
  const { login } = useAuth();
  const navigate = useNavigate();

  function handleLogin() {
    login();
    navigate("/dashboard");
  }

  return (
    <div>
      <h2>Login Page</h2>
      <button onClick={handleLogin}>Log In</button>
    </div>
  );
}
```

#### How Private Routes Work

1. User tries to visit `/dashboard`.
2. If **not authenticated**, `PrivateRoute` redirects to `/login`.
3. After login, user is redirected to `/dashboard` and gains access.

#### Tips for Private Routes

- Integrate with real authentication (JWT, OAuth, etc.).
- Store auth state in context, Redux, or React Query.
- Protect multiple routes by wrapping with `PrivateRoute`.
- Add loading states if authentication check is async.

---

## 9. Mentoring Tips

- Experiment by creating new pages and routes.
- Discuss real-world navigation flows (e.g., login → dashboard).
- Cover error handling with fallback routes (`<Route path="*" element={<NotFound />} />`).
- Practice programmatic navigation after API calls (e.g., redirect after login).
- Demonstrate private routes and authentication flows.

---

## 10. References

- [React Router Documentation](https://reactrouter.com/)
- [React Router Tutorial](https://reactrouter.com/en/main/start/tutorial)
- [React Router Authentication Example](https://reactrouter.com/en/main/examples/auth)
- [React Router Navigate Component](https://reactrouter.com/en/main/components/navigate)

---

Happy Routing and Mentoring! 🚦