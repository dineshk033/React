# Using Promises in React with Axios

As an architect, I'll guide you step-by-step on how to use Promises in React, focusing on Axios for API requests. We'll cover:
- What Promises are
- How to use Axios with Promises in React
- Global Axios configuration (`baseURL`, `timeout`)
- Interceptors (request/response)
- Global error handling
- Examples for each step

---

## 1. What is a Promise?

A **Promise** is a JavaScript object representing the eventual completion (success or failure) of an asynchronous operation. It allows you to run code asynchronously and react to its outcome.

### Promise Example

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
    // or reject("Error!");
  }, 1000);
});

myPromise
  .then(result => console.log(result)) // Success!
  .catch(error => console.log(error)); // if rejected
```

---

## 2. Using Axios with Promises in React

**Axios** is a popular HTTP client that returns Promises for API calls.

### Basic Usage

```js
import axios from 'axios';

axios.get('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

### Usage in React (with useEffect)

```jsx
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function Post() {
  const [post, setPost] = useState(null);

  useEffect(() => {
    axios.get('https://jsonplaceholder.typicode.com/posts/1')
      .then(response => setPost(response.data))
      .catch(error => console.error(error));
  }, []);

  return (
    <div>
      <h2>Post Data</h2>
      {post ? <pre>{JSON.stringify(post, null, 2)}</pre> : "Loading..."}
    </div>
  );
}
```

---

## 3. Global Axios Configuration (baseURL, timeout)

Configure Axios globally to set defaults (base URL, timeout, etc.) for all requests.

### 3.1. Creating an Axios Instance

```js
// src/api/axiosInstance.js
import axios from 'axios';

const axiosInstance = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 5000, // 5 seconds
});

export default axiosInstance;
```

### 3.2. Using the Instance

```js
import axiosInstance from './api/axiosInstance';

axiosInstance.get('/posts/1')
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

---

## 4. Axios Interceptors

**Interceptors** let you run code before a request is sent or after a response is received (e.g., for authentication, logging, error handling).

### 4.1. Request Interceptor Example

```js
// src/api/axiosInstance.js
axiosInstance.interceptors.request.use(
  config => {
    // Add token if available
    const token = localStorage.getItem('authToken');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  error => Promise.reject(error)
);
```

### 4.2. Response Interceptor Example

```js
// src/api/axiosInstance.js
axiosInstance.interceptors.response.use(
  response => response,
  error => {
    // Handle global errors
    if (error.response && error.response.status === 401) {
      // Unauthorized: Redirect to login (example)
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

---

## 5. Global Error Handling

Centralized error handling in Axios interceptors lets you manage errors across your application.

### Example: Showing a Toast on Error

```js
import { toast } from 'react-toastify'; // npm install react-toastify

axiosInstance.interceptors.response.use(
  response => response,
  error => {
    toast.error(error.message || "Something went wrong!");
    return Promise.reject(error);
  }
);
```

---

## 6. Example: Using Configured Axios in a React Component

```jsx
import React, { useEffect, useState } from 'react';
import axiosInstance from './api/axiosInstance';

function Todo() {
  const [todo, setTodo] = useState(null);

  useEffect(() => {
    axiosInstance.get('/todos/1')
      .then(response => setTodo(response.data))
      .catch(error => console.error(error));
  }, []);

  return (
    <div>
      <h2>Todo Data</h2>
      {todo ? <pre>{JSON.stringify(todo, null, 2)}</pre> : "Loading..."}
    </div>
  );
}
```

---

## Summary

- **Promises** handle async operations and their results.
- **Axios** uses Promises for HTTP requests in React.
- Configure Axios globally for `baseURL`, `timeout`, etc.
- Use **interceptors** for request/response logic and global error handling.

**Pro Tip:** Centralizing your Axios configuration improves maintainability, security, and error handling in your React apps!