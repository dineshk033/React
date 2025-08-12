# Forms in React: Controlled vs Uncontrolled

Designing forms is a crucial part of web application architecture. Understanding the distinction between **controlled** and **uncontrolled** forms in React is essential for building robust, maintainable, and user-friendly UIs. This guide explains both approaches, provides example implementations, highlights common input fields, and offers checklists for architecting forms.

---

## 1. Controlled Forms

### **Definition**
A **controlled component** is a form element whose value is managed by React state. Every change in the input is handled by a React event and updates the component's state.

### **Characteristics**
- React state is the _single source of truth_.
- The form data is always available in the component's state.
- Enables validation, formatting, and conditional disabling in real-time.
- Easier to test and debug.

### **Example**

```jsx
import React, { useState } from 'react';

function ControlledForm() {
  const [form, setForm] = useState({
    name: '',
    email: '',
    agree: false,
    gender: '',
    color: '#000000',
    file: null,
  });

  function handleChange(e) {
    const { name, value, type, checked, files } = e.target;
    setForm(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked
        : type === 'file' ? files[0]
        : value
    }));
  }

  function handleSubmit(e) {
    e.preventDefault();
    console.log(form);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={form.name} onChange={handleChange} placeholder="Name" />
      <input name="email" value={form.email} onChange={handleChange} type="email" placeholder="Email" />
      <input name="agree" checked={form.agree} onChange={handleChange} type="checkbox" />
      <select name="gender" value={form.gender} onChange={handleChange}>
        <option value="">Select Gender</option>
        <option value="male">Male</option>
        <option value="female">Female</option>
      </select>
      <input name="color" value={form.color} onChange={handleChange} type="color" />
      <input name="file" onChange={handleChange} type="file" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### **Checklist for Controlled Forms**
- [ ] Each input has a value (or checked) bound to state.
- [ ] All changes handled by `onChange` to update state.
- [ ] Form state initialized with all relevant fields.
- [ ] Validation and formatting logic sits in React.
- [ ] Suitable for complex forms and instant feedback.

---

## 2. Uncontrolled Forms

### **Definition**
An **uncontrolled component** is a form element that maintains its own state internally. React interacts with it via a _ref_ to read its value when needed.

### **Characteristics**
- DOM is the source of truth.
- Easier to integrate with non-React code or legacy codebases.
- Less code for simple forms, but harder to validate or manipulate data as user types.

### **Example**

```jsx
import React, { useRef } from 'react';

function UncontrolledForm() {
  const nameRef = useRef(null);
  const emailRef = useRef(null);
  const agreeRef = useRef(null);
  const genderRef = useRef(null);
  const colorRef = useRef(null);
  const fileRef = useRef(null);

  function handleSubmit(e) {
    e.preventDefault();
    const data = {
      name: nameRef.current.value,
      email: emailRef.current.value,
      agree: agreeRef.current.checked,
      gender: genderRef.current.value,
      color: colorRef.current.value,
      file: fileRef.current.files[0],
    };
    console.log(data);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" ref={nameRef} placeholder="Name" />
      <input name="email" ref={emailRef} type="email" placeholder="Email" />
      <input name="agree" ref={agreeRef} type="checkbox" />
      <select name="gender" ref={genderRef}>
        <option value="">Select Gender</option>
        <option value="male">Male</option>
        <option value="female">Female</option>
      </select>
      <input name="color" ref={colorRef} type="color" defaultValue="#000000" />
      <input name="file" ref={fileRef} type="file" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### **Checklist for Uncontrolled Forms**
- [ ] Use `ref` to access the DOM node of each input.
- [ ] Read values on submit, not on every change.
- [ ] Use `defaultValue` and `defaultChecked` for initial values.
- [ ] Good for simple, non-reactive forms or integrating with third-party libraries.
- [ ] Harder to implement real-time validation.

---

## 3. Input Field Hints

| Field Type      | Controlled Prop   | Uncontrolled Prop  | Notes                 |
|-----------------|------------------|--------------------|-----------------------|
| Text Input      | `value`          | `defaultValue`     |                      |
| Checkbox        | `checked`        | `defaultChecked`   |                      |
| Radio Button    | `checked`        | `defaultChecked`   | `name` should match   |
| Select          | `value`          | `defaultValue`     |                      |
| File Input      | _read only via ref_ | _read only via ref_ | Always uncontrolled  |
| Color Input     | `value`          | `defaultValue`     |                      |
| Number Input    | `value`          | `defaultValue`     |                      |
| Date Input      | `value`          | `defaultValue`     |                      |

---

## 4. Architectural Rules and Best Practices

### Always Use Controlled Components When:
- You need real-time validation or conditional rendering.
- You want to reset, prefill, or manipulate form data.
- You need predictable, testable behavior.

### Use Uncontrolled Components When:
- Integrating with non-React code or libraries.
- Building simple forms with minimal interactivity.
- Performance is critical and you want to avoid re-renders on every input change.

### General Checklist & Rules
- [ ] **Accessibility**: Always use labels for inputs and ensure keyboard navigation.
- [ ] **Validation**: Prefer controlled components for complex validation.
- [ ] **Performance**: For very large forms, consider hybrid approaches.
- [ ] **File Inputs**: Always use refs (uncontrolled).
- [ ] **Form Reset**: Controlled forms require resetting state; uncontrolled forms can use `form.reset()`.
- [ ] **Testing**: Controlled forms are easier to test due to predictable state.

---

## 5. References

- [React Docs: Forms](https://react.dev/reference/react-dom/components/input)
- [React Docs: Controlled vs Uncontrolled](https://react.dev/learn/sharing-state-between-components#controlled-and-uncontrolled-components)

---

**Remember:**  
Controlled forms are ideal for most modern React applications, especially when handling complex, dynamic, or validated input. Uncontrolled forms can be useful for quick forms or when interoperating with external libraries.

---
