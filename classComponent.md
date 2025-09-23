# React Class Components: State, Lifecycle, and Error Boundaries

This guide explains **state management**, **lifecycle methods**, and **error boundaries** in React **Class Components**. Each concept is broken down step by step with simple examples to help you understand and apply them in your projects.

---

## 1. What is a Class Component?

A **class component** is a React component defined using an ES6 class. It allows you to use state and lifecycle methods.

**Example:**
```jsx
import React, { Component } from 'react';

class MyComponent extends Component {
  render() {
    return <div>Hello from Class Component!</div>;
  }
}
```

---

## 2. State in Class Components

**State** is an object that holds data that may change over the lifecycle of the component.

### How to use State

- **Initialization:** State is initialized in the constructor.
- **Update:** Use `this.setState()` to update state.

**Example:**
```jsx
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 }; // Initial state
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 }); // Update state
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```
---

## 3. Lifecycle Methods

Lifecycle methods are special methods that run at different stages of a component's life:

| Method               | When it Runs                        |
|----------------------|-------------------------------------|
| constructor          | When the component is created       |
| componentDidMount    | After the component is rendered     |
| componentDidUpdate   | After state/props change            |
| componentWillUnmount | Just before the component is removed|

### Example: Logging Lifecycle

```jsx
import React, { Component } from 'react';

class LifecycleDemo extends Component {
  constructor(props) {
    super(props);
    this.state = { value: 1 };
    console.log('constructor');
  }

  componentDidMount() {
    console.log('componentDidMount');
  }

  componentDidUpdate(prevProps, prevState) {
    console.log('componentDidUpdate');
  }

  componentWillUnmount() {
    console.log('componentWillUnmount');
  }

  render() {
    return (
      <div>
        <p>Value: {this.state.value}</p>
        <button onClick={() => this.setState({ value: this.state.value + 1 })}>
          Increase
        </button>
      </div>
    );
  }
}
```

---

## 4. Error Boundaries

An **Error Boundary** is a component that catches JavaScript errors anywhere in its child component tree and displays a fallback UI.

### How to Create an Error Boundary

- Use `componentDidCatch(error, info)` and `static getDerivedStateFromError(error)`.

**Example:**
```jsx
import React, { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true }; // Update state so next render shows fallback UI
  }

  componentDidCatch(error, errorInfo) {
    // Log the error to an error reporting service
    console.error("Error caught by boundary:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }
    return this.props.children;
  }
}
```

**Usage:**
```jsx
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

If `MyComponent` or its children throw an error, the `ErrorBoundary` will display the fallback UI.

---

## 5. Step-by-Step: Using All Together in a Project

1. **Create your main component (with state and lifecycle):**
    ```jsx
    class App extends React.Component {
      // state, lifecycle methods, render...
    }
    ```

2. **Wrap components with the Error Boundary:**
    ```jsx
    <ErrorBoundary>
      <App />
    </ErrorBoundary>
    ```

3. **Handle errors gracefully:**  
   If any error occurs in `App` or its child components, the `ErrorBoundary` will catch it and prevent the whole app from crashing.

---

## 6. Summary Table

| Concept        | Purpose                                 | Key Methods                          | Example          |
|----------------|-----------------------------------------|--------------------------------------|------------------|
| State          | Store data that changes                 | `this.state`, `this.setState()`      | Counter example  |
| Lifecycle      | Run code at key points                  | `componentDidMount`, etc.            | LifecycleDemo    |
| Error Boundary | Catch errors in child components        | `getDerivedStateFromError`, `componentDidCatch` | ErrorBoundary    |

---

## 7. Final Notes

- **Functional components** with hooks are preferred in modern React, but class components are still useful and widely found in existing projects.
- Always use Error Boundaries at a high level to catch unexpected errors.
- Use lifecycle methods wisely to manage side effects and cleanups.

---

**Happy Coding!**