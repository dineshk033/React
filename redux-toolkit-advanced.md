# Advanced Redux Toolkit: `createAsyncThunk` Explained with Todo Example

## What is `createAsyncThunk`?

`createAsyncThunk` is a Redux Toolkit API for handling asynchronous logic in Redux, such as fetching data from an API. It automates action creators for request, success, and failure, and integrates perfectly with slices for loading/error states.

### Why Use `createAsyncThunk`?

- Handles async operations (API calls, etc.) in Redux state.
- Automatically generates pending, fulfilled, and rejected action types.
- Supports error handling and loading states.
- Works seamlessly with reducers and slices.

## How `createAsyncThunk` Works

When you define a thunk with `createAsyncThunk`, it:
1. Dispatches a `pending` action when called.
2. Runs your async code (usually an API call).
3. If successful, dispatches a `fulfilled` action with the result.
4. If there's an error, dispatches a `rejected` action with the error.

## Example: Async Operations in Todo Application

Let’s expand our Todo app to include API calls. Imagine we want to fetch todos from a server and add a todo asynchronously.

### 1. Update the Slice to Use Async Thunks

```javascript name=todoSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Example API URLs
const API_URL = 'https://example.com/api/todos';

// 1. Async thunk for fetching todos
export const fetchTodos = createAsyncThunk(
  'todos/fetchTodos',
  async () => {
    const response = await fetch(API_URL);
    if (!response.ok) throw new Error('Failed to fetch todos');
    return response.json(); // Array of todos
  }
);

// 2. Async thunk for adding a todo
export const addTodoAsync = createAsyncThunk(
  'todos/addTodoAsync',
  async (todo) => {
    const response = await fetch(API_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(todo),
    });
    if (!response.ok) throw new Error('Failed to add todo');
    return response.json(); // The added todo object
  }
);

const todoSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    loading: false,
    error: null,
  },
  reducers: {
    removeTodo: (state, action) => {
      state.items = state.items.filter(todo => todo.id !== action.payload);
    },
    updateTodo: (state, action) => {
      const { id, text, completed } = action.payload;
      const todo = state.items.find(todo => todo.id === id);
      if (todo) {
        todo.text = text !== undefined ? text : todo.text;
        if (completed !== undefined) todo.completed = completed;
      }
    },
    // You can keep your sync addTodo if needed
    addTodo: (state, action) => {
      state.items.push(action.payload);
    },
  },
  extraReducers: (builder) => {
    builder
      // Fetch Todos
      .addCase(fetchTodos.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      })
      // Add Todo Async
      .addCase(addTodoAsync.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(addTodoAsync.fulfilled, (state, action) => {
        state.loading = false;
        state.items.push(action.payload);
      })
      .addCase(addTodoAsync.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});

export const { addTodo, removeTodo, updateTodo } = todoSlice.actions;
export default todoSlice.reducer;
```

### 2. Using Thunks in Components

```javascript name=TodoAppAsync.js
import React, { useState, useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchTodos, addTodoAsync, removeTodo, updateTodo } from './todoSlice';

function TodoAppAsync() {
  const { items: todos, loading, error } = useSelector(state => state.todos);
  const dispatch = useDispatch();
  const [text, setText] = useState('');

  useEffect(() => {
    dispatch(fetchTodos());
  }, [dispatch]);

  const handleAdd = () => {
    if (text.trim()) {
      // Add todo via API
      dispatch(addTodoAsync({ text, completed: false }));
      setText('');
    }
  };

  const handleUpdate = (id) => {
    const newText = prompt('Update todo:');
    if (newText)
      dispatch(updateTodo({ id, text: newText }));
  };

  return (
    <div>
      <h2>Async Todo List</h2>
      {loading && <p>Loading...</p>}
      {error && <p style={{color:'red'}}>Error: {error}</p>}
      <input
        value={text}
        onChange={e => setText(e.target.value)}
        placeholder="Add todo"
      />
      <button onClick={handleAdd} disabled={loading}>Add</button>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            {todo.text}
            <button onClick={() => handleUpdate(todo.id)}>Update</button>
            <button onClick={() => dispatch(removeTodo(todo.id))}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoAppAsync;
```

### 3. Summary of Async Flow

- **Dispatch `fetchTodos` on mount** to load initial data.
- **Dispatch `addTodoAsync`** to add a todo via API.
- Redux Toolkit automatically manages:
  - `loading` and `error` states.
  - Actions for pending, fulfilled, rejected.

### 4. Architecture Diagram: Including Async

```plaintext
[Component] --> dispatch(fetchTodos) --> [Thunk API call]
                                  |
                               [Redux Store]
                                  |
                 [pending/fulfilled/rejected actions]
                                  |
                        [Slice updates state]
                                  |
                        [Component re-renders]
```

## Key Points

- Use `createAsyncThunk` for all async operations in Redux Toolkit.
- Handle loading/error states in your slice.
- Use `extraReducers` to respond to thunk actions.

## References

- [Redux Toolkit: createAsyncThunk](https://redux-toolkit.js.org/api/createAsyncThunk)
- [Redux Toolkit Tutorials](https://redux-toolkit.js.org/tutorials/advanced-tutorial)