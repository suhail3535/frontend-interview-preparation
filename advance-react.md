
# ⚛️ React Interview Handbook

A consolidated resource for common React interview questions and essential framework concepts, structured for quick reference.

-----

## ❓ Interview Questions

### Core Concepts & Reconciliation

**1. How does React's reconciliation algorithm work?**

  * **Answer:** React uses the **reconciliation algorithm** (or **diffing algorithm**) to efficiently update the UI. It compares the current **Virtual DOM** tree with the previous one, following simple heuristics to find the minimal changes, and applies only those updates to the **real DOM**, ensuring high performance.

**2. What are React Fiber and how does it improve performance?**

  * **Answer:** **React Fiber** is the new reconciliation engine in React (v16+). It enhances performance by breaking the rendering process into **units of work**. This allows React to **pause, abort, or resume work** on the UI thread, making rendering **incremental and asynchronous**. This prevents long-running rendering tasks from blocking the main thread, thus improving the responsiveness.

-----

### Hooks and Memoization

**3. What are hooks in React, and how do they differ from class component lifecycle methods?**

  * **Answer:** **Hooks** are functions that allow functional components to "hook into" React features like state and lifecycle events. They are superior to class **lifecycle methods** (`componentDidMount`, etc.) as they allow related stateful logic and side effects to be written in a more cohesive and reusable way within functional components.
  * **Syntax Example:**

<!-- end list -->

```javascript
// Hook
const [count, setCount] = useState(0);

// Class Lifecycle Method Equivalent
componentDidMount() {
  // logic
}
```

**4. What is the difference between `useMemo` and `useCallback` hooks?**

  * **Answer:**
      * **`useMemo`**: **Caches the result of a computation** (a value). It recomputes the value only when its dependencies change, avoiding expensive calculations.
      * **`useCallback`**: **Caches the function definition itself**. It only recreates the function instance when its dependencies change, preventing unnecessary prop updates to memoized children.
  * **Syntax Example:**

<!-- end list -->

```javascript
// useMemo: Caching a value
const expensiveValue = useMemo(() => compute(a, b), [a, b]);

// useCallback: Caching a function
const memoizedHandler = useCallback(() => { /* logic */ }, [dependency]);
```

**5. What is the difference between `useState` and `useReducer`?**

  * **Answer:**
      * **`useState`**: Best for managing **simple state** where the update logic is straightforward (e.g., boolean toggles).
      * **`useReducer`**: Ideal for managing **complex state logic** with multiple, complex transitions. It is an alternative to `useState` for state that involves complicated logic or multiple sub-values.
  * **Syntax Example:**

<!-- end list -->

```javascript
// useState
const [count, setCount] = useState(0);

// useReducer
const [state, dispatch] = useReducer(reducer, initialState);
```

**6. Explain the purpose of the `useEffect` hook.**

  * **Answer:** The **`useEffect`** hook is used to handle **side effects** in functional components (e.g., data fetching, setting up subscriptions, manually changing the DOM). Its execution is controlled by a dependency array:
      * `[]`: Runs once after the initial render.
      * `[deps]`: Runs after initial render and when dependencies change.
      * No array: Runs after every render.
  * **Syntax Example:**

<!-- end list -->

```javascript
useEffect(() => {
  // Side effect logic (e.g., API call)
  document.title = `You clicked ${count} times`;

  return () => {
    // Cleanup function (e.g., unsubscribe)
  };
}, [count]); // Dependencies
```

**7. What is the `useLayoutEffect` hook and how is it different from `useEffect`?**

  * **Answer:** **`useLayoutEffect`** executes **synchronously** immediately after all DOM mutations are calculated and *before* the browser paints. It's used when you need to read layout from the DOM and synchronously trigger a re-render. **`useEffect`** runs **asynchronously** after the render and paint cycle, making it non-blocking and suitable for most side effects.
  * **Syntax Example:**

<!-- end list -->

```javascript
useLayoutEffect(() => {
  // Logic to measure/manipulate DOM before paint
  const height = ref.current.offsetHeight;
  // ... synchronously update state
}, [ref.current]);
```

**8. What is the difference between `useEffect` and `useCallback`?**

  * **Answer:**
      * **`useEffect`**: Used for executing **side effects** (like data fetching) that happen *after* the component renders.
      * **`useCallback`**: Used for **memoization** of a function instance to prevent it from being unnecessarily recreated *during* rendering.

-----

### Patterns & Utilities

**9. How does React Context work, and what are the performance pitfalls?**

  * **Answer:** **React Context** allows for sharing values (like themes or auth status) across the component tree without manually passing props (**prop drilling**). The pitfall is that if the context value object changes, **all components consuming that context will re-render**, even if they only use an unchanged part of the value. Memoizing the context value with `useMemo` is crucial to mitigate this.

**10. What are error boundaries in React?**

  * **Answer:** **Error Boundaries** are class components that use the lifecycle methods `static getDerivedStateFromError()` or `componentDidCatch()` to **catch JavaScript errors** anywhere in their child component tree, log those errors, and display a **fallback UI** instead of crashing the app.
  * **Syntax Example:**

<!-- end list -->

```javascript
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Log error to an external service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>; // Fallback UI
    }
    return this.props.children;
  }
}
```

**11. What are PureComponent and React.memo?**

  * **Answer:**
      * **`PureComponent`**: A class component base that implements `shouldComponentUpdate` with a **shallow comparison** of props and state to prevent unnecessary re-renders.
      * **`React.memo`**: A **Higher-Order Component (HOC)** for **functional components** that achieves the same shallow prop comparison to prevent unnecessary re-renders.
  * **Syntax Example:**

<!-- end list -->

```javascript
// React.memo (Functional Component)
const MyMemoizedComp = React.memo((props) => { /* render */ });

// PureComponent (Class Component)
class MyPureComp extends React.PureComponent { /* render */ }
```

**12. How would you optimize the performance of a React application?**

  * **Answer:** Optimizations include:
      * Use **`React.memo`** and **`PureComponent`**.
      * Use **`useMemo`** and **`useCallback`** for memoization.
      * Use **Code Splitting** with `React.lazy` and `Suspense`.
      * Use **Virtualized Lists** (`react-window`) for large data sets.
      * Minimize inline functions and state updates.

**13. What are controlled and uncontrolled components?**

  * **Answer:**
      * **Controlled components**: Form elements whose state is fully managed by **React state**.
      * **Uncontrolled components**: Form elements where the data is managed by the **DOM itself**, accessed via a `ref`.
  * **Syntax Example (Controlled):**

<!-- end list -->

```javascript
<input value={name} onChange={(e) => setName(e.target.value)} />
```

**14. What are React Portals?**

  * **Answer:** **React Portals** provide a way to render a component's children into a DOM node that exists **outside the DOM hierarchy** of the parent component. They are useful for elements like **modals** that need to visually break out of a parent's styling/layout.
  * **Syntax Example:**

<!-- end list -->

```javascript
// Creates a Portal, rendering children into a specified DOM node
ReactDOM.createPortal(
  <div>My Modal Content</div>,
  document.getElementById('modal-root')
);
```

**15. What is the purpose of React.StrictMode?**

  * **Answer:** **`React.StrictMode`** is a development-only tool that wraps a component tree to activate extra checks and warnings for descendants, highlighting potential problems like unsafe lifecycle methods or unexpected side effects.
  * **Syntax Example:**

<!-- end list -->

```javascript
<React.StrictMode>
  <App />
</React.StrictMode>
```

**16. What is a Higher Order Component (HOC)?**

  * **Answer:** A **Higher-Order Component (HOC)** is a function that takes a component as an argument and returns a new component with enhanced props or logic. It's a pattern for **reusing component logic** (e.g., `withRouter`).
  * **Syntax Example:**

<!-- end list -->

```javascript
// HOC Definition
const withLogger = (WrappedComponent) => {
  return (props) => {
    console.log('Rendering', WrappedComponent.name);
    return <WrappedComponent {...props} />;
  };
};

// Usage
const LoggedComponent = withLogger(MyComponent);
```

**17. What are Render Props in React?**

  * **Answer:** **Render Props** is a technique for **sharing code and logic** where a component receives a function as a prop (often named `render` or `children`) and calls that function with its internal state or logic, allowing the consumer to determine *what* is rendered.
  * **Syntax Example:**

<!-- end list -->

```javascript
// Component using Render Prop
const DataFetcher = ({ render }) => {
  const data = useData(); // internal logic
  return render(data); // Calls the prop function
};

// Usage
<DataFetcher render={(data) => (
  <h1>Data: {data.count}</h1>
)} />
```

**18. Explain the concept of code splitting in React.**

  * **Answer:** **Code Splitting** is the practice of dividing your JavaScript bundle into smaller chunks that can be loaded on demand to improve initial load time. This is implemented in React using **`React.lazy`** (for dynamic component imports) and **`Suspense`** (for the loading fallback UI).
  * **Syntax Example:**

<!-- end list -->

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <OtherComponent />
    </Suspense>
  );
}
```

-----

## 📚 Deep Dive: Supplementary Concepts

### JSX (JavaScript XML)

**JSX** is a syntax extension for JavaScript that allows writing HTML-like code within JavaScript.

  * **Key points:**
      * It is **not HTML**; it's syntactic sugar for `React.createElement()`.
      * It's compiled to regular JavaScript by tools like **Babel**.
      * It combines UI structure with JavaScript logic.

### Components

Components let you split the UI into independent, reusable pieces.

#### Function component

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

#### Class Component

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### StateLess v/s Stateful Component

| Feature | Stateless Component | Stateful Component |
| :--- | :--- | :--- |
| **State Management** | No state management, relies only on props | Manages internal state (`useState` or `this.state`) |
| **Reactivity** | Does not trigger re-renders based on state | Re-renders when internal state changes |

-----

### React Class LifeCycle Methods / Flow

#### Mounting (Creation and Insertion into the DOM)

  * `constructor()`: Initializes the component’s state and binds methods.
  * `static getDerivedStateFromProps()`: Updates state based on changes in props, called before each render.
  * `render()`: Renders the component’s UI based on the current state and props.
  * **`componentDidMount()`**: Called once the component is mounted; used for setting up subscriptions, fetching data.

#### Updating (Re-rendering from props or state changes)

  * `shouldComponentUpdate()`: Determines whether the component should re-render (can optimize performance).
  * **`componentDidUpdate()`**: Called after the component updates, useful for performing side effects based on state or prop changes.

#### Unmounting (Removal from the DOM)

  * **`componentWillUnmount()`**: Used to clean up resources like timers or event listeners before the component is removed.

-----

### Composition

**Composition** is a design pattern where components are built using smaller, reusable components. React heavily favors composition over inheritance.

| Aspect | Inheritance | Composition |
| :--- | :--- | :--- |
| **Basic Concept** | One class inherits from another. | Combine smaller components to create larger ones. |
| **Coupling** | Tight coupling | Loose coupling |
| **React’s Preference** | Not commonly used in React. | **React prefers composition** as it leads to better modularity. |

-----

### Context API

A powerful tool that allows you to share state or values across the entire component tree, avoiding **prop drilling**.

  * **Steps:** 1. Create a Context. 2. Provide the context. 3. Consume the context.

-----

### Error Boundaries

Error Boundaries are like safety nets for your app. They catch errors in parts of your app and show a fallback message.

  * **Key Points:** Uses `getDerivedStateFromError` and `componentDidCatch`.

-----

### Refs and Forwarding Refs

  * **Refs**: A way to get direct access to a DOM element or a React component. Used to focus on an input field automatically, measure size, etc.
  * **Forwarding Refs**: Allows a parent component to pass a ref to its child component's DOM element using `React.forwardRef`.

#### Example of Forwarding Refs

```javascript
import React, { forwardRef } from 'react';

// Child component (using forwardRef)
const Input = forwardRef((props, ref) => {
  return <input ref={ref} type="text" />;
});

// Parent component (using ref)
function App() {
  const inputRef = React.createRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <Input ref={inputRef} />
      <button onClick={focusInput}>Focus the input</button>
    </div>
  );
}

export default App;
```

-----

### Higher-Order Components

A higher-order component is a function that takes a component and returns a new component.

-----

### Optimizing Performance

#### 1\. Use `React.memo()`:

  * **Prevent unnecessary re-renders of functional components.**
  * **Example:**

<!-- end list -->

```javascript
const MyComponent = React.memo((props) => {
  return <div>{props.name}</div>;
});
```

#### 2\. Use `useCallback` for functions:

  * **Memoize callback functions** so they don't get recreated on every render.
  * **Example:**

<!-- end list -->

```javascript
const handleClick = useCallback(() => {
  console.log('Button clicked');
}, []);
```

#### 3\. Use `useMemo()`:

  * **Memoize the result of an expensive calculation** so it doesn't run on every render.
  * **Example:**

<!-- end list -->

```javascript
const expensiveCalculation = useMemo(() => {
  return someExpensiveFunction(data);
}, [data]);
```

#### 4\. Avoid Anonymous Functions in JSX.

  * Anonymous functions create new instances on every render, which can cause unnecessary re-renders.
  * **Example:**

<!-- end list -->

```javascript
// Bad
<button onClick={() => handleClick()}>Click me</button>

// Good
const memoizedHandleClick = useCallback(() => handleClick(), []);
<button onClick={memoizedHandleClick}>Click me</button>
```

#### 5\. Lazy Load Components with `React.lazy` and `Suspense`.

  * Split your application into smaller chunks.
  * **Example:**

<!-- end list -->

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <OtherComponent />
    </Suspense>
  );
}
```

#### 6\. Optimize State Updates.

  * Minimize unnecessary state updates; avoid setting state if the value hasn’t changed.
  * **Example:**

<!-- end list -->

```javascript
setState(prevState => {
  if (prevState.someValue !== newValue) {
    return { someValue: newValue };
  }
  return prevState;
});
```

#### 8\. Virtualize Long Lists (React Virtualized).

  * Only render the items that are currently visible in the viewport.
  * **Example (using react-window):**

<!-- end list -->

```javascript
import { FixedSizeList as List } from 'react-window';

const rowRenderer = ({ index, style }) => (
  <div style={style}>Item {index}</div>
);

function App() {
  return (
    <List
      height={150}
      itemCount={1000}
      itemSize={35}
      width={300}
    >
      {rowRenderer}
    </List>
  );
}
```

-----

### Redux and Middleware

**Redux** is a state management library for predictable state changes.

#### Redux Thunk

  * Allows action creators to return a **function (thunk)** instead of a plain action object.
  * **Syntax Example:**

<!-- end list -->

```javascript
// Thunk Action Creator
const fetchUser = (id) => (dispatch, getState) => {
  dispatch({ type: 'FETCH_USER_REQUEST' });
  api.getUser(id).then(user => {
    dispatch({ type: 'FETCH_USER_SUCCESS', payload: user });
  });
};
```

#### Redux Saga

  * Uses **Generator Functions** and declarative **Effects** for complex, asynchronous flows.
  * **Syntax Example (Generator Function):**

<!-- end list -->

```javascript
function* fetchUserSaga(action) {
  try {
    const user = yield call(api.getUser, action.payload.id);
    yield put({ type: 'FETCH_USER_SUCCESS', user: user });
  } catch (e) {
    yield put({ type: 'FETCH_USER_FAILURE', message: e.message });
  }
}
```
