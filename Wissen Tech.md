Here’s what I was asked 👇
⚛️ React.js Questions ➡️
- What is useRef Hook?
- What are the use cases of useRef other than accessing DOM elements?
- Explain the useEffect Hook.
- What is the cleanup function inside useEffect?
- Difference between localStorage and sessionStorage (with practical examples).

💡 JavaScript Concepts ➡️
- Explain Closures in JavaScript.
- What is the difference between Event Bubbling and Event Capturing?

🧠 JavaScript Coding Questions ➡️
1️⃣ Given the sentence "India is my country."
 Return the same sentence but with the first letter of each word capitalized.
 👉 Output: India Is My Country.

2️⃣ Print numbers from 1 to 10, with a 1-second delay between each print (e.g., 1… 2… 3…).

🧠React Coding Question ➡️
Create a small app with:
- Three buttons — Start, Stop, and Pause.
- A counter that starts from 0 and increases every second after clicking Start.
- Stop should reset the timer.
- Pause should pause and Resume should continue the count.

=======================================Solutions===================================
That’s a great and well-rounded interview question set 💪 — here’s a **complete and best-explained answer sheet** for all those topics with examples and short explanations that would **impress any interviewer**.

---

## ⚛️ **React.js Questions**

### 🧩 1. What is `useRef` Hook?

**Definition:**
`useRef` is a React Hook that returns a **mutable object** (`{ current: ... }`) that persists for the entire lifecycle of the component — **without causing re-renders** when it changes.

**Example (accessing DOM):**

```jsx
import React, { useRef, useEffect } from 'react';

function InputFocus() {
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus(); // Access the input DOM element
  }, []);

  return <input ref={inputRef} placeholder="Focus on load" />;
}
```

---

### 🧠 2. Use cases of `useRef` other than accessing DOM

✅ **Store previous values**

```jsx
const prevCount = useRef();
useEffect(() => {
  prevCount.current = count;
}, [count]);
```

✅ **Store timers / intervals**

```jsx
const timerRef = useRef();
timerRef.current = setInterval(() => setCount(c => c + 1), 1000);
```

✅ **Avoid re-renders by storing mutable values**

```jsx
const renderCount = useRef(0);
renderCount.current++;
```

✅ **Cache expensive values**

```jsx
const cache = useRef({});
```

---

### ⚙️ 3. Explain `useEffect` Hook

**Definition:**
`useEffect` lets you perform **side effects** (like data fetching, event listeners, DOM manipulation) in functional components.

**Example:**

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]);
```

* Runs after render.
* Runs again when dependencies (`count`) change.

---

### 🧹 4. What is the cleanup function inside `useEffect`?

**Definition:**
The cleanup function prevents memory leaks by cleaning up side effects when the component **unmounts** or **re-runs** the effect.

**Example:**

```jsx
useEffect(() => {
  const id = setInterval(() => console.log('Running...'), 1000);
  return () => clearInterval(id); // cleanup
}, []);
```

---

### 💾 5. Difference between `localStorage` and `sessionStorage`

| Feature  | localStorage                          | sessionStorage             |
| -------- | ------------------------------------- | -------------------------- |
| Lifespan | Persists even after browser close     | Cleared when tab is closed |
| Scope    | Shared across all tabs of same domain | Unique per tab             |
| Size     | ~5MB                                  | ~5MB                       |

**Example:**

```js
// localStorage
localStorage.setItem('user', 'Suhail');
console.log(localStorage.getItem('user')); // Suhail

// sessionStorage
sessionStorage.setItem('theme', 'dark');
console.log(sessionStorage.getItem('theme')); // dark
```

---

## 💡 **JavaScript Concepts**

### 🔒 1. Explain Closures in JavaScript

**Definition:**
A **closure** gives a function access to variables from its **outer scope** even after that outer function has finished executing.

**Example:**

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
```

✅ Here, `inner()` “remembers” `count` from `outer()` — that’s a closure.

---

### 🌀 2. Event Bubbling vs Event Capturing

| Phase         | Description                           | Example               |
| ------------- | ------------------------------------- | --------------------- |
| **Bubbling**  | Event travels from **child → parent** | Default phase         |
| **Capturing** | Event travels from **parent → child** | Set `useCapture=true` |

**Example:**

```html
<div id="parent">
  <button id="child">Click me</button>
</div>

<script>
document.getElementById('parent').addEventListener('click', () => console.log('Parent'), true);  // Capturing
document.getElementById('child').addEventListener('click', () => console.log('Child'));  // Bubbling
</script>
```

👉 Output order:
If capture = true → **Parent → Child**
If capture = false → **Child → Parent**

---

## 🧠 **JavaScript Coding Questions**

### ✅ 1️⃣ Capitalize first letter of each word

**Best (simple & clean) solution:**

```js
function capitalizeWords(str) {
  const words = str.split(' ');
  for (let i = 0; i < words.length; i++) {
    if (words[i]) {
      words[i] = words[i][0].toUpperCase() + words[i].slice(1);
    }
  }
  return words.join(' ');
}

console.log(capitalizeWords("India is my country.")); 
// Output: India Is My Country.
```

---

### ✅ 2️⃣ Print 1 to 10 with 1-second delay

**Modern async/await solution:**

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function printNumbers() {
  for (let i = 1; i <= 10; i++) {
    console.log(i);
    await sleep(1000);
  }
}

printNumbers();
```

---

## ⚛️ **React Coding Question**

### ⏱️ Timer App (Start / Stop / Pause / Resume)

**React functional component example:**

```jsx
import React, { useState, useRef } from 'react';

function TimerApp() {
  const [count, setCount] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const timerRef = useRef(null);

  const start = () => {
    if (!isRunning) {
      setIsRunning(true);
      timerRef.current = setInterval(() => {
        setCount(prev => prev + 1);
      }, 1000);
    }
  };

  const pause = () => {
    clearInterval(timerRef.current);
    setIsRunning(false);
  };

  const stop = () => {
    clearInterval(timerRef.current);
    setCount(0);
    setIsRunning(false);
  };

  return (
    <div style={{ textAlign: 'center' }}>
      <h2>Timer: {count}</h2>
      <button onClick={start}>Start</button>
      <button onClick={pause}>Pause</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}

export default TimerApp;
```

✅ **Explanation:**

* `useRef` stores the interval ID.
* `setInterval` increases count every second.
* `clearInterval` stops or pauses the timer.
* State (`count` and `isRunning`) ensures controlled re-rendering.

---



