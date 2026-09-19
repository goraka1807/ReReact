#  Handling User Input, Refs & Controlled vs Uncontrolled Components

## 1.  Handling User Input in React

React needs a way to **capture what the user types** into an input.

A common approach is to store the input value in **state**.

### Basic Syntax

```js
const [value, setValue] = useState(""); 
// value = current input value
// setValue = changes the value
// "" = initial value
```

Then connect the state to the input:

```js
<input
  value={value} 
  // The input displays whatever is currently stored in state

  onChange={(event) => setValue(event.target.value)}
  // Runs whenever the user types
  // event.target = the input element
  // event.target.value = what the user typed
  // setValue() updates the state
/>
```

### Example

```js
import React, { useState } from "react";

function Greeting() {
  const [name, setName] = useState("");
  // name stores whatever the user types

  return (
    <div>
      <h1>Hello, {name}!</h1>
      {/* React displays the current value of name */}

      <input
        type="text"
        value={name}
        // Input gets its value from React state

        onChange={(event) => setName(event.target.value)}
        // User types → onChange runs → state updates
      />
    </div>
  );
}
```
---

# 2. What Are Refs?

A **ref** gives us a way to keep a reference to a DOM element or another value.

For DOM elements, `useRef()` can let us access the actual element directly.

### Basic Syntax

```js
const myRef = useRef();
// Creates a ref object
// Initially, myRef.current is null
```

Then attach it to an element:

```js
<input ref={myRef} />
// React connects myRef to this specific input element
```

After React connects it:

```js
myRef.current
```

points to that input DOM element.

---

## Example 1 — Creating a Ref

```js
import React, { useRef } from "react";

function Greeting() {
  const nameRef = useRef();
  // Creates the ref
  // nameRef.current starts as null

  return (
    <input
      ref={nameRef}
      // Connects the ref to this input
    />
  );
}
```

The important relationship is:

```js
nameRef
   ↓
nameRef.current
   ↓
<input> DOM element
```

---

## Example 2 — Reading the Input Value

```js
function Greeting() {
  const nameRef = useRef();

  return (
    <div>
      <input
        ref={nameRef}
        // nameRef.current will point to this input
      />

      <button
        onClick={() => {
          console.log(nameRef.current.value);
          // .current = the input element
          // .value = the text currently inside the input
        }}
      >
        Show Name
      </button>
    </div>
  );
}
```

If the user types:

```
Alex
```

then:

```js
nameRef.current.value
```

gives:

```
"Alex"
```

---

## Example 3 — Using a Ref to Focus an Input

```js
function Greeting() {
  const inputRef = useRef();

  function focusInput() {
    inputRef.current.focus();
    // current = actual input element
    // focus() = browser method that focuses the input
  }

  return (
    <div>
      <input ref={inputRef} />

      <button onClick={focusInput}>
        Focus Input
      </button>
    </div>
  );
}
```

Here, React isn't storing the input's text in state.

Instead, we're directly accessing the DOM element through:

```
inputRef.current
```

### Remember

> **`useRef()` creates the reference, `ref={...}` connects it to the element, and `.current` gives you access to that element.**

---

#  3. Controlled vs Uncontrolled Components

This is the important distinction of the lesson.

### Controlled

**React state controls the input.**

```js
const [name, setName] = useState("");
```

The input gets its value from:

```js
value={name}
```

and changes state through:

```js
onChange={...}
```

---

### Uncontrolled

**The DOM keeps track of the input's value.**

Instead of storing every keystroke in state, we use a ref to access the input when we need it.

```js
const inputRef = useRef();

<input ref={inputRef} />
```

Then:

```js
inputRef.current.value
```

reads the current value directly from the DOM.

---

# 📝 4. Creating an Uncontrolled Component

An uncontrolled input does **not** have its value controlled by React state.

The browser/input keeps track of the value.

### Example

```js
import React, { useRef } from "react";

function UncontrolledComponent() {
  const inputRef = useRef();
  // Creates a ref for the input

  function handleSubmit(event) {
    event.preventDefault();
    // Stops the form from refreshing the page

    alert(inputRef.current.value);
    // current = the actual input element
    // value = whatever the user typed
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={inputRef}
        type="text"
        // Connects the input to inputRef
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```
---

#  5. Creating a Controlled Component

In a **controlled component**, React state is responsible for the input's value.

### Example

```js
import React, { useState } from "react";

function ControlledComponent() {
  const [name, setName] = useState("");
  // name = current input value
  // setName = updates the input value

  function handleSubmit(event) {
    event.preventDefault();
    // Prevents the browser's default form submission

    alert(name);
    // Reads the value from React state
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        // Input displays the value stored in React state

        onChange={(event) => setName(event.target.value)}
        // User types → get the new value → update state
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

### The flow

If the user types `"John"`:

```js
User types
    ↓
onChange runs
    ↓
event.target.value = "John"
    ↓
setName("John")
    ↓
React state becomes "John"
    ↓
value={name} displays "John"
```

The important part is that **React is in control of the value**.

---

#  6. Controlled vs Uncontrolled — Side by Side

### Controlled
```js
const [name, setName] = useState("");

<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

React owns the value.
### Uncontrolled

```js
const inputRef = useRef();

<input ref={inputRef} />
```

The DOM owns the value, and React accesses it when needed:

```js
inputRef.current.value
```

###  Golden Rule

> **Controlled = React state owns the input value.**  
> **Uncontrolled = the DOM owns the input value, and a ref gives you access to it.**

---

#  7. Nested Functional Components & Props

React applications are usually divided into smaller components.

For example:

```
People
 ├── Person
 ├── Person
 └── Person
```

`People` is the **parent**.

Each `Person` is a **child**.

The parent can pass information to the children using **props**.

---

## Example 1 — One Person

```js
function Person({ name }) {
  // name comes from the parent as a prop

  return <p>Hello, {name}!</p>;
}

function People() {
  return (
    <Person name="Alice" />
    // People passes "Alice" to Person through the name prop
  );
}
```

The data flow is:

```js
People
  ↓
name="Alice"
  ↓
Person({ name })
```

---

## Example 2 — Multiple People with `.map()`

```js
function Person({ name }) {
  // Receives name from People through props

  return <p>Hello, {name}!</p>;
}

function People() {
  const names = ["Alice", "Bob", "Charlie"];

  return (
    <div>
      {names.map((name) => (
        <Person
          key={name}
          name={name}
          // Passes the current name to Person as a prop
        />
      ))}
    </div>
  );
}
```

The `.map()` runs once for every name.

For `"Alice"`:

```js
<Person key="Alice" name="Alice" />
```

For `"Bob"`:

```js
<Person key="Bob" name="Bob" />
```

For `"Charlie"`:

```js
<Person key="Charlie" name="Charlie" />
```

And each `Person` receives its own `name` prop.

---

## Example 3 — Parent Passing Different Props

Props aren't limited to strings.

```js
function Person({ name, age }) {
  // Receives both values from the parent

  return (
    <p>
      {name} is {age} years old.
    </p>
  );
}

function People() {
  return (
    <div>
      <Person name="Alice" age={25} />
      <Person name="Bob" age={30} />
    </div>
  );
}
```

Here:

```js
<Person name="Alice" age={25} />
```

passes two props.

The child receives them:

```js
function Person({ name, age })
```

So:

```js
Parent
  ↓
name + age props
  ↓
Child
```

### Remember

> **Props are values passed from a parent component to a child component. The child receives them through its function parameters.**

---

# 🧠 Golden Rule

> **Use state when React needs to control and react to input changes. Use a ref when you need direct access to a DOM element without storing that value in state.**

# 💡 Remember

- **`useState`** → React manages the input value.
- **`useRef`** → gives you a persistent reference to a DOM element.
- **Controlled input** → `value` + `onChange` + state.
- **Uncontrolled input** → `ref` + DOM value.
- **Props** → parent passes data to child.

## ⚡ Quick Revision

```js
// Controlled
const [name, setName] = useState("");

<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

```js
// Uncontrolled
const inputRef = useRef();

<input ref={inputRef} />

// Later:
inputRef.current.value
```

```js
// Props
function Person({ name }) {
  return <p>{name}</p>;
}

<Person name="Alice" />
```

The three concepts to keep separate are:

**State controls values → Refs access DOM → Props pass data between components.**