# Why Use Axios Instead of Fetch in React?

When building React applications that interact with APIs, you’ll often choose between the native `fetch` API and a third-party library like **Axios**. Here’s an architectural perspective on why Axios is commonly preferred, and what features it offers over fetch.

---

## 1. Axios vs Fetch: Key Differences

| Feature              | Axios                      | Fetch (Native)            |
|----------------------|---------------------------|---------------------------|
| Promise-based        | Yes                       | Yes                       |
| Request/Response     | Automatically transforms  | Manual handling           |
| Interceptors         | Yes                       | No                        |
| Global config        | Yes                       | No                        |
| Timeout support      | Yes                       | Manual workaround         |
| Error handling       | Handles HTTP errors       | Must check manually       |
| Browser/Node support | Yes                       | Browser only (native)     |
| Cancel requests      | Yes (CancelToken)         | Manual workaround         |
| Progress events      | Yes                       | Limited                   |

---

## 2. Features of Axios

### a) **Automatic JSON Data Transformation**
- Axios automatically parses JSON responses.
- With fetch, you need to call `.json()` manually.

```js
// Axios
axios.get('/api/data').then(res => console.log(res.data));

// Fetch
fetch('/api/data')
  .then(res => res.json())
  .then(data => console.log(data));
```

---

### b) **Global Configuration**
- Set base URLs, headers, or timeouts globally for all requests.

```js
const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: { 'Authorization': 'Bearer ...' }
});
```

---

### c) **Interceptors**
- Run logic before requests (e.g., add auth token) or after responses (e.g., error handling).

```js
api.interceptors.request.use(config => {
  // Add token or modify request
  return config;
});
api.interceptors.response.use(
  response => response,
  error => {
    // Handle error globally
    return Promise.reject(error);
  }
);
```

---

### d) **Error Handling**
- Axios treats HTTP errors (like 404/500) as errors.
- Fetch only throws for network errors; you must check response status manually.

```js
// Axios
api.get('/bad-url').catch(error => {
  console.log(error.response.status); // 404, 500, etc.
});

// Fetch
fetch('/bad-url')
  .then(res => {
    if (!res.ok) throw new Error(res.status);
    return res.json();
  })
  .catch(error => console.log(error));
```

---

### e) **Request Cancellation**
- Axios supports cancelling requests natively.

```js
const source = axios.CancelToken.source();
api.get('/data', { cancelToken: source.token });
// To cancel:
source.cancel('Operation canceled by the user.');
```

---

### f) **Timeouts**
- Specify request timeouts directly in config.

```js
axios.get('/slow-api', { timeout: 1000 });
```

---

### g) **Other Features**
- Supports form data, query string serialization, multipart uploads, and more.
- Works in both browsers and Node.js.
- Progress events for uploads/downloads.

---

## 3. When to Use Axios?

- When you want a robust, feature-rich HTTP client.
- When you need global error handling, interceptors, or configuration.
- When you want easier handling of JSON, timeouts, and cancellation.

## 4. When is Fetch Enough?

- For quick, simple requests in modern browsers.
- When you don’t need advanced features or global config.

---

**Summary:**  
Axios provides a higher-level, more convenient API for HTTP requests, global configuration, error handling, interceptors, and more. For scalable React apps, Axios is often the better architectural choice.
