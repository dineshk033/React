# React Task – Props, Conditional Rendering & List Rendering

## 🎯 Goal
Build a simple **"Student Dashboard"** app in React that displays a list of students with their status (Pass / Fail) using **props**, **conditional rendering**, and **list rendering**.

---

## 📋 Requirements

### 1. Props
- Create a component `StudentCard` that accepts:
  - `name` (string) – Student's name
  - `score` (number) – Student's score
- Pass these props from a parent component.

### 2. Conditional Rendering
- If `score >= 50` → Show **"Pass ✅"** in green.
- If `score < 50` → Show **"Fail ❌"** in red.

### 3. List Rendering
- Create an array of student objects in the parent component.
- Use `.map()` to render **multiple** `StudentCard` components.

---

## 🧩 Example Data
```javascript
const students = [
  { name: "Alice", score: 85 },
  { name: "Bob", score: 42 },
  { name: "Charlie", score: 67 },
  { name: "David", score: 30 }
];
