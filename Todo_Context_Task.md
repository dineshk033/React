# React Context Task: Todo Application Context Implementation

## Task Overview

**Goal:**  
Create a Todo Context in a React app to manage a list of todos globally, allowing components to add, remove, and toggle completion of todo items.

This moderate-level task will help you practice context creation, provider setup, and context consumption for managing application state shared across multiple components.

---

## What You Need to Implement

### 1. Create a TodoContext

- Define a new context called `TodoContext` (e.g., in `TodoContext.js`).
- This context should provide:
  - The current list of todos.
  - An `addTodo` function to add a new todo.
  - A `removeTodo` function to remove a todo by ID.
  - A `toggleTodo` function to mark a todo as completed or not.

---

### 2. Create a Provider Component

- Create a `TodoProvider` component (e.g., in `TodoProvider.js`) that uses React state to manage the todos.
- Wrap your application with `TodoProvider` so any component can access and interact with the todo list.

---

### 3. Consume Context in Components

- Make an `AddTodoForm` component:
  - Contains an input for entering a new todo.
  - On submit, calls `addTodo` from context.

- Make a `TodoList` component:
  - Displays the list of todos.
  - For each todo, show the text and a button to remove it (calls `removeTodo`).
  - For each todo, show a checkbox to toggle completion (calls `toggleTodo`).

---

### 4. Wire Everything Together

- In your `App.js`, wrap everything in the `TodoProvider`.
- Render the `AddTodoForm` and `TodoList` components.

---

## Example File Structure

```
src/
  TodoContext.js
  TodoProvider.js
  AddTodoForm.js
  TodoList.js
  App.js
```

---

## Step-by-Step Implementation Guide

### Step 1: Create Context

```jsx
// TodoContext.js
import React from "react";
// TODO: Create and export TodoContext using React.createContext()
// Example: const TodoContext = React.createContext();
// export default TodoContext;
```

---

### Step 2: Create Provider

```jsx
// TodoProvider.js
import React, { useState } from "react";
import TodoContext from "./TodoContext";

// TODO: Create TodoProvider component
// - Use useState to manage array of todos
// - Implement addTodo, removeTodo, and toggleTodo functions
// - Provide { todos, addTodo, removeTodo, toggleTodo } via TodoContext.Provider
// - Render children inside Provider
```

---

### Step 3: Create Components

```jsx
// AddTodoForm.js
import React, { useContext, useState } from "react";
import TodoContext from "./TodoContext";

// TODO: Use useState to manage input value
// - On form submit, call addTodo from context with new todo text
// - Clear input after adding
```

```jsx
// TodoList.js
import React, { useContext } from "react";
import TodoContext from "./TodoContext";

// TODO: Consume TodoContext
// - Display list of todos
// - For each todo:
//     - Show the text
//     - Checkbox to toggle completed status (calls toggleTodo)
//     - Button to remove todo (calls removeTodo)
```

---

### Step 4: Wire Up in App.js

```jsx
// App.js
import React from "react";
import { TodoProvider } from "./TodoProvider";
import AddTodoForm from "./AddTodoForm";
import TodoList from "./TodoList";

// TODO: Wrap the application in TodoProvider
// - Render AddTodoForm and TodoList inside TodoProvider
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
2. Test adding, removing, and toggling todos.
3. Try extending functionality: persist todos in local storage, add editing capability, or filter completed/uncompleted todos.

**Ask if you need help or suggestions for improvements!**