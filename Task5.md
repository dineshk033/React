# React Capstone Project Scaffolding: useReducer, useEffect, Custom Hooks

## What is this project?

This capstone project is a **Task Manager App** built with React. It is designed to help you learn and practice three key React concepts:

- **useReducer**: For managing complex state (the list of tasks).
- **useEffect**: For handling side effects, such as fetching data when the app loads.
- **Custom Hooks**: For encapsulating reusable logic (like fetching tasks from an API).

You will be given only the function signatures and structure (scaffolding). Your job is to fill in the code where indicated by comments.

---

## What needs to be filled in?

- **Custom Hook (`useFetchTasks`)**: Write the logic for fetching tasks (simulate with setTimeout).
- **Reducer Function**: Implement the logic for handling actions (`initialize`, `add`, `toggle`) for the task state.
- **Task Manager Component**: Combine the custom hook, reducer, and effect to build the UI and manage state.
- **Main App Component**: Render the Task Manager.

**You will learn:**
- How to design and use a custom hook
- How to manage state with a reducer
- How to use effects for lifecycle events
- How to connect all these concepts in a working React component

---

## 1. Custom Hook Scaffolding (`useFetchTasks.js`)

```javascript
// hooks/useFetchTasks.js
import { useState, useEffect } from "react";

/*
  Custom hook for fetching tasks.
  - Takes a URL as input.
  - Returns { tasks, loading }
  Fill in the logic!
*/
export default function useFetchTasks(url) {
  // Declare state variables

  // useEffect: Fetch tasks from API (simulate with setTimeout)

  // Return tasks and loading state
}
```

---

## 2. Reducer Function Scaffolding

```javascript
// components/TaskManager.js

/*
  Reducer function for managing task state.
  - Handles 'initialize', 'add', 'toggle' actions.
  Fill in the logic!
*/
function taskReducer(state, action) {
  // Write your switch logic here
}
```

---

## 3. Task Manager Component Scaffolding

```javascript
// components/TaskManager.js
import React, { useReducer, useEffect, useState } from "react";
import useFetchTasks from "../hooks/useFetchTasks";

/*
  TaskManager component
  - Uses useReducer for state
  - Uses useFetchTasks for API data
  - useEffect to initialize state from fetched tasks
  Fill in the logic!
*/
export default function TaskManager() {
  // Use custom hook to fetch tasks

  // useReducer for tasks

  // useState for input field

  // useEffect: Initialize reducer state when loaded

  // Handler for adding a task

  return (
    <div>
      <h2>Task Manager</h2>
      {/* Input and add button */}
      {/* Render loading state or task list */}
      {/* Each task: click to toggle complete */}
    </div>
  );
}
```

---

## 4. Main App Scaffolding

```javascript
// App.js
import React from "react";
import TaskManager from "./components/TaskManager";

/*
  Main App component
  Fill in the logic!
*/
function App() {
  return (
    <div>
      {/* Render TaskManager */}
    </div>
  );
}

export default App;
```

---

# Instructions

1. **Fill in the logic** where comments indicate.
2. Use `useReducer` for state transitions, `useEffect` for side effects, and a custom hook for data fetching.
3. Simulate API calls using `setTimeout` in your hook.
4. Add handlers for adding tasks and toggling their completion status.
5. Test your app by running it and adding/toggling tasks in the UI.

---

**This scaffolding helps you practice React state, effects, and hooks by implementing the core logic yourself!**