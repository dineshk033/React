# 📘 Forms Task – React + Bootstrap (Uncontrolled Forms)

## 🎯 Objective

Build a React form to **add a new student** to a given course, using **Bootstrap styling**. This task will help you understand how to handle **uncontrolled forms** in React.

---

## 🗂 Provided Data

You are given the following dataset:

```javascript
export const courses = [
  {
    courseName: "React Basics",
    instructor: "Ms. Smith",
    students: [
      { name: "Alice", score: 85, attendance: 92 },
      { name: "Bob", score: 42, attendance: 80 },
      { name: "Charlie", score: 67, attendance: 70 },
    ],
  },
  {
    courseName: "Advanced JS",
    instructor: "Mr. Lee",
    students: [
      { name: "David", score: 90, attendance: 88 },
      { name: "Eva", score: 77, attendance: 60 },
      { name: "Frank", score: 55, attendance: 78 },
    ],
  },
];
```

```javascript
import React, { useRef, useMemo } from "react";
import { courses } from "./courses";

export default function App() {
  return (
    <div className="container py-4">
      <h1 className="mb-4">Course Enrollment (Uncontrolled Form)</h1>
      <UncontrolledCourseForm courses={courses} />
    </div>
  );
}

function UncontrolledCourseForm({ courses }) {
  const formRef = useRef(null);

  // Precompute a simple map for instructors (courseName -> instructor)
  const instructorByCourse = useMemo(() => {
    const map = new Map();
    courses.forEach((c) => map.set(c.courseName, c.instructor));
    return map;
  }, [courses]);

  const handleSubmit = (e) => {
    e.preventDefault();
    const form = formRef.current;

    // trigger native HTML5 validation styles
    if (!form.checkValidity()) {
      form.classList.add("was-validated");
      return;
    }

    const data = Object.fromEntries(new FormData(form).entries());

    // Enrich with computed values (example: instructor by selected course)
    if (data.courseName && instructorByCourse.has(data.courseName)) {
      data.instructor = instructorByCourse.get(data.courseName);
    }

    // Convert numeric fields
    if (data.score) data.score = Number(data.score);
    if (data.attendance) data.attendance = Number(data.attendance);

    console.log("Submitted payload:", data);
    alert(`Submitted!\n\n${JSON.stringify(data, null, 2)}`);

    // Optional: reset the form and validation state
    form.reset();
    form.classList.remove("was-validated");
  };

  const handleReset = () => {
    const form = formRef.current;
    form.reset();
    form.classList.remove("was-validated");
  };

  return (
    <form
      ref={formRef}
      className="row g-3 needs-validation"
      noValidate
      onSubmit={handleSubmit}
    >
      {/* Course */}
      <div className="col-md-6">
        <label htmlFor="courseName" className="form-label">
          Course
        </label>
        <select
          className="form-select"
          id="courseName"
          name="courseName"
          required
          defaultValue=""
        >
          <option value="" disabled>
            Choose...
          </option>
          {courses.map((c) => (
            <option key={c.courseName} value={c.courseName}>
              {c.courseName}
            </option>
          ))}
        </select>
        <div className="invalid-feedback">Please select a course.</div>
      </div>

      {/* Student Name */}
      <div className="col-md-6">
        <label htmlFor="studentName" className="form-label">
          Student Name
        </label>
        <input
          type="text"
          className="form-control"
          id="studentName"
          name="studentName"
          placeholder="Enter student name"
          required
          minLength={2}
        />
        <div className="invalid-feedback">
          Please provide a valid student name (min 2 characters).
        </div>
      </div>

      {/* Score */}
      <div className="col-md-6">
        <label htmlFor="score" className="form-label">
          Score
        </label>
        <input
          type="number"
          className="form-control"
          id="score"
          name="score"
          placeholder="0 - 100"
          required
          min={0}
          max={100}
        />
        <div className="invalid-feedback">
          Please enter a score between 0 and 100.
        </div>
      </div>

      {/* Attendance */}
      <div className="col-md-6">
        <label htmlFor="attendance" className="form-label">
          Attendance (%)
        </label>
        <input
          type="number"
          className="form-control"
          id="attendance"
          name="attendance"
          placeholder="0 - 100"
          required
          min={0}
          max={100}
        />
        <div className="invalid-feedback">
          Please enter attendance between 0 and 100.
        </div>
      </div>

      {/* Notes (optional) */}
      <div className="col-12">
        <label htmlFor="notes" className="form-label">
          Notes (optional)
        </label>
        <textarea
          className="form-control"
          id="notes"
          name="notes"
          rows="3"
          placeholder="Any remarks…"
        />
      </div>

      {/* Terms */}
      <div className="col-12">
        <div className="form-check">
          <input
            className="form-check-input"
            type="checkbox"
            id="terms"
            name="terms"
            required
          />
          <label className="form-check-label" htmlFor="terms">
            I confirm the details are accurate.
          </label>
          <div className="invalid-feedback">
            You must confirm before submitting.
          </div>
        </div>
      </div>

      {/* Actions */}
      <div className="col-12 d-flex gap-2">
        <button type="submit" className="btn btn-primary">
          Submit
        </button>
        <button
          type="button"
          className="btn btn-secondary"
          onClick={handleReset}
        >
          Reset
        </button>
      </div>

      {/* Helper: shows selected instructor (computed on submit) */}
      <div className="form-text">
        Instructor will be inferred from the selected course on submit.
      </div>
    </form>
  );
}
```

---

## 📝 Task Requirements

1. **Form UI**

   - Use **Bootstrap form components** (`form-group`, `form-control`, `btn`, etc.).
   - Fields to include:
     - `Student Name` (text input)
     - `Score` (number input)
     - `Attendance` (number input)
     - `Course` (dropdown select with course names from `courses`)
   - Add a **Submit** button.

2. **controlled Form Handling**

   - Use `useState` to access form input values.
   - On form submit:
     - Read values from state.
     - Push the new student into the correct course’s `students` array.

3. **Validation**

   - If any input is empty, show an alert (`alert("All fields are required!")`).
   - `Score` should be between `0–100`.
   - `Attendance` should be between `0–100`.

4. **Display Updated Students**
   - After submission, display the updated student list under each course.
   - Use a **Bootstrap table** for student details (`name`, `score`, `attendance`).

---

## ✅ Example Behavior

1. User enters:
   - Name: `John`
   - Score: `78`
   - Attendance: `85`
   - Course: `React Basics`
2. On submit:
   - `John` is added to the `React Basics` course’s student list.
   - Student list updates in the UI.

---

## 📚 Bonus Task (Optional)

- Show **pass/fail status**:
  - Pass if `score >= 50 && attendance >= 75`.
  - Fail otherwise.
- Highlight failed students in **red** in the table.

---

## 🚀 Deliverables

- A working React app with:
  - Bootstrap-styled form
  - Uncontrolled input handling
  - Updated course student tables
- Submit your GitHub repo link.
