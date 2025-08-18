# Complete Guide to React with Vite and JSX Fundamentals

## Table of Contents
1. [Introduction to Vite](#introduction-to-vite)
2. [Creating React App with Vite](#creating-react-app-with-vite)
3. [Project Structure Deep Dive](#project-structure-deep-dive)
4. [Development Workflow](#development-workflow)
5. [JSX Fundamentals](#jsx-fundamentals)
6. [JSX Rules and Limitations](#jsx-rules-and-limitations)
7. [JSX Advanced Concepts](#jsx-advanced-concepts)
8. [Best Practices](#best-practices)
9. [Common Pitfalls and Solutions](#common-pitfalls-and-solutions)
10. [Practical Examples](#practical-examples)

---

## Introduction to Vite

### What is Vite?
Vite (pronounced "veet") is a modern build tool created by Evan You (creator of Vue.js). It provides a fast development server and optimized build process for modern web projects.

### Why Vite over Create React App (CRA)?

| Feature | Vite | Create React App |
|---------|------|------------------|
| **Development Server** | ⚡ Lightning fast (ESM-based) | 🐌 Slower (webpack-based) |
| **Hot Module Replacement** | ⚡ Instant | 🔄 Slower |
| **Bundle Size** | 📦 Smaller | 📦 Larger |
| **Build Time** | ⚡ Faster (Rollup) | 🐌 Slower (webpack) |
| **Configuration** | 🎛️ Flexible | 🔒 Opinionated (ejection needed) |
| **TypeScript Support** | ✅ Built-in | ✅ Built-in |
| **Plugin Ecosystem** | 🔌 Rich (Rollup plugins) | 🔌 webpack plugins |

### Key Benefits of Vite:
- **Fast Cold Start**: No bundling during development
- **Instant Hot Updates**: Changes reflect immediately
- **Rich Plugin Ecosystem**: Rollup-compatible plugins
- **Built-in TypeScript Support**: No additional configuration
- **Optimized Build**: Tree-shaking and code splitting out of the box

---

## Creating React App with Vite

### Prerequisites
```bash
# Check Node.js version (18.0+ recommended)
node --version

# Check npm version
npm --version
```

### Step 1: Create New React Project

#### Method 1: Using npm create
```bash
npm create vite@latest my-react-app -- --template react

# With TypeScript
npm create vite@latest my-react-app -- --template react-ts
```

#### Method 2: Using yarn
```bash
yarn create vite my-react-app --template react

# With TypeScript
yarn create vite my-react-app --template react-ts
```

#### Method 3: Using pnpm
```bash
pnpm create vite my-react-app --template react

# With TypeScript
pnpm create vite my-react-app --template react-ts
```

### Step 2: Navigate and Install Dependencies
```bash
cd my-react-app
npm install

# Or with yarn
yarn

# Or with pnpm
pnpm install
```

### Step 3: Start Development Server
```bash
npm run dev

# Or with yarn
yarn dev

# Or with pnpm
pnpm dev
```

### Step 4: Available Commands
```bash
# Development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Lint code (if ESLint is configured)
npm run lint
```

---

## Project Structure Deep Dive

### Initial Project Structure
```
my-react-app/
├── public/                 # Static assets
│   ├── vite.svg           # Vite logo
│   └── favicon.ico        # Favicon
├── src/                   # Source code
│   ├── assets/           # Assets (images, fonts, etc.)
│   │   └── react.svg     # React logo
│   ├── App.css           # App component styles
│   ├── App.jsx           # Main App component
│   ├── index.css         # Global styles
│   └── main.jsx          # Application entry point
├── .gitignore            # Git ignore rules
├── eslint.config.js      # ESLint configuration
├── index.html            # HTML template
├── package.json          # Dependencies and scripts
├── README.md             # Project documentation
└── vite.config.js        # Vite configuration
```

### Understanding Key Files

#### `index.html` (Application Template)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React</title>
  </head>
  <body>
    <div id="root"></div>
    <!-- Vite injects the script here -->
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

#### `src/main.jsx` (Entry Point)
```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

// React 18 Concurrent Mode
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

#### `src/App.jsx` (Main Component)
```javascript
import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'

function App() {
  const [count, setCount] = useState(0)

  return (
    <>
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.jsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </>
  )
}

export default App
```

#### `vite.config.js` (Vite Configuration)
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  // Additional configuration options
  server: {
    port: 3000,
    open: true
  },
  build: {
    outDir: 'dist',
    sourcemap: true
  }
})
```

---

## Development Workflow

### Development Server Features
- **Hot Module Replacement (HMR)**: Changes reflect instantly
- **Error Overlay**: Clear error messages in browser
- **CSS HMR**: Styles update without page reload
- **Fast Refresh**: React state preserved during updates

### Build Process
```bash
# Production build
npm run build

# Files generated in 'dist' folder:
dist/
├── assets/
│   ├── index-[hash].css    # Minified CSS
│   └── index-[hash].js     # Minified JavaScript
├── index.html              # HTML with asset links
└── vite.svg               # Static assets
```

### Environment Variables
```bash
# .env file
VITE_API_URL=https://api.example.com
VITE_APP_TITLE=My React App

# Usage in code
const apiUrl = import.meta.env.VITE_API_URL;
const appTitle = import.meta.env.VITE_APP_TITLE;
```

**Important**: Environment variables must be prefixed with `VITE_` to be accessible in client-side code.

---

## JSX Fundamentals

### What is JSX?
JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code within JavaScript. It's not HTML, but a syntactic sugar that gets transpiled to JavaScript function calls.

### JSX Transformation
```javascript
// JSX Code
const element = <h1>Hello, World!</h1>;

// Transpiled to (React 18)
const element = React.createElement('h1', null, 'Hello, World!');

// Transpiled to (React 17+ with new JSX Transform)
import { jsx as _jsx } from 'react/jsx-runtime';
const element = _jsx('h1', { children: 'Hello, World!' });
```

### Basic JSX Syntax

#### Elements and Attributes
```javascript
// HTML-like syntax
const element = <div className="container">Hello, World!</div>;

// With attributes
const image = <img src="logo.png" alt="Logo" width="100" height="50" />;

// Boolean attributes
const input = <input disabled />;
const checkbox = <input type="checkbox" checked={true} />;
```

#### Embedding JavaScript Expressions
```javascript
const name = 'John Doe';
const age = 25;

const greeting = (
  <div>
    <h1>Hello, {name}!</h1>
    <p>You are {age} years old.</p>
    <p>Next year you'll be {age + 1}.</p>
  </div>
);
```

#### Conditional Rendering
```javascript
const isLoggedIn = true;
const username = 'Alice';

// Using ternary operator
const header = (
  <div>
    {isLoggedIn ? (
      <h1>Welcome back, {username}!</h1>
    ) : (
      <h1>Please sign in.</h1>
    )}
  </div>
);

// Using logical AND (&&)
const notification = (
  <div>
    {isLoggedIn && <p>You have 5 new messages.</p>}
  </div>
);
```

#### Rendering Lists
```javascript
const fruits = ['apple', 'banana', 'orange', 'grape'];

const fruitList = (
  <ul>
    {fruits.map((fruit, index) => (
      <li key={index}>{fruit}</li>
    ))}
  </ul>
);

// Better with unique keys
const users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
  { id: 3, name: 'Charlie' }
];

const userList = (
  <ul>
    {users.map(user => (
      <li key={user.id}>{user.name}</li>
    ))}
  </ul>
);
```

---

## JSX Rules and Limitations

### 1. Single Root Element (React 17 and earlier)
```javascript
// ❌ Invalid - Multiple root elements
return (
  <h1>Title</h1>
  <p>Paragraph</p>
);

// ✅ Valid - Single root element
return (
  <div>
    <h1>Title</h1>
    <p>Paragraph</p>
  </div>
);

// ✅ Valid - React Fragment
return (
  <React.Fragment>
    <h1>Title</h1>
    <p>Paragraph</p>
  </React.Fragment>
);

// ✅ Valid - Short Fragment syntax
return (
  <>
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);
```

### 2. Self-Closing Tags
```javascript
// ❌ Invalid - HTML void elements must be self-closed
const image = <img src="logo.png" alt="Logo">;
const input = <input type="text">;
const br = <br>;

// ✅ Valid - Self-closed tags
const image = <img src="logo.png" alt="Logo" />;
const input = <input type="text" />;
const br = <br />;
```

### 3. Reserved Words and Attribute Names
```javascript
// ❌ Invalid - 'class' is reserved in JavaScript
const element = <div class="container">Content</div>;

// ✅ Valid - Use 'className' instead
const element = <div className="container">Content</div>;

// ❌ Invalid - 'for' is reserved
const label = <label for="input">Label</label>;

// ✅ Valid - Use 'htmlFor' instead
const label = <label htmlFor="input">Label</label>;
```

### 4. Case Sensitivity
```javascript
// ❌ Invalid - HTML attributes should be camelCase
const element = <div onclick={handleClick} tabindex="0">Click me</div>;

// ✅ Valid - camelCase attributes
const element = <div onClick={handleClick} tabIndex="0">Click me</div>;

// Component names must start with capital letter
// ❌ Invalid
const myComponent = () => <div>Hello</div>;
return <myComponent />; // Treated as HTML element

// ✅ Valid
const MyComponent = () => <div>Hello</div>;
return <MyComponent />; // Treated as React component
```

### 5. JavaScript Expressions vs Statements
```javascript
// ✅ Valid - Expressions (return values)
const element = (
  <div>
    <p>{userName}</p>
    <p>{age + 1}</p>
    <p>{isLoggedIn ? 'Welcome' : 'Please login'}</p>
    <p>{users.map(user => user.name).join(', ')}</p>
  </div>
);

// ❌ Invalid - Statements (don't return values)
const element = (
  <div>
    {if (isLoggedIn) { return <p>Welcome</p>; }} // ❌ if statement
    {for (let i = 0; i < 5; i++) { /* ... */ }} // ❌ for loop
    {const message = 'Hello';} // ❌ variable declaration
  </div>
);
```

### 6. Comments in JSX
```javascript
const element = (
  <div>
    {/* This is a comment in JSX */}
    <h1>Title</h1>
    
    {/* 
      Multi-line comment
      in JSX 
    */}
    <p>Paragraph</p>
    
    {/* ❌ Don't use HTML comments */}
    <!-- This won't work -->
  </div>
);
```

### 7. String Literals and Escaping
```javascript
// ✅ Valid - String literals
const element = <div>Hello "World"</div>;
const element2 = <div>Hello 'World'</div>;

// ✅ Valid - Escaping quotes in attributes
const element3 = <div title="Say \"Hello\"">Content</div>;
const element4 = <div title='Say \'Hello\''>Content</div>;

// ✅ Valid - Using template literals
const message = `Hello ${name}`;
const element5 = <div>{message}</div>;
```

### 8. Event Handlers
```javascript
// ❌ Invalid - String event handlers (like HTML)
const button = <button onclick="handleClick()">Click me</button>;

// ✅ Valid - Function reference
const handleClick = () => {
  console.log('Button clicked!');
};
const button = <button onClick={handleClick}>Click me</button>;

// ✅ Valid - Inline function
const button2 = <button onClick={() => console.log('Clicked!')}>
  Click me
</button>;

// ✅ Valid - Function with parameters
const button3 = <button onClick={(e) => handleClick(e, 'data')}>
  Click me
</button>;
```

---

## JSX Advanced Concepts

### 1. Fragments and Keys
```javascript
// Fragment with key (useful in lists)
const ListItem = ({ items }) => (
  <React.Fragment key="list-fragment">
    {items.map(item => (
      <div key={item.id}>{item.name}</div>
    ))}
  </React.Fragment>
);

// Short syntax doesn't support keys
const items = [
  <>
    <td>Cell 1</td>
    <td>Cell 2</td>
  </>,
  // ❌ Can't add key to short syntax
];
```

### 2. Dynamic Tag Names
```javascript
const DynamicElement = ({ tag, children, ...props }) => {
  const Tag = tag; // Must be capitalized
  return <Tag {...props}>{children}</Tag>;
};

// Usage
<DynamicElement tag="h1" className="title">
  Dynamic Heading
</DynamicElement>
```

### 3. Spread Attributes
```javascript
const props = {
  className: 'btn btn-primary',
  disabled: false,
  onClick: handleClick
};

// ✅ Spread operator
const button = <button {...props}>Click me</button>;

// ✅ Override specific props
const button2 = <button {...props} disabled={true}>Click me</button>;

// ✅ Conditional spread
const conditionalProps = isDisabled ? { disabled: true } : {};
const button3 = <button {...props} {...conditionalProps}>Click me</button>;
```

### 4. Children Prop
```javascript
// Children as a prop
const Card = ({ children, title }) => (
  <div className="card">
    <h2>{title}</h2>
    <div className="card-body">
      {children}
    </div>
  </div>
);

// Usage
<Card title="My Card">
  <p>This is the card content.</p>
  <button>Action</button>
</Card>

// Children as function (render props)
const DataProvider = ({ children }) => {
  const [data, setData] = useState([]);
  
  return children({ data, setData });
};

// Usage
<DataProvider>
  {({ data, setData }) => (
    <div>
      <p>Data count: {data.length}</p>
      <button onClick={() => setData([...data, 'new item'])}>
        Add Item
      </button>
    </div>
  )}
</DataProvider>
```

### 5. Conditional Attributes
```javascript
const Input = ({ isRequired, isDisabled, placeholder }) => (
  <input
    type="text"
    placeholder={placeholder}
    // Conditional attributes
    {...(isRequired && { required: true })}
    {...(isDisabled && { disabled: true })}
    // Alternative approach
    required={isRequired || undefined}
    disabled={isDisabled || undefined}
  />
);
```

### 6. Dangerous HTML (dangerouslySetInnerHTML)
```javascript
// ⚠️ Use with caution - XSS vulnerability risk
const HtmlContent = ({ htmlString }) => (
  <div dangerouslySetInnerHTML={{ __html: htmlString }} />
);

// Safer approach - sanitize first
import DOMPurify from 'dompurify';

const SafeHtmlContent = ({ htmlString }) => {
  const sanitizedHtml = DOMPurify.sanitize(htmlString);
  return <div dangerouslySetInnerHTML={{ __html: sanitizedHtml }} />;
};
```

---

## Best Practices

### 1. Component Organization
```javascript
// ✅ Good - Clear component structure
const UserProfile = ({ user, onUpdate }) => {
  const [isEditing, setIsEditing] = useState(false);
  
  const handleSave = (userData) => {
    onUpdate(userData);
    setIsEditing(false);
  };
  
  if (isEditing) {
    return <UserEditForm user={user} onSave={handleSave} />;
  }
  
  return (
    <div className="user-profile">
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      <button onClick={() => setIsEditing(true)}>
        Edit Profile
      </button>
    </div>
  );
};
```

### 2. Meaningful Keys
```javascript
// ❌ Bad - Using array index as key
const TodoList = ({ todos }) => (
  <ul>
    {todos.map((todo, index) => (
      <li key={index}>{todo.text}</li>
    ))}
  </ul>
);

// ✅ Good - Using unique identifier
const TodoList = ({ todos }) => (
  <ul>
    {todos.map(todo => (
      <li key={todo.id}>{todo.text}</li>
    ))}
  </ul>
);
```

### 3. Event Handler Patterns
```javascript
// ✅ Good - Avoid inline functions in render
const TodoItem = ({ todo, onToggle, onDelete }) => {
  const handleToggle = () => onToggle(todo.id);
  const handleDelete = () => onDelete(todo.id);
  
  return (
    <li>
      <span onClick={handleToggle}>{todo.text}</span>
      <button onClick={handleDelete}>Delete</button>
    </li>
  );
};

// ✅ Alternative - Using useCallback for optimization
const TodoItem = ({ todo, onToggle, onDelete }) => {
  const handleToggle = useCallback(() => {
    onToggle(todo.id);
  }, [todo.id, onToggle]);
  
  const handleDelete = useCallback(() => {
    onDelete(todo.id);
  }, [todo.id, onDelete]);
  
  return (
    <li>
      <span onClick={handleToggle}>{todo.text}</span>
      <button onClick={handleDelete}>Delete</button>
    </li>
  );
};
```

### 4. Conditional Rendering Best Practices
```javascript
// ✅ Good - Clear conditional rendering
const UserDashboard = ({ user, isLoading, error }) => {
  if (isLoading) {
    return <LoadingSpinner />;
  }
  
  if (error) {
    return <ErrorMessage error={error} />;
  }
  
  if (!user) {
    return <LoginPrompt />;
  }
  
  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <UserStats user={user} />
      <RecentActivity user={user} />
    </div>
  );
};

// ✅ Good - Short-circuit evaluation
const Notification = ({ message, type }) => (
  <div>
    {message && (
      <div className={`notification ${type}`}>
        {message}
      </div>
    )}
  </div>
);
```

---

## Common Pitfalls and Solutions

### 1. Forgetting Keys in Lists
```javascript
// ❌ Problem - Missing keys
const ItemList = ({ items }) => (
  <ul>
    {items.map(item => <li>{item.name}</li>)}
  </ul>
);

// ✅ Solution
const ItemList = ({ items }) => (
  <ul>
    {items.map(item => <li key={item.id}>{item.name}</li>)}
  </ul>
);
```

### 2. Incorrect Event Handler Binding
```javascript
// ❌ Problem - Function called immediately
const ButtonList = ({ items, onItemClick }) => (
  <div>
    {items.map(item => (
      <button key={item.id} onClick={onItemClick(item.id)}>
        {item.name}
      </button>
    ))}
  </div>
);

// ✅ Solution - Arrow function
const ButtonList = ({ items, onItemClick }) => (
  <div>
    {items.map(item => (
      <button key={item.id} onClick={() => onItemClick(item.id)}>
        {item.name}
      </button>
    ))}
  </div>
);
```

### 3. State Mutation
```javascript
// ❌ Problem - Mutating state directly
const TodoList = () => {
  const [todos, setTodos] = useState([]);
  
  const addTodo = (text) => {
    todos.push({ id: Date.now(), text }); // ❌ Direct mutation
    setTodos(todos);
  };
  
  return (
    <div>
      {todos.map(todo => <div key={todo.id}>{todo.text}</div>)}
    </div>
  );
};

// ✅ Solution - Create new array
const TodoList = () => {
  const [todos, setTodos] = useState([]);
  
  const addTodo = (text) => {
    const newTodo = { id: Date.now(), text };
    setTodos([...todos, newTodo]); // ✅ Create new array
  };
  
  return (
    <div>
      {todos.map(todo => <div key={todo.id}>{todo.text}</div>)}
    </div>
  );
};
```

### 4. Missing Dependencies in Hooks
```javascript
// ❌ Problem - Missing dependencies
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, []); // ❌ Missing userId dependency
  
  return user ? <div>{user.name}</div> : <div>Loading...</div>;
};

// ✅ Solution - Include all dependencies
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // ✅ Include userId
  
  return user ? <div>{user.name}</div> : <div>Loading...</div>;
};
```

---

## Practical Examples

### Example 1: Complete Todo Application
```javascript
import React, { useState, useCallback } from 'react';

const TodoApp = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [filter, setFilter] = useState('all'); // all, active, completed

  const addTodo = useCallback(() => {
    if (inputValue.trim()) {
      const newTodo = {
        id: Date.now(),
        text: inputValue.trim(),
        completed: false,
        createdAt: new Date().toISOString()
      };
      setTodos(prev => [...prev, newTodo]);
      setInputValue('');
    }
  }, [inputValue]);

  const toggleTodo = useCallback((id) => {
    setTodos(prev => 
      prev.map(todo => 
        todo.id === id 
          ? { ...todo, completed: !todo.completed }
          : todo
      )
    );
  }, []);

  const deleteTodo = useCallback((id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  }, []);

  const clearCompleted = useCallback(() => {
    setTodos(prev => prev.filter(todo => !todo.completed));
  }, []);

  const filteredTodos = todos.filter(todo => {
    switch (filter) {
      case 'active':
        return !todo.completed;
      case 'completed':
        return todo.completed;
      default:
        return true;
    }
  });

  const activeCount = todos.filter(todo => !todo.completed).length;
  const completedCount = todos.filter(todo => todo.completed).length;

  return (
    <div className="todo-app">
      <header>
        <h1>Todo Application</h1>
        <div className="todo-input">
          <input
            type="text"
            value={inputValue}
            onChange={(e) => setInputValue(e.target.value)}
            onKeyPress={(e) => e.key === 'Enter' && addTodo()}
            placeholder="What needs to be done?"
          />
          <button onClick={addTodo} disabled={!inputValue.trim()}>
            Add Todo
          </button>
        </div>
      </header>

      <main>
        {todos.length > 0 && (
          <div className="todo-filters">
            <button 
              className={filter === 'all' ? 'active' : ''}
              onClick={() => setFilter('all')}
            >
              All ({todos.length})
            </button>
            <button 
              className={filter === 'active' ? 'active' : ''}
              onClick={() => setFilter('active')}
            >
              Active ({activeCount})
            </button>
            <button 
              className={filter === 'completed' ? 'active' : ''}
              onClick={() => setFilter('completed')}
            >
              Completed ({completedCount})
            </button>
          </div>
        )}

        <div className="todo-list">
          {filteredTodos.length === 0 ? (
            <p className="empty-message">
              {filter === 'all' 
                ? 'No todos yet. Add one above!' 
                : `No ${filter} todos.`
              }
            </p>
          ) : (
            <ul>
              {filteredTodos.map(todo => (
                <TodoItem
                  key={todo.id}
                  todo={todo}
                  onToggle={toggleTodo}
                  onDelete={deleteTodo}
                />
              ))}
            </ul>
          )}
        </div>

        {completedCount > 0 && (
          <div className="todo-actions">
            <button onClick={clearCompleted}>
              Clear Completed ({completedCount})
            </button>
          </div>
        )}
      </main>

      <footer>
        <p>
          {activeCount === 0 
            ? "All tasks completed! 🎉" 
            : `${activeCount} task${activeCount === 1 ? '' : 's'} remaining`
          }
        </p>
      </footer>
    </div>
  );
};

const TodoItem = React.memo(({ todo, onToggle, onDelete }) => {
  const handleToggle = useCallback(() => {
    onToggle(todo.id);
  }, [todo.id, onToggle]);

  const handleDelete = useCallback(() => {
    onDelete(todo.id);
  }, [todo.id, onDelete]);

  return (
    <li className={`todo-item ${todo.completed ? 'completed' : ''}`}>
      <label>
        <input
          type="checkbox"
          checked={todo.completed}
          onChange={handleToggle}
        />
        <span className="todo-text">{todo.text}</span>
      </label>
      <button 
        className="delete-button"
        onClick={handleDelete}
        aria-label={`Delete ${todo.text}`}
      >
        ×
      </button>
    </li>
  );
});

export default TodoApp;
```

### Example 2: User Card with Conditional Rendering
```javascript
import React, { useState } from 'react';

const UserCard = ({ user, onEdit, onDelete, isEditable = true }) => {
  const [isExpanded, setIsExpanded] = useState(false);
  
  if (!user) {
    return (
      <div className="user-card skeleton">
        <div className="skeleton-avatar"></div>
        <div className