# React Testing Library with Jest — Vite React Setup Guide

A comprehensive guide to set up **React Testing Library** with **Jest** in a **Vite + React** project including complete code coverage configuration, examples and troubleshooting tips.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1: Create Vite React Project](#step-1-create-vite-react-project)
- [Step 2: Install Testing Dependencies](#step-2-install-testing-dependencies)
- [Step 3: Jest Configuration](#step-3-jest-configuration)
- [Step 4: Babel Configuration](#step-4-babel-configuration)
- [Step 5: Setup Test Environment](#step-5-setup-test-environment)
- [Step 6: Code Coverage Configuration](#step-6-code-coverage-configuration)
- [Step 7: Write Your First Test (example)](#step-7-write-your-first-test-example)
- [Step 8: Running Tests with Coverage](#step-8-running-tests-with-coverage)
- [Advanced Coverage Configuration](#advanced-coverage-configuration)
- [Common Testing Patterns](#common-testing-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

- Node.js (LTS recommended) and npm or yarn installed.
- Basic familiarity with React and Vite.
- This guide shows configuration for JavaScript projects. TypeScript notes are included where relevant.

---

## Step 1: Create Vite React Project

```bash
# Using npm
npm create vite@latest my-react-app -- --template react

# Using yarn
yarn create vite my-react-app --template react

cd my-react-app
npm install
# or
yarn install
```

---

## Step 2: Install Testing Dependencies

Install core testing libraries and tools:

```bash
# core testing libs
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event

# jest and jsdom environment
npm install --save-dev jest jest-environment-jsdom @types/jest

# babel for transforming code
npm install --save-dev babel-jest @babel/preset-env @babel/preset-react @babel/preset-typescript

# optional: use ts-jest if you prefer (for TS projects)
npm install --save-dev ts-jest

# optional: mock css modules
npm install --save-dev identity-obj-proxy
```

> Notes:
>
> - Vite's native testing tool is **Vitest**. If you specifically need Jest (e.g., for CI parity or team preference), the steps below configure Jest to work in a Vite React project.
> - If you're using TypeScript, either use `ts-jest` or `babel-jest` with `@babel/preset-typescript`.

---

## Step 3: Jest Configuration

Create a `jest.config.js` at the project root. The configuration below is a solid starting point:

```js
// jest.config.js
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["<rootDir>/src/setupTests.js"],
  moduleNameMapper: {
    ".(css|less|scss|sass)$": "identity-obj-proxy",
    ".(jpg|jpeg|png|gif|webp|svg)$": "<rootDir>/__mocks__/fileMock.js",
  },
  transform: {
    "^.+.(js|jsx|ts|tsx)$": "babel-jest",
  },
  collectCoverage: true,
  collectCoverageFrom: [
    "src/**/*.{js,jsx,ts,tsx}",
    "!src/main.jsx",
    "!src/main.tsx",
    "!src/**/setupTests.js",
    "!src/**/index.js",
  ],
  coverageDirectory: "coverage",
  coverageReporters: ["json", "lcov", "text", "clover"],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
  moduleFileExtensions: ["js", "jsx", "ts", "tsx", "json", "node"],
  testMatch: [
    "<rootDir>/src/**/__tests__/**/*.{js,jsx,ts,tsx}",
    "<rootDir>/src/**/*.{spec,test}.{js,jsx,ts,tsx}",
  ],
  transformIgnorePatterns: ["/node_modules/", ".pnp.[^/]+$"],
  testPathIgnorePatterns: ["/node_modules/", "/build/", "/dist/"],
};
```

---

## Step 4: Babel Configuration

Create `babel.config.js`:

```js
// babel.config.js
module.exports = {
  presets: [
    ["@babel/preset-env", { targets: { node: "current" } }],
    ["@babel/preset-react", { runtime: "automatic" }],
    "@babel/preset-typescript",
  ],
};
```

---

## Step 5: Setup Test Environment

Create `src/setupTests.js`:

```js
import "@testing-library/jest-dom/extend-expect";

if (typeof window !== "undefined" && !window.matchMedia) {
  window.matchMedia = () => ({
    matches: false,
    addListener: () => {},
    removeListener: () => {},
  });
}
```

```js
//eslint.config.js
import js from "@eslint/js";
import globals from "globals";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import { defineConfig, globalIgnores } from "eslint/config";

export default defineConfig([
  globalIgnores(["dist"]),
  {
    files: ["**/*.{js,jsx}"],
    extends: [
      js.configs.recommended,
      reactHooks.configs["recommended-latest"],
      reactRefresh.configs.vite,
    ],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
      parserOptions: {
        ecmaVersion: "latest",
        ecmaFeatures: { jsx: true },
        sourceType: "module",
      },
    },
    rules: {
      "no-unused-vars": ["error", { varsIgnorePattern: "^[A-Z_]" }],
    },
    env: {
      jest: true,
    },
  },
]);
```

Create `__mocks__/fileMock.js`:

```js
module.exports = "test-file-stub";
```

---

## Step 6: Code Coverage Configuration

Add scripts to `package.json`:

```json
"scripts": {
  "test": "jest",
  "test:watch": "jest --watch",
  "test:coverage": "jest --coverage --coverageProvider=v8"
}
```

---

## Step 7: Write Your First Test (example)

**Component: `src/components/Button.jsx`**

```jsx
import React from "react";

export default function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}
```

**Test: `src/components/Button.test.jsx`**

```jsx
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Button from "./Button";

describe("Button", () => {
  test("renders button and handles click", async () => {
    const user = userEvent.setup();
    const handleClick = jest.fn();

    render(<Button onClick={handleClick}>Click me</Button>);

    const btn = screen.getByRole("button", { name: /click me/i });
    await user.click(btn);

    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

---

## Step 8: Running Tests with Coverage

```bash
npm test
npm run test:coverage
npm run test:watch
```

### Create a Button Component with Bootstrap

Create `src/components/Button.jsx`:

```jsx
import React from "react";
import PropTypes from "prop-types";

const Button = ({
  children,
  onClick,
  disabled = false,
  variant = "primary",
  size = "md",
  type = "button",
  className = "",
}) => {
  // Bootstrap button classes
  const baseClasses = "btn";
  const variantClasses = `btn-${variant}`;
  const sizeClasses = size !== "md" ? `btn-${size}` : "";

  const buttonClasses = [baseClasses, variantClasses, sizeClasses, className]
    .filter(Boolean)
    .join(" ");

  const handleClick = (e) => {
    if (!disabled && onClick) {
      onClick(e);
    }
  };

  return (
    <button
      type={type}
      className={buttonClasses}
      onClick={handleClick}
      disabled={disabled}
      aria-disabled={disabled}
    >
      {children}
    </button>
  );
};

Button.propTypes = {
  children: PropTypes.node.isRequired,
  onClick: PropTypes.func,
  disabled: PropTypes.bool,
  variant: PropTypes.oneOf([
    "primary",
    "secondary",
    "success",
    "danger",
    "warning",
    "info",
    "light",
    "dark",
    "link",
  ]),
  size: PropTypes.oneOf(["sm", "md", "lg"]),
  type: PropTypes.oneOf(["button", "submit", "reset"]),
  className: PropTypes.string,
};

export default Button;
```

### Create Button Tests

Create your first test file `src/components/__tests__/Button.test.jsx`:

```jsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Button from "../Button";

// Test suite for Button component
describe("Button Component", () => {
  // Basic rendering tests
  describe("Rendering", () => {
    test("renders button with text", () => {
      render(<Button>Click me</Button>);
      expect(
        screen.getByRole("button", { name: /click me/i })
      ).toBeInTheDocument();
    });

    test("renders with correct default props and Bootstrap classes", () => {
      render(<Button>Default Button</Button>);
      const button = screen.getByRole("button");

      expect(button).toHaveAttribute("type", "button");
      expect(button).not.toBeDisabled();
      expect(button).toHaveClass("btn", "btn-primary"); // Bootstrap classes
    });
  });

  // Bootstrap variant testing
  describe("Bootstrap Variants", () => {
    test("applies correct Bootstrap variant classes", () => {
      const variants = [
        "primary",
        "secondary",
        "success",
        "danger",
        "warning",
        "info",
        "light",
        "dark",
        "link",
      ];

      variants.forEach((variant) => {
        const { rerender } = render(
          <Button variant={variant}>{variant}</Button>
        );
        expect(screen.getByRole("button")).toHaveClass(`btn-${variant}`);
        rerender(<div></div>); // Clear between tests
      });
    });

    test("applies primary variant by default", () => {
      render(<Button>Default Button</Button>);
      expect(screen.getByRole("button")).toHaveClass("btn-primary");
    });
  });

  // Bootstrap size testing
  describe("Bootstrap Sizes", () => {
    test("applies correct Bootstrap size classes", () => {
      const { rerender } = render(<Button size="sm">Small</Button>);
      expect(screen.getByRole("button")).toHaveClass("btn-sm");

      rerender(<Button size="md">Medium</Button>);
      const mediumButton = screen.getByRole("button");
      expect(mediumButton).toHaveClass("btn");
      expect(mediumButton).not.toHaveClass("btn-md"); // Bootstrap doesn't have btn-md

      rerender(<Button size="lg">Large</Button>);
      expect(screen.getByRole("button")).toHaveClass("btn-lg");
    });
  });

  // Custom className testing
  describe("Custom Classes", () => {
    test("applies additional custom classes", () => {
      render(<Button className="my-custom-class">Custom Button</Button>);
      const button = screen.getByRole("button");

      expect(button).toHaveClass("btn", "btn-primary", "my-custom-class");
    });

    test("combines Bootstrap classes with custom classes", () => {
      render(
        <Button variant="success" size="lg" className="me-3 shadow">
          Combined Classes
        </Button>
      );
      const button = screen.getByRole("button");

      expect(button).toHaveClass(
        "btn",
        "btn-success",
        "btn-lg",
        "me-3",
        "shadow"
      );
    });
  });

  // Props testing
  describe("Props", () => {
    test("sets correct button type", () => {
      const { rerender } = render(<Button type="submit">Submit</Button>);
      expect(screen.getByRole("button")).toHaveAttribute("type", "submit");

      rerender(<Button type="reset">Reset</Button>);
      expect(screen.getByRole("button")).toHaveAttribute("type", "reset");
    });
  });

  // Interaction tests
  describe("Interactions", () => {
    test("calls onClick handler when clicked", async () => {
      const handleClick = jest.fn();
      const user = userEvent.setup();

      render(<Button onClick={handleClick}>Click me</Button>);

      await user.click(screen.getByRole("button"));
      expect(handleClick).toHaveBeenCalledTimes(1);
    });

    test("passes event to onClick handler", async () => {
      const handleClick = jest.fn();
      const user = userEvent.setup();

      render(<Button onClick={handleClick}>Click me</Button>);

      await user.click(screen.getByRole("button"));
      expect(handleClick).toHaveBeenCalledWith(expect.any(Object));
    });

    test("does not call onClick when disabled", async () => {
      const handleClick = jest.fn();
      const user = userEvent.setup();

      render(
        <Button onClick={handleClick} disabled>
          Disabled Button
        </Button>
      );

      await user.click(screen.getByRole("button"));
      expect(handleClick).not.toHaveBeenCalled();
    });
  });

  // Bootstrap disabled state
  describe("Disabled State", () => {
    test("applies disabled attribute and aria-disabled", () => {
      render(<Button disabled>Disabled Button</Button>);
      const button = screen.getByRole("button");

      expect(button).toBeDisabled();
      expect(button).toHaveAttribute("aria-disabled", "true");
    });

    test("disabled button still has Bootstrap classes", () => {
      render(
        <Button disabled variant="danger">
          Disabled Danger
        </Button>
      );
      const button = screen.getByRole("button");

      expect(button).toHaveClass("btn", "btn-danger");
      expect(button).toBeDisabled();
    });
  });

  // Edge cases
  describe("Edge Cases", () => {
    test("handles click without onClick prop", async () => {
      const user = userEvent.setup();

      render(<Button>No Handler</Button>);

      // Should not throw error
      await user.click(screen.getByRole("button"));
      expect(screen.getByRole("button")).toBeInTheDocument();
    });

    test("handles empty className prop", () => {
      render(<Button className="">Empty Class</Button>);
      const button = screen.getByRole("button");

      expect(button).toHaveClass("btn", "btn-primary");
    });

    test("handles undefined className prop", () => {
      render(<Button className={undefined}>Undefined Class</Button>);
      const button = screen.getByRole("button");

      expect(button).toHaveClass("btn", "btn-primary");
    });
  });
});
```

## Step 8: Running Tests with Coverage

### Basic Test Commands

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Run tests with coverage in watch mode
npm run test:coverage:watch

# Run tests for CI (no watch, with coverage)
npm run test:ci
```

### Coverage Output Example

When you run `npm run test:coverage`, you'll see output like:

```
 PASS  src/components/__tests__/Button.test.jsx
  Button Component
    Rendering
      ✓ renders button with text (25 ms)
      ✓ renders with correct default props (8 ms)
    Props
      ✓ applies correct variant classes (12 ms)
      ✓ applies correct size classes (10 ms)
      ✓ sets correct button type (7 ms)
    Interactions
      ✓ calls onClick handler when clicked (15 ms)
      ✓ passes event to onClick handler (12 ms)
      ✓ does not call onClick when disabled (10 ms)
    Accessibility
      ✓ has correct aria attributes when disabled (8 ms)
      ✓ is focusable when enabled (7 ms)
      ✓ is not focusable when disabled (6 ms)
    Edge Cases
      ✓ handles click without onClick prop (9 ms)
      ✓ applies disabled styles correctly (6 ms)

------------|---------|----------|---------|---------|-------------------
File        | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
------------|---------|----------|---------|---------|-------------------
All files   |     100 |      100 |     100 |     100 |
 Button.jsx |     100 |      100 |     100 |     100 |
------------|---------|----------|---------|---------|-------------------

Test Suites: 1 passed, 1 total
Tests:       12 passed, 12 total
Snapshots:   0 total
Time:        2.56 s
```

## Advanced Coverage Configuration

### Coverage Reports Location

After running coverage, you'll find detailed reports in:

```
coverage/
├── lcov-report/          # HTML report (open index.html in browser)
│   ├── index.html
│   ├── base.css
│   └── ...
├── lcov.info            # LCOV format for CI/CD
├── clover.xml           # Clover XML format
└── coverage-final.json  # JSON format
```

### Viewing HTML Coverage Report

```bash
# After running coverage
npm run test:coverage

# Open HTML report (macOS)
open coverage/lcov-report/index.html

# Open HTML report (Windows)
start coverage/lcov-report/index.html

# Open HTML report (Linux)
xdg-open coverage/lcov-report/index.html
```

### Custom Coverage Scripts

Add these additional scripts to `package.json`:

```json
{
  "scripts": {
    "test:coverage:open": "npm run test:coverage && open coverage/lcov-report/index.html",
    "test:coverage:json": "jest --coverage --coverageReporters=json",
    "test:coverage:text": "jest --coverage --coverageReporters=text",
    "test:changed": "jest --onlyChanged",
    "test:related": "jest --findRelatedTests"
  }
}
```

## Common Testing Patterns

### Testing Forms with Coverage

```jsx
import React, { useState } from "react";

const ContactForm = ({ onSubmit }) => {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    message: "",
  });
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const validate = () => {
    const newErrors = {};
    if (!formData.name.trim()) newErrors.name = "Name is required";
    if (!formData.email.trim()) newErrors.email = "Email is required";
    if (!/^\S+@\S+\.\S+$/.test(formData.email))
      newErrors.email = "Email is invalid";
    if (!formData.message.trim()) newErrors.message = "Message is required";

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!validate()) return;

    setIsSubmitting(true);
    try {
      await onSubmit(formData);
      setFormData({ name: "", email: "", message: "" });
    } catch (error) {
      console.error("Submit error:", error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({ ...prev, [name]: value }));
    if (errors[name]) {
      setErrors((prev) => ({ ...prev, [name]: "" }));
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name:</label>
        <input
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
        />
        {errors.name && <span role="alert">{errors.name}</span>}
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input
          id="email"
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <span role="alert">{errors.email}</span>}
      </div>

      <div>
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
        />
        {errors.message && <span role="alert">{errors.message}</span>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting..." : "Submit"}
      </button>
    </form>
  );
};

export default ContactForm;
```

### Comprehensive Form Test with Coverage

```jsx
import React from "react";
import { render, screen, waitFor } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import ContactForm from "../ContactForm";

describe("ContactForm", () => {
  const mockSubmit = jest.fn();

  beforeEach(() => {
    mockSubmit.mockClear();
  });

  describe("Rendering", () => {
    test("renders all form fields", () => {
      render(<ContactForm onSubmit={mockSubmit} />);

      expect(screen.getByLabelText(/name/i)).toBeInTheDocument();
      expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
      expect(screen.getByLabelText(/message/i)).toBeInTheDocument();
      expect(
        screen.getByRole("button", { name: /submit/i })
      ).toBeInTheDocument();
    });
  });

  describe("Validation", () => {
    test("shows validation errors for empty fields", async () => {
      const user = userEvent.setup();
      render(<ContactForm onSubmit={mockSubmit} />);

      await user.click(screen.getByRole("button", { name: /submit/i }));

      expect(await screen.findByText("Name is required")).toBeInTheDocument();
      expect(await screen.findByText("Email is required")).toBeInTheDocument();
      expect(
        await screen.findByText("Message is required")
      ).toBeInTheDocument();
      expect(mockSubmit).not.toHaveBeenCalled();
    });

    test("shows error for invalid email", async () => {
      const user = userEvent.setup();
      render(<ContactForm onSubmit={mockSubmit} />);

      await user.type(screen.getByLabelText(/name/i), "John Doe");
      await user.type(screen.getByLabelText(/email/i), "invalid-email");
      await user.type(screen.getByLabelText(/message/i), "Hello world");
      await user.click(screen.getByRole("button", { name: /submit/i }));

      expect(await screen.findByText("Email is invalid")).toBeInTheDocument();
      expect(mockSubmit).not.toHaveBeenCalled();
    });

    test("clears errors when user starts typing", async () => {
      const user = userEvent.setup();
      render(<ContactForm onSubmit={mockSubmit} />);

      // Trigger validation errors
      await user.click(screen.getByRole("button", { name: /submit/i }));
      expect(await screen.findByText("Name is required")).toBeInTheDocument();

      // Start typing in name field
      await user.type(screen.getByLabelText(/name/i), "J");

      // Error should be cleared
      expect(screen.queryByText("Name is required")).not.toBeInTheDocument();
    });
  });

  describe("Form Submission", () => {
    test("submits form with valid data", async () => {
      const user = userEvent.setup();
      mockSubmit.mockResolvedValue();

      render(<ContactForm onSubmit={mockSubmit} />);

      await user.type(screen.getByLabelText(/name/i), "John Doe");
      await user.type(screen.getByLabelText(/email/i), "john@example.com");
      await user.type(screen.getByLabelText(/message/i), "Hello world");
      await user.click(screen.getByRole("button", { name: /submit/i }));

      expect(mockSubmit).toHaveBeenCalledWith({
        name: "John Doe",
        email: "john@example.com",
        message: "Hello world",
      });
    });

    test("shows loading state during submission", async () => {
      const user = userEvent.setup();
      mockSubmit.mockImplementation(
        () => new Promise((resolve) => setTimeout(resolve, 100))
      );

      render(<ContactForm onSubmit={mockSubmit} />);

      await user.type(screen.getByLabelText(/name/i), "John Doe");
      await user.type(screen.getByLabelText(/email/i), "john@example.com");
      await user.type(screen.getByLabelText(/message/i), "Hello world");
      await user.click(screen.getByRole("button", { name: /submit/i }));

      expect(screen.getByText("Submitting...")).toBeInTheDocument();
      expect(screen.getByRole("button")).toBeDisabled();

      await waitFor(() => {
        expect(screen.getByText("Submit")).toBeInTheDocument();
      });
    });

    test("resets form after successful submission", async () => {
      const user = userEvent.setup();
      mockSubmit.mockResolvedValue();

      render(<ContactForm onSubmit={mockSubmit} />);

      const nameInput = screen.getByLabelText(/name/i);
      const emailInput = screen.getByLabelText(/email/i);
      const messageInput = screen.getByLabelText(/message/i);

      await user.type(nameInput, "John Doe");
      await user.type(emailInput, "john@example.com");
      await user.type(messageInput, "Hello world");
      await user.click(screen.getByRole("button", { name: /submit/i }));

      await waitFor(() => {
        expect(nameInput).toHaveValue("");
        expect(emailInput).toHaveValue("");
        expect(messageInput).toHaveValue("");
      });
    });

    test("handles submission errors gracefully", async () => {
      const user = userEvent.setup();
      const consoleSpy = jest.spyOn(console, "error").mockImplementation();
      mockSubmit.mockRejectedValue(new Error("Submission failed"));

      render(<ContactForm onSubmit={mockSubmit} />);

      await user.type(screen.getByLabelText(/name/i), "John Doe");
      await user.type(screen.getByLabelText(/email/i), "john@example.com");
      await user.type(screen.getByLabelText(/message/i), "Hello world");
      await user.click(screen.getByRole("button", { name: /submit/i }));

      await waitFor(() => {
        expect(screen.getByText("Submit")).toBeInTheDocument();
      });

      expect(consoleSpy).toHaveBeenCalledWith(
        "Submit error:",
        expect.any(Error)
      );

      consoleSpy.mockRestore();
    });
  });
});
```

## Best Practices

### 1. Organize Tests by Functionality

```jsx
describe("ComponentName", () => {
  describe("Rendering", () => {
    // Basic rendering tests
  });

  describe("Props", () => {
    // Props-related tests
  });

  describe("Interactions", () => {
    // User interaction tests
  });

  describe("Accessibility", () => {
    // A11y tests
  });

  describe("Edge Cases", () => {
    // Error handling, edge cases
  });
});
```

### 2. Aim for High Coverage Quality

```jsx
// Good - Tests actual behavior
test("displays error when validation fails", async () => {
  const user = userEvent.setup();
  render(<LoginForm />);

  await user.click(screen.getByRole("button", { name: /login/i }));
  expect(screen.getByText(/email is required/i)).toBeInTheDocument();
});

// Avoid - Just testing implementation
test("calls validate function", () => {
  const spy = jest.spyOn(LoginForm.prototype, "validate");
  // This tests implementation, not behavior
});
```

### 3. Mock External Dependencies

```jsx
// Mock API calls
jest.mock("../services/api", () => ({
  fetchUserData: jest.fn(() => Promise.resolve({ id: 1, name: "John" })),
  updateUser: jest.fn(() => Promise.resolve({ success: true })),
}));

// Mock React Router
jest.mock("react-router-dom", () => ({
  ...jest.requireActual("react-router-dom"),
  useNavigate: () => jest.fn(),
  useParams: () => ({ id: "1" }),
}));
```

## Common Testing Patterns & Best Practices

- Use `getByRole`, `getByLabelText`, `getByText`.
- Prefer `userEvent` over `fireEvent`.
- Use `msw` for network mocking.
- Reset mocks between tests: `afterEach(() => jest.resetAllMocks())`.
- Keep tests focused and behavior-driven.

---

## Troubleshooting

- Transform issues: tweak `transformIgnorePatterns`.
- CSS/asset imports: ensure `moduleNameMapper` configured.
- TypeScript: use `ts-jest` or `babel-jest` + `@babel/preset-typescript`.
- Slow coverage: use `--coverageProvider=v8` on modern Node.

---

## Example package.json snippet

(See body of this README for a full example.)

---

If you'd like, I can also create a zip with sample files (component + test + jest config + babel config + mocks).
