<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# create a md file to explain step by step to explain class component how to create and how to use lifecycle hooks and states and error boundary with example application to easily explain to trainee with proper syntax and rules

Below is a comprehensive Markdown file for trainees, explaining step-by-step how to create a React **class component**, use **lifecycle methods**, manage **state**, and implement an **error boundary**, including easy-to-understand examples and syntax rules.[^1][^2][^3][^4][^5][^6][^7][^8]

***

# React Class Components: Complete Step-by-Step Guide

## What is a Class Component?

A **class component** in React is defined using an ES6 class that extends `React.Component`. It provides powerful features like state management, lifecycle hooks, and error boundary capabilities.[^2][^9][^6][^1]

***

## Creating a Class Component

**Syntax:**

```javascript
import React from 'react';

class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

export default Welcome;
```

- The class must extend `React.Component`.
- The class name must start with a capital letter.
- The `render()` method is required and returns JSX.[^6][^1][^2]

***

## Managing State in Class Components

**State Initialization:**

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 }; // initial state
  }
}
```

- `this.state` holds component data.
- Update state using `this.setState()`.[^8][^6]

**State Update Example:**

```javascript
increment = () => {
  this.setState({ count: this.state.count + 1 });
}
```

- Event handlers can update state inside class methods.[^4][^6]

***

## Lifecycle Methods Explained

**Common Lifecycle Methods:**


| Method | Purpose |
| :-- | :-- |
| `componentDidMount()` | Called after component mounts (setup/init) |
| `componentDidUpdate()` | Called after state or props update |
| `componentWillUnmount()` | Called before component is removed (cleanup) |

**Example:**

```javascript
class Test extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: "Hello!" };
  }

  componentDidMount() {
    console.log("Mounted!");
  }

  componentDidUpdate() {
    console.log("Updated!");
  }

  componentWillUnmount() {
    console.log("Will unmount!");
  }

  render() {
    return <div>{this.state.message}</div>;
  }
}
```

- Use lifecycle hooks for tasks like fetching data, updating the DOM, or cleaning up resources (such as timers).[^7][^4][^8]

***

## Error Boundary: Catching Errors in Components

**Purpose:** Error Boundary components catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI, preventing the whole app from crashing.[^3][^5][^10][^11]

**How to Implement:**

```javascript
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true }; // Update state to show fallback UI
  }

  componentDidCatch(error, info) {
    console.error("Error caught in ErrorBoundary:", error, info); // Log error
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>; // Fallback UI
    }
    return this.props.children; // Normal UI
  }
}
```

**Usage Example:**

```javascript
// Wrap risky components with ErrorBoundary
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

- Only class components can be error boundaries.
- Use `getDerivedStateFromError` and `componentDidCatch` methods.[^5][^11][^12][^3]

***

## Example: Counter Application with Lifecycle \& Error Boundary

```javascript
import React from 'react';

class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  componentDidMount() {
    console.log("Counter mounted!");
  }
  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      console.log("Counter updated!");
    }
  }
  componentWillUnmount() {
    console.log("Counter will unmount!");
  }
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };
  render() {
    // Simulate error if count > 5
    if (this.state.count > 5) {
      throw new Error("Counter limit exceeded!");
    }
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

// Error Boundary usage
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  componentDidCatch(error, info) {
    console.error(error, info);
  }
  render() {
    if (this.state.hasError) {
      return <h1>Counter crashed: Too many increments!</h1>;
    }
    return this.props.children;
  }
}

// Main App
function App() {
  return (
    <ErrorBoundary>
      <Counter />
    </ErrorBoundary>
  );
}
```

- The counter demonstrates state, lifecycle hooks, and error boundaries in action.[^3][^4][^5][^6][^8]

***

## Best Practices \& Rules

- Use capital letters for class component names.
- Always extend React.Component.
- Keep one `render()` method per class.
- Use state for dynamic data and update with `setState`.
- Place resource initialization in `componentDidMount` and cleanup in `componentWillUnmount`.
- Use error boundaries to handle child errors gracefully.

***

**References:**
[Class Components - W3Schools][^1]
[Lifecycle Methods - GeeksforGeeks][^4]
[Error Boundaries - DEV Community][^3]
[Structure and Examples - GeeksforGeeks][^6]
[Lifecycles - Scaler Topics][^8]
<span style="display:none">[^13][^14][^15][^16][^17][^18][^19][^20]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.w3schools.com/react/react_class.asp

[^2]: https://www.scaler.com/topics/react/class-component-in-react/

[^3]: https://dev.to/mohiyaddeen7/mastering-error-boundaries-in-react-why-how-1fhf

[^4]: https://www.geeksforgeeks.org/reactjs/reactjs-lifecycle-components/

[^5]: https://builtin.com/software-engineering-perspectives/react-error-boundary

[^6]: https://www.geeksforgeeks.org/reactjs/reactjs-class-components/

[^7]: https://www.w3schools.com/react/react_lifecycle.asp

[^8]: https://www.scaler.com/topics/react/react-life-cycle-methods/

[^9]: https://www.freecodecamp.org/news/function-component-vs-class-component-in-react/

[^10]: https://legacy.reactjs.org/docs/error-boundaries.html

[^11]: https://refine.dev/blog/react-error-boundaries/

[^12]: https://stackoverflow.com/questions/48482619/how-can-i-make-use-of-error-boundaries-in-functional-react-components

[^13]: https://react.dev/reference/react/Component

[^14]: https://www.w3schools.com/react/react_components.asp

[^15]: https://legacy.reactjs.org/docs/components-and-props.html

[^16]: https://www.tothenew.com/blog/understanding-the-react-component-lifecycle-hooks-vs-class-components/

[^17]: https://www.freecodecamp.org/news/react-lifecycle-methods-and-hooks-for-beginners/

[^18]: https://react.dev/learn/your-first-component

[^19]: https://legacy.reactjs.org/docs/react-component.html

[^20]: https://legacy.reactjs.org/docs/state-and-lifecycle.html

