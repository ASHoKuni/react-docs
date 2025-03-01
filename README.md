# React.js Design Patterns 

🔹 Component Patterns → HOC, Render Props, Compound Components  
🔹 State Management → Context, Redux, Zustand    
🔹 Performance → Memoization, Lazy Loading  
🔹 Architecture → Atomic Design, Micro-Frontends   


## 🚀 Component Composition in React (Component Design Patterns)

Component Composition in React
Component Composition is the practice of building UI components by combining smaller, reusable components instead of creating large, monolithic ones. It follows the "composition over inheritance" principle, making the UI modular, maintainable, and scalable.

#### Why Use Component Composition?  

✅ Reusability – Breaks down UI into smaller pieces that can be reused across the app.  
✅ Maintainability – Easier to debug and update small components rather than a huge component.  
✅ Scalability – Helps in managing complex UIs by structuring them into smaller parts.  
✅ Separation of Concerns – Each component has a single responsibility, making the code clean.  

### Types of composition - 

#### 1. Containment (Children Prop)

Containment refers to a design pattern where a parent component wraps children inside it using the children prop.

Useful when a component needs to wrap other components (e.g., Card, Modal, Layout).

```javascript 

const Card = ({ children }: { children: React.ReactNode }) => {
  return (
    <div className="border p-4 rounded-lg shadow-md">{children}</div>
  );
};

const App = () => {
  return (
    <Card>
      <h2 className="text-xl font-bold">Card Title</h2>
      <p>This is some card content.</p>
    </Card>
  );
};
```


#### 2. What is HOC 

A Higher-Order Component (HOC) is a function that takes a component and returns a new enhanced component with additional functionality.

#####  Why Use HOCs? 

✅ Code Reusability - Share logic between multiple components.   
✅ Separation of Concerns - Keep components focused on UI, while logic is handled by HOCs.  
✅ Encapsulation - Wrap a component with additional behavior without modifying the original.  
✅ Conditional Rendering - Show/hide content based on logic.


```javascript
function withUserData(WrappedComponent: React.ComponentType<{ user: string }>) {
  return function EnhancedComponent(props: any) {
    const user = "John Doe"; // Simulating fetched user data
    return <WrappedComponent {...props} user={user} />;
  };
}

// Component expecting a "user" prop
const Profile = ({ user }: { user: string }) => {
  return <h1>Welcome, {user}!</h1>;
};

const ProfileWithUserData = withUserData(Profile);

export default function App() {
  return <ProfileWithUserData />;
}

```

#### 3. What is Render Props in React?


 A Render Prop is a pattern in React where a component receives a function as a prop that it uses to determine what to render.

📌 Definition:

A Render Prop is a function prop that a component uses to control what is rendered inside it.

``` javascript

import { useState } from "react";

const MouseTracker = ({ render }: { render: (pos: { x: number; y: number }) => JSX.Element }) => {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (event: React.MouseEvent) => {
    setPosition({ x: event.clientX, y: event.clientY });
  };

  return <div onMouseMove={handleMouseMove} style={{ height: "100vh", background: "#f5f5f5" }}>{render(position)}</div>;
};

// Usage
export default function App() {
  return (
    <MouseTracker render={({ x, y }) => (
      <h1>Mouse Position: {x}, {y}</h1>
    )} />
  );
}

```

#### 4. Specialization (Props-Based Composition)
 
Instead of using inheritance, React allows specialized components by passing props to a parent component.


```javascript 

const Button = ({ type, label }: { type: "primary" | "secondary"; label: string }) => {
  return (
    <button className={`px-4 py-2 ${type === "primary" ? "bg-blue-500 text-white" : "bg-gray-300 text-black"}`}>
      {label}
    </button>
  );
};

const App = () => {
  return (
    <div>
      <Button type="primary" label="Submit" />
      <Button type="secondary" label="Cancel" />
    </div>
  );
};

```


#### 5.  Controlled vs. Uncontrolled Components


- Controlled: Manages state via useState or useReducer.
- Uncontrolled: Uses useRef to control values.

####  Example (Controlled)
 
``` Javascript 
const ControlledInput = () => {
  const [value, setValue] = useState("");
  return <input value={value} onChange={(e) => setValue(e.target.value)} />;
};
```
#### Example (Uncontrolled)
 
``` Javascript 
const UncontrolledInput = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  const handleSubmit = () => alert(inputRef.current?.value);

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
};
```

---

## 🚀 State Management Patterns


### 👉 Lift State Up

Use Case: Share state between sibling components.

Example

```javascript
const Parent = () => {
  const [count, setCount] = useState(0);
  return <Child count={count} setCount={setCount} />;
};

const Child = ({ count, setCount }: { count: number; setCount: React.Dispatch<React.SetStateAction<number>> }) => (
  <button onClick={() => setCount(count + 1)}>Count: {count}</button>
);
```

### 👉 Context API for Global State

Use Case: Avoid prop drilling.

Example:

```javascript 
const UserContext = createContext({ user: "Guest" });

const App = () => (
  <UserContext.Provider value={{ user: "John" }}>
    <Child />
  </UserContext.Provider>
);

const Child = () => {
  const { user } = useContext(UserContext);
  return <h2>{user}</h2>;
};
```

### 👉 Redux for Large Scale State Management

Use Case: Manage state in complex applications.

Example (Redux Toolkit)
 
```javascript 
const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
  },
});

export const { increment } = counterSlice.actions;
export default counterSlice.reducer;
```

### 👉 Zustand for Lightweight State

Use Case: Alternative to Redux for small projects.

Example:
 ```javascript 
import create from "zustand";

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

const Counter = () => {
  const { count, increment } = useStore();
  return <button onClick={increment}>Count: {count}</button>;
};
```
---

## 🚀 Performance Optimization Patterns
These patterns help improve performance in React apps.

### 👉 Memoization (useMemo)
Use Case: Avoid unnecessary recalculations.
Example:

  ```javascript 
const expensiveCalculation = useMemo(() => computeExpensiveValue(input), [input]);
```

### 👉  Memoization (useCallback)
Use Case: Prevent function recreation.

Example:

  ```javascript 
const memoizedCallback = useCallback(() => expensiveFunction(), [dependencies]);
```
### 👉 React.memo for Component Memoization
Use Case: Prevent re-renders of components with same props.

Example:

  ```javascript 
const MemoizedComponent = React.memo(({ count }) => <h2>{count}</h2>);
```
---
## 🚀  Architectural Patterns

These patterns help structure React apps efficiently.

### 👉 Atomic Design Pattern
Use Case: Break down UI into small reusable components.

Example:

  ```javascript 
components/
  ├── atoms/
  ├── molecules/
  ├── organisms/
  ├── templates/
  ├── pages/
```

### 👉 Micro-Frontends with Module Federation

Use Case: Split monolithic frontend into multiple deployable parts.

  ```javascript 
import("remoteApp/Button").then(({ Button }) => <Button />);
```
## 🚀 Advanced Patterns
These patterns enhance complex React apps.

### 👉  Portals for Modals

Use Case: Render outside parent DOM tree.

  ```javascript 
return ReactDOM.createPortal(<Modal />, document.getElementById("modal-root"));
```

### 👉 Error Boundaries
Use Case: Catch runtime errors in components.

Example : 

```javascript 
class ErrorBoundary extends React.Component {
  componentDidCatch(error, info) { console.log(error, info); }
  render() { return this.props.children; }
}
```
---

# 🚀 React Reconciliation & Fiber Architecture

 Reconciliation is the diffing algorithm React uses to efficiently update the DOM. Instead of updating everything in the DOM, React compares the previous Virtual DOM with the new Virtual DOM and updates only the changed parts.

## How Reconciliation Works: (without fiber)

**Virtual DOM:** React maintains a lightweight copy of the actual DOM called the Virtual DOM. When a component's state or props change, React creates a new Virtual DOM tree.

**Diffing Algorithm:** React compares the new Virtual DOM tree with the previous one using a diffing algorithm. This process is called "reconciliation." The goal is to determine the minimal set of changes needed to update the actual DOM.

**Update Dom:** Instead of re-rendering the entire DOM, React only updates the parts that have changed. This makes the process much faster and more efficient.

**Keys for Lists:** When rendering lists, React uses key props to identify which items have changed, been added, or been removed. This helps React optimize the reconciliation process for lists.

### Key Concepts:
**Re-rendering:** When a component's state or props change, React re-renders the component and its children.

**Batching:** React batches multiple state updates into a single re-render to improve performance.

**Component Lifecycle:** Reconciliation interacts with the component lifecycle methods (e.g., componentDidUpdate, useEffect) to ensure updates are handled correctly.

Example:

```javascript 
function MyComponent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```
🔹When the button is clicked, setCount updates the state.  
🔹React re-renders the component, creates a new Virtual DOM tree, and reconciles it with the previous one.  
🔹Only the p tag's text content is updated in the actual DOM.  
  
#### Benefits of Reconciliation:  
***Performance:*** Minimizes DOM manipulations, making updates faster.

***Declarative UI:*** Developers describe what the UI should look like, and React handles the updates efficiently.

***Simplified Development:*** Developers don't need to manually manage DOM updates.

In summary, reconciliation is the process that allows React to efficiently update the UI in response to state and prop changes, ensuring a smooth and performant user experience.

### React's Optimization in Reconciliation
React uses two key strategies to make this process efficient:

1️⃣ **Element Type Comparison**  
🔹If the type (tag) of the element is the same, React updates its properties (attributes).  
🔹If the type changes, React destroys the old element and creates a new one.

✅ Example (Same Element Type → Only Text Updates):
```javascript 
function App() {
  return <h1>Hello</h1>; 
}

// Updates to:
function App() {
  return <h1>Hi</h1>; // React only updates the text, not the whole <h1> tag
}

```
🔴 Example (Different Element Type → Re-creates DOM Node):
```javascript 
 function App() {
  return <h1>Hello</h1>;
}

// Updates to:
function App() {
  return <p>Hello</p>; // React destroys <h1> and creates a new <p> tag
}
```

2️⃣ **Key-Based List Reconciliation**  
🔹When rendering lists, React tracks items using the "key" prop.  
🔹If a key remains the same, React reuses the DOM node.  
🔹If a key changes, React removes the old node and creates a new one.   

✅ Correct Way (Keys are stable):
```javascript 
const items = ["A", "B", "C"];
return items.map((item) => <li key={item}>{item}</li>);
```
🔴 Incorrect Way (No Key or Index Key):
```javascript
return items.map((item, index) => <li key={index}>{item}</li>); 
```
🚨 Using index as a key can cause unwanted re-renders when items reorder.

**Conclusion**  

Reconciliation is React’s way of efficiently updating the UI without unnecessary DOM updates. React Fiber (React 16+) further optimizes this process using incremental rendering.

*******
# 🚀 React Fiber

**React Fiber is the new reconciliation algorithm introduced in React 16+**. It is designed to improve the performance, flexibility, and responsiveness of React applications, especially for complex and dynamic UIs. Fiber enables features like **incremental rendering**, prioritization of updates, and the ability to pause, abort, or reuse work during rendering.


### 🛠 Why Was Fiber Introduced?

Before Fiber, React used a synchronous reconciliation algorithm. This meant:

- Long-running updates blocked the main thread, causing UI freezes.  
- React couldn't prioritize urgent updates (e.g., animations, user interactions).  
- Updates couldn't be interrupted once started.  

Fiber fixes these issues by breaking rendering into small units of work and allowing React to pause, resume, or cancel updates - based on priority.  

 ### 🔄How Fiber Works?

Fiber introduces two key phases:

 
1️⃣ **Render Phase (Work Phase - Can be Interrupted)** - Initial Render -->   State Update --> Reconciliation
- React creates a Fiber Tree and calculates changes.
- It doesn't commit changes immediately but prepares them.
- This phase can be paused for high-priority tasks (e.g., animations, clicks).

2️⃣ **Commit Phase (DOM Update - Cannot be Interrupted)**
- React applies the calculated changes to the real DOM.
- This phase is synchronous and happens in a single step.

 
### 🚀 Key Features of Fiber

#### ✅ 1. Interruptible Rendering
- React pauses rendering when a high-priority task (like user input) comes in. 
- Helps keep the UI responsive.

```javascript 
import { startTransition, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    startTransition(() => {
      setCount((prev) => prev + 1); // Low-priority update
    });
  }

  return <button onClick={handleClick}>Click me ({count})</button>;
}
```
📝 startTransition tells React:  
"Hey, this update is not urgent. Pause it if needed."


#### ✅ 2. Concurrent Mode (Improved UI Responsiveness)  
- React splits rendering into small tasks and processes high-priority updates first.
- Helps smoothen animations and user interactions.

```javascript 
import { useState, useTransition } from "react";

function SearchList({ query }) {
  return <div>Showing results for {query}...</div>;
}

function App() {
  const [query, setQuery] = useState("");
  const [isPending, startTransition] = useTransition();

  function handleChange(event) {
    startTransition(() => {
      setQuery(event.target.value); // Low priority update
    });
  }

  return (
    <div>
      <input type="text" onChange={handleChange} placeholder="Search..." />
      {isPending ? <p>Loading...</p> : <SearchList query={query} />}
    </div>
  );
}

```

#### ✅ 3. Prioritized Updates
React Fiber assigns priority levels to different tasks:

1. 🎯 User interactions (High Priority)  
2.  🖼 Animations & transitions (Medium Priority)  
3. 🛠 Data fetching, background tasks (Low Priority)  

💡 This prevents UI blocking, ensuring smoother interactions.  

#### ✅ 4. Better Error Handling
- With Fiber, React can recover from errors without crashing the entire app.
- Error Boundaries (introduced in React 16) help catch errors in a part of the component tree.

```javascript 
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error("Error:", error, info);
  }

  render() {
    return this.state.hasError ? <h2>Something went wrong.</h2> : this.props.children;
  }
}

// Usage:
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>;

```
🚀 Without crashing the whole app, errors are handled gracefully!



