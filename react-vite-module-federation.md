
# ⚛️ React + Vite Module Federation (Micro-Frontend Guide)

This guide explains how to implement **Module Federation** using **React with Vite**.  
We’ll create a **Host** and a **Remote** application to demonstrate **Micro-Frontends**.

---

## 1️⃣ Why Module Federation?

In large organizations, different teams work on different parts of the UI (like Dashboard, Profile, Settings).  
Traditional **monolithic builds** cause problems:  
- 🚫 Large bundle sizes  
- 🚫 Tight coupling between teams  
- 🚫 Hard to update independent features  

✅ **Module Federation solves this** by allowing **multiple apps to share code at runtime**, enabling **Micro-Frontends**.

**Benefits:**
- Independent deployments per app (Host/Remote).  
- Teams can use different versions of React/Vite.  
- Faster builds (each app builds separately).  
- Share common components (e.g., Button library, Navbar).  

---

## 2️⃣ Setup: Installing Dependencies

We need **Vite + React** + **Module Federation plugin**.

```bash
# Host application
npm create vite@latest host-app --template react
cd host-app
npm install @originjs/vite-plugin-federation

# Remote application
npm create vite@latest remote-app --template react
cd remote-app
npm install @originjs/vite-plugin-federation
```

---

## 3️⃣ Remote Application Setup

The **Remote app** exposes components to the Host.

📄 `vite.config.ts`
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import federation from '@originjs/vite-plugin-federation'

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'remote_app',
      filename: 'remoteEntry.js',
      exposes: {
        './Header': './src/components/Header.jsx', // expose component
      },
      shared: ['react', 'react-dom'],
    }),
  ],
  build: {
    target: 'esnext',
    minify: false,
    cssCodeSplit: true,
  },
})
```

📄 `src/components/Header.jsx`
```tsx
export default function Header() {
  return <h2>🚀 Remote Header Component</h2>
}
```

Run the remote:
```bash
npm run dev -- --port=5001
```

---

## 4️⃣ Host Application Setup

The **Host app** consumes the Remote component.

📄 `vite.config.ts`
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import federation from '@originjs/vite-plugin-federation'

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'host_app',
      remotes: {
        remote_app: 'http://localhost:5001/assets/remoteEntry.js',
      },
      shared: ['react', 'react-dom'],
    }),
  ],
  build: {
    target: 'esnext',
    minify: false,
    cssCodeSplit: true,
  },
})
```

📄 `src/App.jsx`
```tsx
import React, { Suspense } from 'react'

// Load remote Header dynamically
const RemoteHeader = React.lazy(() => import('remote_app/Header'))

function App() {
  return (
    <div>
      <h1>🏠 Host Application</h1>

      <Suspense fallback={<div>Loading Remote...</div>}>
        <RemoteHeader />
      </Suspense>
    </div>
  )
}

export default App
```

Run the host:
```bash
npm run dev -- --port=5000
```

---

## 5️⃣ How It Works

- **Remote app** builds `remoteEntry.js` which exposes components.  
- **Host app** fetches `remoteEntry.js` at runtime.  
- Host renders Remote’s `Header` component as if it was local.  
- Both apps share **React** to avoid bundle duplication.  

---

## 6️⃣ Real-World Use Case

Imagine an **E-commerce Platform**:
- **Host App** → Main shell (Routing, Authentication).  
- **Remote 1** → Product Listing module.  
- **Remote 2** → Cart & Checkout.  
- **Remote 3** → User Profile.  

Each team owns & deploys their micro-frontend **independently** 🚀.  

---

## ✅ Summary (Architect’s View)

- **Module Federation** enables scalable **micro-frontend architecture**.  
- **Host app** → consumes remote components.  
- **Remote app** → exposes components.  
- **Shared deps** → React, ReactDOM to avoid duplication.  
- **Benefits** → Independent deployments, better scaling, faster builds.  

---

🚀 With Module Federation in Vite + React, you can build **independent, deployable, and scalable micro-frontends** with ease.  
