# React Task – Props, Conditional Rendering & Advanced List Rendering (Complex Rendering)

## 🎯 Goal
Build an enhanced **"Course Dashboard"** app in React that displays a list of courses, each with multiple enrolled students and their details. The app should demonstrate advanced usage of **props**, **conditional rendering**, and **nested list rendering** with a clear and intuitive UI.

---

## 📁 Project Structure & Naming Conventions

Follow this folder pattern and filename conventions for scalability and maintainability:

```
src/
│
├── components/
│   ├── CourseCard/
│   │   ├── CourseCard.jsx         // Main component for rendering a course
│   │   └── CourseCard.module.css  // CSS module for CourseCard styling
│   └── StudentRow/
│       ├── StudentRow.jsx         // Component for rendering a single student in the course
│       └── StudentRow.module.css  // CSS module for StudentRow styling
│
├── data/
│   └── courses.js                 // Sample data for courses and students
│
├── App.jsx                        // Entry point, renders the dashboard
└── index.js
```

- **components/**: Contains all UI components, each in its own folder with related CSS.
- **data/**: Contains mock data, like course and student lists.
- **App.jsx**: Main app file that brings everything together.

---

## 📋 Requirements

### 1. Props (with Nested Data)
- `CourseCard` receives:
  - `courseName` (string)
  - `instructor` (string)
  - `students` (array of objects, each with `name`, `score`, `attendance`)
- Each student is rendered using a `StudentRow` component.

### 2. Complex Conditional Rendering
- For each student in a course:
  - If `score >= 50` **and** `attendance >= 75`: Show **"Eligible ✅"** (green).
  - If `score >= 50` **but** `attendance < 75`: Show **"Low Attendance ⚠️"** (orange).
  - If `score < 50`: Show **"Not Eligible ❌"** (red), regardless of attendance.
- The top scorer in each course is highlighted with a star (⭐) next to their name.

### 3. Advanced List Rendering (Nested)
- Use `.map()` in the parent component to render `CourseCard` for each course.
- Within `CourseCard`, use `.map()` to render `StudentRow` for each student.

---

## 🎨 UI Design

### App Layout
- **Header:** "Course Dashboard" (centered, bold, large font).
- **Course Cards:** Displayed in a responsive grid or stacked vertically.
- **CourseCard Component:**
  - Card-style box with:
    - Course Name (large, bold)
    - Instructor Name (smaller, subtler)
    - Students Table:
      - Columns: Name, Score, Attendance (%), Status
      - Top scorer: Name with ⭐
      - Status: Color-coded as per eligibility
      - Clean borders & subtle background for rows

#### Example (Wireframe)

```
+-------------------------------------------------------+
|                 Course Dashboard                      |
+-------------------------------------------------------+
| +-------------------------+   +---------------------+ |
| | React Basics            |   | Advanced JS         | |
| | Instructor: Ms. Smith   |   | Instructor: Mr. Lee | |
| |-------------------------|   |---------------------| |
| | Name     | Score | Att. |   | Name  | Score | Att.|
| | ⭐ Alice  |  85   | 92%  |   | ⭐ David|  90   | 88%|
| | Bob      |  42   | 80%  |   | Eva   |  77   | 60% |
| | Charlie  |  67   | 70%  |   | Frank |  55   | 78% |
| |-------------------------|   |---------------------| |
| | Status:                 |   | Status:             | |
| | Alice: Eligible ✅       |   | David: Eligible ✅  | |
| | Bob: Not Eligible ❌     |   | Eva: Low Attend.⚠️ | |
| | Charlie: Low Attend.⚠️  |   | Frank: Eligible ✅  | |
| +-------------------------+   +---------------------+ |
+-------------------------------------------------------+
```

### Colors & Styles
- **Eligible:** Green text/badge
- **Low Attendance:** Orange text/badge
- **Not Eligible:** Red text/badge
- **Top Scorer:** Gold star emoji (⭐) beside name
- Use spacing, padding, and subtle shadows for card separation.
- Responsive: Stacks vertically on mobile.

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

- **Props:** Receives all course data (name, instructor, student array).
- **Top Scorer:** Find student(s) with max score and add ⭐ next to their name.
- **Student Table:** Each student rendered with name, score, attendance, and colored status.
- **Accessibility:** Use semantic HTML (`<table>`, `<thead>`, `<tbody>`) for the student list.
- **Styling:** Use CSS modules for scoped, maintainable styles.

---

## 🚀 Bonus

- Add search/filter for courses or students.
- Add sorting for student scores or attendance.
- Add a summary row (e.g., average score/attendance).

---

**This structure and UI will help you practice scalable patterns and clear, user-friendly React UI design while working with nested data and advanced rendering techniques.**