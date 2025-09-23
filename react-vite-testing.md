
# 🧪 Testing a Vite + React App — Step‑by‑Step with React Testing Library & Vitest

This guide shows a **practical, step-by-step** way to add **React Testing Library** to a **Vite + React** project, run tests with **Vitest**, and collect **code coverage**. It includes example components, test files, MSW (for network mocking) setup, `vitest` configuration, useful npm scripts, and a sample CI workflow.

---

## 🚦 Prerequisites

- Node.js (LTS) installed (Node 16+ recommended; Node 18+ preferred).
- A Vite + React project already created (JS or TS). If not:
```bash
# JavaScript
npm create vite@latest my-app --template react

# TypeScript
npm create vite@latest my-app --template react-ts
```

---

## 1️⃣ Install dev dependencies

We will use:
- **Vitest** — test runner tailored for Vite.
- **@testing-library/react** — queries and utilities for testing React components.
- **@testing-library/user-event** — realistic user-event helpers.
- **@testing-library/jest-dom** — jest-dom assertions (works with Vitest).
- **jsdom** — DOM environment (Vitest will use it when `environment: "jsdom"`).
- **msw** — Mock Service Worker for API mocking in tests.
- Optional: **c8** if you want the coverage provider explicitly.

```bash
cd my-app
npm install -D vitest @testing-library/react @testing-library/user-event @testing-library/jest-dom msw
# optional (if you want to pin coverage tool)
npm install -D c8
```

If you use Yarn:
```bash
yarn add -D vitest @testing-library/react @testing-library/user-event @testing-library/jest-dom msw
```

For TypeScript projects, also install types if needed (often not required because many packages ship types):
```bash
npm install -D @types/node
```

---

## 2️⃣ Configure Vitest

Create a `vitest.config.ts` at the project root (next to `vite.config.ts`), or update it if you already have one.

```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,                 // use global test APIs (describe, it, expect)
    environment: 'jsdom',          // browser-like environment
    setupFiles: './src/setupTests.ts', // run once before the test suite
    include: ['src/**/*.test.{js,ts,jsx,tsx}'], // file patterns to include
    coverage: {
      provider: 'c8',              // provider: 'c8' or 'istanbul'
      reporter: ['text', 'lcov', 'html'],
      reportsDirectory: 'coverage',
      all: true,                   // include all files, not just those touched by tests
      include: ['src/**/*.{ts,tsx,js,jsx}'],
      exclude: ['src/main.*', 'src/vitest-setup.*', 'src/mocks/**'],
    },
  },
})
```

**Notes**
- `globals: true` lets you write `describe`/`it`/`expect` without importing them.
- `setupFiles` is where we put test setup code (e.g., `@testing-library/jest-dom` and MSW server).
- `coverage.all: true` makes sure untouched files are also considered in coverage reports (helpful to track total coverage).

---

## 3️⃣ Add npm scripts

Update `package.json` scripts:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:watch": "vitest --watch",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

- `npm run test` — launches the Vitest interactive runner.
- `npm run test:watch` — watch mode, re-runs tests on file changes.
- `npm run test:run` — run tests once (CI friendly).
- `npm run test:coverage` — run tests and output coverage reports.

---

## 4️⃣ Test setup file

Create `src/setupTests.ts`. This file runs once before tests and is ideal for global config.

```ts
// src/setupTests.ts
import '@testing-library/jest-dom'    // adds custom matchers like toBeInTheDocument()
import 'whatwg-fetch'                // polyfill fetch if needed (optional)

// If you use MSW for API mocking, start the server here:
import { server } from './mocks/server' // see MSW section

// Establish API mocking before all tests.
beforeAll(() => server.listen({ onUnhandledRequest: 'warn' }))
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```

If you **don't** use MSW, keep at least `import '@testing-library/jest-dom'`.

---

## 5️⃣ Example 1 — Simple component + test

**Component**: `src/components/Counter.tsx`
```tsx
import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)
  return (
    <div>
      <span>Count: {count}</span>
      <button onClick={() => setCount((c) => c + 1)}>Inc</button>
    </div>
  )
}
```

**Test**: `src/components/Counter.test.tsx`
```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import Counter from './Counter'

test('increments counter on click', async () => {
  render(<Counter />)

  const btn = screen.getByRole('button', { name: /inc/i })
  const label = screen.getByText(/count:/i)

  await userEvent.click(btn)
  expect(label).toHaveTextContent('Count: 1')
})
```

Run:
```bash
npm run test
# or single run
npm run test:run
```

---

## 6️⃣ Example 2 — Component that fetches data (use MSW to mock API)

**Component**: `src/components/FetchMessage.tsx`
```tsx
import { useEffect, useState } from 'react'

export default function FetchMessage() {
  const [message, setMessage] = useState<string | null>(null)
  useEffect(() => {
    fetch('/api/message')
      .then((r) => r.json())
      .then((data) => setMessage(data.message))
  }, [])
  if (!message) return <div>Loading...</div>
  return <div role="status">Message: {message}</div>
}
```

**MSW handlers**: `src/mocks/handlers.ts`
```ts
import { rest } from 'msw'

export const handlers = [
  rest.get('/api/message', (req, res, ctx) => {
    return res(ctx.json({ message: 'Hello from MSW' }))
  }),
]
```

**MSW server**: `src/mocks/server.ts`
```ts
import { setupServer } from 'msw/node'
import { handlers } from './handlers'

export const server = setupServer(...handlers)
```

**Test**: `src/components/FetchMessage.test.tsx`
```tsx
import { render, screen, waitFor } from '@testing-library/react'
import FetchMessage from './FetchMessage'

test('shows message from API', async () => {
  render(<FetchMessage />)
  expect(screen.getByText(/loading/i)).toBeInTheDocument()

  // wait for the mocked API response to be rendered
  await waitFor(() => expect(screen.getByRole('status')).toHaveTextContent('Hello from MSW'))
})
```

MSW ensures your tests never hit the real network and lets you simulate success/failure scenarios easily.

---

## 7️⃣ Coverage reports

Run coverage:
```bash
npm run test:coverage
```

Outputs:
- Terminal summary (text)
- `coverage/lcov-report/index.html` — open in the browser for a beautiful HTML report
- `coverage/` directory with reports (lcov, html, text)

If you want to **fail CI** when coverage drops below thresholds, you can post-process `lcov` with tools like `coveralls` or write a small script to check `.coverage-summary`. Vitest itself does not enforce thresholds out-of-the-box in every version; for strict enforcement add a check script or use a coverage action.

---

## 8️⃣ Editor / Debugging tips

- In VS Code install **Vitest** extension or use the built‑in testing UI to run tests.
- Use `vitest --watch` during development to get fast feedback.
- To debug a failing test, run `vitest --run --reporter verbose` or use breakpoints with the Node debugger.

---

## 9️⃣ CI example (GitHub Actions)

`.github/workflows/ci-test.yml`
```yaml
name: CI - Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run test:run # run tests once
      - run: npm run test:coverage # produce coverage artifacts
      - uses: actions/upload-artifact@v3
        with:
          name: coverage-report
          path: coverage
```

Optionally upload `coverage/lcov-info` to **Codecov** or **Coveralls** using their GitHub Actions to see PR coverage comments.

---

## ✅ Best Practices & Tips (Architect's view)

- **Test user flows** (render → interact → assert) rather than implementation details.
- **Keep tests small and deterministic** — avoid flaky network/time-based tests (use MSW/fake timers).
- **Prefer accessibility queries** (`getByRole`, `getByLabelText`) — they encourage accessible UIs.
- **Group tests by feature** and colocate tests next to components (e.g., `Counter.test.tsx` next to `Counter.tsx`).
- **Mock external services** with MSW (fast, realistic, and safe).
- **Run tests in CI** on every PR to catch regressions early.
- **Use coverage reports to guide** which areas need tests; don’t chase 100% just for the number — focus on important logic paths.
- **Avoid snapshot overuse**; prefer meaningful assertions.

---

## 📁 Example folder structure

```
my-app/
├─ src/
│  ├─ components/
│  │  ├─ Counter.tsx
│  │  ├─ Counter.test.tsx
│  │  ├─ FetchMessage.tsx
│  │  └─ FetchMessage.test.tsx
│  ├─ mocks/
│  │  ├─ handlers.ts
│  │  └─ server.ts
│  ├─ setupTests.ts
│  └─ main.tsx
├─ vitest.config.ts
├─ vite.config.ts
├─ package.json
└─ tsconfig.json
```

---

## 🙋 Common gotchas

- If tests fail with `ReferenceError: window is not defined`, make sure `environment: 'jsdom'` is set in `vitest.config.ts`.
- If you rely on `import.meta.env` values, set them in tests or mock `import.meta.env` if needed.
- If coverage looks too low, confirm `coverage.include` covers the files you expect and that `coverage.all` is set if you want untouched files counted.
- For TypeScript projects, add `"types": ["vitest/globals", "vite/client"]` to `tsconfig.json` to avoid missing types for `describe`/`it`.

---

## ✅ Quick checklist (short)

1. `npm install -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event msw`
2. Create `vitest.config.ts` (jsdom, setupFiles, coverage).
3. Create `src/setupTests.ts` and import `@testing-library/jest-dom` and start MSW server.
4. Write tests alongside components (`*.test.tsx`).
5. Run `npm run test:watch` for dev, `npm run test:coverage` to generate coverage.
6. Add CI workflow to run tests and upload coverage.

---

If you want, I can:
- generate **this file** as a downloadable Markdown for you, and
- optionally include a **ready-to-run example repo** (zipped) with all files wired up so juniors can clone & run immediately.

Which would you prefer?