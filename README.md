
# 📌 **Complete Guide to React Hooks**
---
React **Hooks** are functions that let you **use state and lifecycle features** in functional 
components. Introduced in React 16.8, they simplify component logic and enhance code reusability.

Here’s a breakdown of all React Hooks:
---


## **1️⃣ State & Lifecycle Hooks**  
These hooks manage **state** and **side effects** in functional components.

### **1. useState** – Manage Local State
Manages state within a functional component.
```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### **2. useEffect** – Handle Side Effects
Executes side effects (e.g., fetching data, updating the DOM, cleaning up resources).
```jsx
import { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Component mounted or count changed.");
    return () => console.log("Cleanup before next execution.");
  }, [count]); // Runs when `count` changes

  return <button onClick={() => setCount(count + 1)}>Click</button>;
}
```

---

### **3. useContext** – Consume Context Easily
Accesses a context without needing a Consumer.
```jsx
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function Component() {
  const theme = useContext(ThemeContext);
  return <p>Current theme: {theme}</p>;
}
```

---

## **2️⃣ Performance Optimization Hooks**  
These hooks help **optimize rendering and calculations**.

### **4. useMemo** – Memorize Expensive Computations
Memoizes a computed value to avoid unnecessary calculations.
```jsx
import { useMemo, useState } from "react";

function Example() {
  const [count, setCount] = useState(0);

  const computedValue = useMemo(() => {
    console.log("Running calculation...");
    return count * 2;
  }, [count]);

  return (
    <div>
      <p>Value: {computedValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### **5. useCallback** – Memorize Functions  
Memoizes functions to prevent unnecessary re-renders.
```jsx
import { useCallback, useState } from "react";

function Example() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount((c) => c + 1);
  }, []);

  return <button onClick={increment}>Click {count}</button>;
}
```

---

### **6. useRef** – Create Persistent References
Creates a persistent reference that does not trigger re-renders.
```jsx
import { useRef, useEffect } from "react";

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} />;
}
```

---

### **7. useLayoutEffect** – Run Before Screen Paint  
Similar to useEffect, but runs before the screen is painted.
```jsx
import { useState, useLayoutEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  useLayoutEffect(() => {
    console.log("Executed before painting.");
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>Click</button>;
}
```

---

## **3️⃣ Advanced Hooks**  
These hooks manage **complex state and behaviors**.

### **8. useReducer** – Manage Complex State
An alternative to useState for more complex state logic.
```jsx
import { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
}
```

---

### **9. useImperativeHandle** – Control Child Component Methods 
Allows exposing specific functions from a child component to the parent. 
```jsx
import { useRef, forwardRef, useImperativeHandle } from "react";

const Input = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
  }));

  const inputRef = useRef(null);

  return <input ref={inputRef} />;
});

function Form() {
  const inputRef = useRef(null);

  return (
    <div>
      <Input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </div>
  );
}
```

---

### **10. useId** – Generate Unique IDs (React 18+)
Generates unique IDs to be used in HTML elements.
```jsx
import { useId } from "react";

function Form() {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Name:</label>
      <input id={id} type="text" />
    </div>
  );
}
```

---

### **11. useTransition** – Handle Non-Blocking UI Updates (React 18+)
Indicates when an update is "smooth" and can be interrupted.
```jsx
import { useState, useTransition } from "react";

function HeavyList() {
  const [items, setItems] = useState([]);
  const [isPending, startTransition] = useTransition();

  const loadItems = () => {
    startTransition(() => {
      setItems(Array.from({ length: 20000 }, (_, i) => `Item ${i}`));
    });
  };

  return (
    <div>
      <button onClick={loadItems}>Load Items</button>
      {isPending ? <p>Loading...</p> : items.map((item) => <p key={item}>{item}</p>)}
    </div>
  );
}
```

---

### **12. useDeferredValue** – Defer State Updates (React 18+)
Delays value updates to prevent blocking.
```jsx
import { useState, useDeferredValue } from "react";

function Search() {
  const [query, setQuery] = useState("");
  const deferredQuery = useDeferredValue(query);

  return (
    <div>
      <input onChange={(e) => setQuery(e.target.value)} />
      <p>Showing results for: {deferredQuery}</p>
    </div>
  );
}
```

---

### **📌 Summary of React Hooks**
| Hook                | Purpose |
|---------------------|---------|
| **useState**        | Manage local state |
| **useEffect**       | Handle side effects |
| **useContext**      | Access React context |
| **useMemo**         | Optimize expensive calculations |
| **useCallback**     | Memorize functions |
| **useRef**          | Access DOM elements & store values without re-rendering |
| **useLayoutEffect** | Run effects before painting |
| **useReducer**      | Manage complex state logic |
| **useImperativeHandle** | Expose child component methods |
| **useId**           | Generate unique IDs (React 18) |
| **useTransition**   | Handle non-blocking UI updates (React 18) |
| **useDeferredValue** | Defer updates for smooth UI (React 18) |

---


