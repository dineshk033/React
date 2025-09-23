
# 📘 Vite + React Build Configuration Guide

This document explains how to configure **Vite** for **React applications**, focusing on **environment variables, base URL, and build settings**.  
The goal is to understand *what to configure and why*, like an **architect designing a scalable build pipeline**.

---

## 1️⃣ Environment Variables

Vite uses `.env` files to manage environment variables.  
Files are automatically loaded based on **mode** (`development`, `production`, or custom).

### 📂 Common `.env` files
- `.env` → loaded in **all modes**
- `.env.development` → loaded only in **development**
- `.env.production` → loaded only in **production**

### 📝 Example
```bash
# .env.development
VITE_API_URL=http://localhost:5000/api
VITE_APP_TITLE=React App (Dev)

# .env.production
VITE_API_URL=https://api.myapp.com
VITE_APP_TITLE=React App (Prod)
```

⚠️ **Important:** Variables must start with `VITE_` to be exposed to your React code.

### ✅ Usage in React
```tsx
console.log(import.meta.env.VITE_API_URL);
console.log(import.meta.env.VITE_APP_TITLE);
```

---

## 2️⃣ Base URL Configuration

The `base` option defines the **public path** your app will be served from.  
This is critical when deploying to subfolders (e.g., GitHub Pages).

### 📄 `vite.config.ts`
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/my-app/', // base URL (default: '/')
})
```

### ✅ Example
- **Production URL:** `https://username.github.io/my-app/`
- **Base in config:** `base: '/my-app/'`

---

## 3️⃣ Path Aliases

Aliasing makes imports clean and maintainable.

### 📄 `vite.config.ts`
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})
```

### ✅ Usage in React
```tsx
import Button from '@/components/Button'
```

---

## 4️⃣ Build Configuration (Advanced)

The **`build`** section in `vite.config.ts` controls how your app is optimized for production.  
Here’s how to **tune performance, debugging, and deployment**.

### 📄 Example Config
```ts
export default defineConfig({
  build: {
    outDir: 'dist',              // output folder (default: dist)
    sourcemap: true,             // enable debugging in production
    minify: 'esbuild',           // 'terser' if you need fine-grained control
    chunkSizeWarningLimit: 600,  // increase size warning limit (default: 500kb)
    rollupOptions: {
      output: {
        manualChunks: {
          // separate vendor libraries into their own chunk
          react: ['react', 'react-dom'],
          utils: ['lodash', 'axios'],
        },
      },
    },
  },
})
```

---

### 🔹 Split Vendor Chunks

Large third-party libraries should be split from your main bundle to improve caching.  

✅ Example:
```ts
rollupOptions: {
  output: {
    manualChunks: {
      vendor: ['react', 'react-dom'], // React core in its own chunk
      charting: ['chart.js'],         // Charting libs separately
    },
  },
}
```

📌 **Why?**  
- Browser caches vendor chunks longer since they change less often.  
- Faster reloads for end-users when only your app code changes.  

---

### 🔹 Increase Size Limit

By default, Vite warns if a chunk > **500kb**.  
For real-world apps, especially with **UI libraries** (Material UI, Ant Design), you may need to raise this.

✅ Example:
```ts
build: {
  chunkSizeWarningLimit: 1000, // raise to 1MB
}
```

📌 **Why?**  
- Avoid false warnings during build.  
- Lets architects track **real** problematic bundle sizes instead of noise.  

---

### 🔹 Enable Source Maps

Enable **source maps** for debugging production issues.  

✅ Example:
```ts
build: {
  sourcemap: true,  // generates .map files
}
```

📌 **Why?**  
- Lets you trace production errors back to original TypeScript/JSX.  
- Tools like **Sentry** or **TrackJS** use these maps to show readable stack traces.  

---

### 🔹 Minification

By default, Vite uses **esbuild** (fast).  
For finer-grained optimization, you can use **Terser**.

✅ Example:
```ts
build: {
  minify: 'terser',
  terserOptions: {
    compress: {
      drop_console: true,   // remove console.log in production
      drop_debugger: true,  // remove debugger statements
    },
  },
}
```

📌 **Why?**  
- `esbuild` = fastest, best for most apps.  
- `terser` = slower but allows custom control.  

---

## 5️⃣ Deployment Considerations

### GitHub Pages
```ts
// vite.config.ts
export default defineConfig({
  base: '/my-app/', // repo name
})
```
Build & deploy:
```bash
npm run build
npm run preview
```

### Netlify / Vercel
No need to set `base`. Just run:
```bash
npm run build
```
Deploy `dist` folder.

---

## ✅ Summary (Architect’s View)

- **`.env` files** → manage environment-specific configs.  
- **`base`** → critical for hosting (subfolders, GitHub Pages).  
- **Aliases** → improve readability in large codebases.  
- **Build config** →  
  - **Split vendor chunks** → better caching  
  - **Increase size limit** → realistic bundle monitoring  
  - **Enable sourcemaps** → debug production safely  
  - **Minify** → smaller bundle, remove unused logs  
- **Deployment** → GitHub Pages vs Netlify/Vercel require different `base` setups.  

---

🚀 With these configurations, your **Vite + React build pipeline** will be **scalable, maintainable, and optimized for production debugging and deployment**.  
