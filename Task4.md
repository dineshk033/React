# React Task – Props, State (with OnClick), Conditional Rendering & Advanced List Rendering

## 🎯 Goal
Build an enhanced **"Course Dashboard"** app in React that displays a list of courses, each with multiple enrolled students and their details. The app demonstrates advanced usage of **props**, **state** (with interactive onClick events instead of forms), **conditional rendering**, and **nested list rendering** with a clean UI.

---

## 📁 Project Structure & Naming Conventions

```
src/
│
├── components/
│   ├── CourseCard/
│   │   ├── CourseCard.jsx
│   │   └── CourseCard.module.css
│   └── StudentRow/
│       ├── StudentRow.jsx
│       └── StudentRow.module.css
│
├── data/
│   └── courses.js
│
├── App.jsx
└── index.js
```

---

## 📋 Requirements

### 1. Props (with Nested Data)
- `CourseCard` receives:
  - `courseName` (string)
  - `instructor` (string)
  - `students` (array of objects, each with `name`, `score`, `attendance`)
- Each student is rendered using a `StudentRow` component.

### 2. State (OnClick-Driven Interactivity)
All stateful interactions should be handled through buttons or clickable UI elements—**no input fields or forms**.

#### A. Filter Students by Status (with OnClick)
- Show a set of filter buttons (e.g., "All", "Eligible", "Low Attendance", "Not Eligible") at the top of each `CourseCard`.
- When a button is clicked, update the state to show only students matching that status.
- Highlight the active filter button.

#### B. Toggle Course Details (with OnClick)
- Add a button (e.g., "Show Students" / "Hide Students") on each `CourseCard` to collapse or expand the student list.
- Clicking the button toggles the expanded/collapsed state.

#### C. Highlight Student Row (with OnClick)
- Clicking on a student row toggles a "highlighted" effect for that row (e.g., background color or border).
- Only one student can be highlighted at a time per course.

---

## 🎨 UI Design

### App Layout
- **Header:** "Course Dashboard" (centered, bold, large font).
- **Course Cards:** Responsive, displayed in a grid/stack.
- **CourseCard:**
  - Course Name & Instructor
  - Filter Buttons (row, clearly styled, one active)
  - Toggle Button ("Show Students"/"Hide Students")
  - When expanded, shows a table/list of students:
    - Columns: Name (⭐ for top scorer), Score, Attendance (%), Status
    - Rows: Clickable for highlighting
    - Status: Color-coded (see below)

#### Status Colors
- **Eligible:** Green
- **Low Attendance:** Orange
- **Not Eligible:** Red

#### Example UI (Wireframe)

```
+-------------------------------------------------------+
|                 Course Dashboard                      |
+-------------------------------------------------------+
| +-------------------------+   +---------------------+ |
| | React Basics            |   | Advanced JS         | |
| | Instructor: Ms. Smith   |   | Instructor: Mr. Lee | |
| | [All] [Eligible] [Low Attendance] [Not Eligible]   |
| | [Show Students]                                   |
| |-------------------------|   |---------------------| |
| | Name    | Score | Att. |   | ...                 |
| | ⭐Alice  |  85   | 92%  |   | ...                 |
| | Bob     |  42   | 80%  |   | ...                 |
| | ...     | ...   | ...  |   | ...                 |
| +-------------------------+   +---------------------+ |
+-------------------------------------------------------+
```
- Clicking a filter button updates the list.
- Clicking "Show Students"/"Hide Students" toggles visibility.
- Clicking a row applies a highlight.

---

## 🧩 Example Data

```javascript
// src/data/courses.js
export const courses = [
  {
    courseName: "React Basics",
    instructor: "Ms. Smith",
    students: [
      { name: "Alice", score: 85, attendance: 92 },
      { name: "Bob", score: 42, attendance: 80 },
      { name: "Charlie", score: 67, attendance: 70 }
    ]
  },
  {
    courseName: "Advanced JS",
    instructor: "Mr. Lee",
    students: [
      { name: "David", score: 90, attendance: 88 },
      { name: "Eva", score: 77, attendance: 60 },
      { name: "Frank", score: 55, attendance: 78 }
    ]
  }
];
```

---

## 💡 Design Details for `CourseCard`

- **Props:** Receives course data.
- **Top Scorer:** Find the highest score; add ⭐ next to that student’s name.
- **Student Table:** Name, score, attendance, status. Row is clickable for highlight.
- **State:** 
  - Current filter (All, Eligible, etc.)
  - Expand/collapse state
  - Highlighted student index
- **No input fields or forms:** All interactions via buttons or clicking on rows.
- **Accessibility:** Use semantic HTML for tables/lists.

---

## 🚀 Bonus (Optional)
- Animate the expand/collapse of the student list.
- Add a "Reset Highlight" button.
- Add a "Show only top scorer" button/filter.

---

**This structure and UI will help you master React state, props, onClick-driven interactivity, advanced rendering, and clean design—without using forms or input fields.**