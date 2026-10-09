# `useEffect` and its uses

##  What is `useEffect`?

`useEffect` is a **React Hook** that lets functional components perform **effects**.

An effect is a task that happens because the component **renders, updates, or unmounts**.

Examples of effects:

- Logging something to the console
- Fetching data
- Starting a timer
- Listening for browser events
- Cleaning something up when the component disappears

Before understanding `useEffect`, remember:

>  **`useState` gives a component memory.**  
>  **`useEffect` lets the component perform side effects.**

---

## `useEffect` Syntax

```js
useEffect(() => {
  // effect
}, [dependencies]);
````

There are two important parts:

- `() => {}` → **what the effect should do**
- `[dependencies]` → **what changes should cause the effect to run again**

---

# Example: `useEffect` with State

Suppose we have a counter.

Whenever the counter changes, we want to print its value in the console.

```js
import React, { useState, useEffect } from "react";

function CounterApp() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    console.log(`Counter value: ${counter}`);
  }, [counter]);

  return (
    <div>
      <p>Counter: {counter}</p>

      <button onClick={() => setCounter(counter + 1)}>
        Increase Count
      </button>
    </div>
  );
}

export default CounterApp;
```

Here:

```
[counter]
```

is the **dependency array**.

It tells React:

> "Run this effect again when `counter` changes."

---

# Dependency Array

The **dependency array** is the second argument of `useEffect`.

It tells `useEffect` which **state or props it should watch**.

### Example

```
useEffect(() => {
  console.log(`Counter value: ${counter}`);
}, [counter]);
```

Because `counter` is inside the dependency array, the effect runs again whenever `counter` changes.

---

#  Three Ways `useEffect` Can Run

This is one of the most important things to remember.

## 1️.) No Dependency Array

```
useEffect(() => {
  console.log("Component rendered.");
});
```

Without a dependency array, the effect runs **after every render**.

```
Render → Effect
Render → Effect
Render → Effect
```

It doesn't watch one specific state or prop.

> 🧠 **No dependency array → runs after every render**

---

## 2️.) Empty Dependency Array `[]`

```
useEffect(() => {
  console.log("Component did mount");
}, []);
```

An empty dependency array means there are **no dependencies to watch**.

The effect runs when the component **mounts**.

### What is mounting?

**Mounting** means the component appears on the screen for the first time.

So:

```
useEffect(() => {
  console.log("Component did mount");
}, []);
```

means:

> "Do this when the component first appears."

### 🚀 Easy way to remember

Think of a spaceship starting its mission.

The launch sequence happens when the spaceship takes off.

Similarly:

```
[]
```

means the effect performs its initial action when the component mounts.

> ⚡ **Empty dependency array `[]` → run on mount**

---

# 3️.) Non-empty Dependency Array

```
useEffect(() => {
  console.log(`Counter value: ${counter}`);
}, [counter]);
```

Here, `counter` is being watched.

The effect runs:

- When the component initially renders
- Again whenever `counter` changes

For example:

```
counter = 0 → Effect runs
counter = 1 → Effect runs
counter = 2 → Effect runs
counter = 3 → Effect runs
```

> 🎯 **`[counter]` → run again when `counter` changes**

---

#  `useEffect` Dependency Cheat Sheet

|Dependency|When effect runs|
|---|---|
|No array|After every render|
|`[]`|When component mounts|
|`[count]`|On mount + whenever `count` changes|
|`[count, name]`|On mount + whenever `count` or `name` changes|

---

#  Cleaning Up with `useEffect`

Sometimes an effect **starts something that needs to be stopped later**.

For example:

- Start a timer → stop the timer
- Add an event listener → remove it
- Subscribe to something → unsubscribe

This is where the **cleanup function** comes in.

### Syntax

```js
useEffect(() => {
  // setup

  return () => {
    // cleanup
  };
}, []);
```

The function returned from `useEffect` is the **cleanup function**.

---

##  Cleanup Example

```js
useEffect(() => {
  console.log("Component did mount");

  return () => {
    console.log("Component will unmount");
  };
}, []);
```

The first part runs when the component mounts:

```js
console.log("Component did mount");
```

The returned function runs when the component is about to **unmount**:

```js
return () => {
  console.log("Component will unmount");
};
```

### What is unmounting?

**Unmounting** means the component is removed from the screen.

So:

```js
Component appears → Mount
Component disappears → Unmount
```

> **Cleanup = undo what your effect started.**

---

#  The Big Picture

`useEffect` is mainly about controlling **when an effect runs** and, when necessary, **cleaning it up**.

```js
useEffect(() => {
  // do something

  return () => {
    // clean it up
  };
}, [dependencies]);
```

Think about it in three questions:

### 1. What should I do?

```js
useEffect(() => {
  // effect
});
```

### 2. When should I do it?

```js
[dependencies]
```

### 3. What should I undo?

```js
return () => {
  // cleanup
};
```

---

# 🔑 Golden Rule

> **`useState` remembers. `useEffect` reacts. Dependencies control when it reacts. Cleanup undoes what it started.**

---

#  Quick Revision

### `useEffect`

Used to perform **side effects** in functional components.

```js
useEffect(() => {
  // effect
}, [dependencies]);
```

### Dependency Array

```js
useEffect(() => {
  // runs after every render
});
```

```js
useEffect(() => {
  // runs on mount
}, []);
```

```js
useEffect(() => {
  // runs on mount + when count changes
}, [count]);
```

### Cleanup

```js
useEffect(() => {
  // setup

  return () => {
    // cleanup
  };
}, []);
```

### Important Terms

- **Mount** → component appears on the screen
- **Render** → React creates/updates the UI
- **Effect** → side-effect code executed after rendering
- **Dependency** → state/prop the effect watches
- **Unmount** → component is removed from the screen
- **Cleanup** → code that undoes an effect

---

#  Remember

```js
useEffect(() => {
  // WHAT should happen?

  return () => {
    // WHAT should be cleaned up?
  };
}, [dependencies]); // WHEN should it happen again?
```

>  **WHAT → Effect**  
>  **WHEN → Dependencies**  
> **UNDO → Cleanup**