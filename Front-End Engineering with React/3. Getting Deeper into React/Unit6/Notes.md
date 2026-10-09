# ⚛️ Advanced Forms, Child-to-Parent Data & `props.children`

## Overview

- **Advanced form validation**
- **Passing data from child → parent**
- **Handling child events in a parent**
- **`props.children`** for building wrapper components

The important idea is that React normally passes **data down through props**, but a child can communicate back up by **calling a function given to it by the parent**.

---

# 🔐 Advanced Form Validation

Form validation checks whether the information entered by the user is acceptable **before submitting it**.

For example, an email should follow a valid format.

We can use:

- `useState` → stores the current form value
- `onChange` → updates the value while typing
- `onSubmit` → runs validation when the form is submitted
- `pattern.test()` → checks whether the value matches a pattern

### Example

```js
import React, { useState } from "react";

function SimpleEmailForm() {
  const [email, setEmail] = useState("");
  // State stores whatever the user types

  const handleEmailChange = (e) => {
    setEmail(e.target.value);
    // Update email state whenever the input changes
  };

  const validateEmail = (e) => {
    e.preventDefault();
    // Prevent the browser from refreshing the page

    const pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    // Pattern used to check the basic email format

    pattern.test(email)
      ? alert("Email is valid")
      : alert("Invalid email");
    // test() returns true or false
  };

  return (
    <form onSubmit={validateEmail}>
      <input
        type="text"
        value={email}
        onChange={handleEmailChange}
        // Input value comes from React state
      />

      <button type="submit">
        Submit
      </button>
    </form>
  );
}
```

### 🧠 How it works

1. User types an email.
2. `onChange` runs.
3. `setEmail()` updates the state.
4. User clicks **Submit**.
5. `validateEmail()` runs.
6. `pattern.test(email)` checks the email.
7. React shows whether it is valid.

---

#  Child → Parent Data

React normally passes information from **parent to child using props**.

But sometimes a child needs to send information back.

The common React pattern is:

**Parent creates a function → passes it to child as a prop → child calls that function with some data.**

The parent then receives that data and can update its state.

### Example

```js
import React, { useState } from "react";

function Parent() {
  const [message, setMessage] = useState("");

  const handleMessage = (data) => {
    setMessage(data);
    // Receives data from Child and stores it in Parent state
  };

  return (
    <div>
      <Child sendMessage={handleMessage} />

      <p>{message}</p>
    </div>
  );
}

function Child({ sendMessage }) {
  return (
    <button
      onClick={() => sendMessage("Hello from Child!")}
      // Calls the function received from Parent
    >
      Send Message
    </button>
  );
}
```

### Where does the data come from?

- `Parent` owns the `message` state.
- `Parent` creates `handleMessage`.
- `handleMessage` is passed to `Child` as `sendMessage`.
- `Child` calls `sendMessage(...)`.
- The data goes into `Parent`'s `handleMessage`.
- Parent updates its own state.

**The child doesn't directly change the parent's state. It calls the function the parent gave it.**

---

#  Child → Parent Through Multiple Components

The same idea can work through several levels.

```js
function Grandparent() {
  const [data, setData] = useState("");

  const handleCallback = (value) => {
    setData(value);
    // Receives data that originated in Child
  };

  return (
    <Parent grandparentCallback={handleCallback} />
  );
}

function Parent({ grandparentCallback }) {
  return (
    <Child parentCallback={grandparentCallback} />
    // Passes the same callback down to Child
  );
}

function Child({ parentCallback }) {
  return (
    <button
      onClick={() => parentCallback("Data from Child")}
      // Child calls the callback with its data
    >
      Click
    </button>
  );
}
```

Here, the **Grandparent owns the state**, while the **Child provides the data** by calling the callback it received.

---

# Handling Events in Parent Components

A parent can define an event handler and give it to a child.

The child doesn't need to know what the function does.

It simply calls the function when its event happens.

```js
function Parent() {
  const handleClick = () => {
    console.log("Clicked in Child");
    // Parent decides what should happen
  };

  return (
    <Child onButtonClick={handleClick} />
    // Parent passes its function to Child
  );
}

function Child({ onButtonClick }) {
  return (
    <button onClick={onButtonClick}>
      Click
    </button>
    // Child triggers the parent's function when clicked
  );
}
```

###  Key idea

The **event happens inside the Child**, but the **function handling that event belongs to the Parent**.

This is useful when the parent needs to control what happens after something occurs in a child.

---

# 📦 `props.children`

`props.children` is a special prop that contains **whatever you place between a component's opening and closing tags**.

For example:

```js
<Box>
  <h2>Hello!</h2>
  <p>Welcome to React</p>
</Box>
```

Everything inside `<Box>...</Box>` becomes `props.children`.

### Example

```js
function Box(props) {
  return (
    <div className="box">
      {props.children}
      {/* Renders whatever was placed inside <Box> */}
    </div>
  );
}

function App() {
  return (
    <Box>
      <h2>Hello there!</h2>
      <p>Welcome to React</p>
    </Box>
  );
}
```

So `Box` doesn't need to know exactly what content it will receive.

It simply provides the **container/wrapper**, while the parent decides what goes inside it.

### Easier syntax with destructuring

```js
function Box({ children }) {
  return (
    <div className="box">
      {children}
      {/* children contains the content placed inside Box */}
    </div>
  );
}
```

---

# 🧠 Golden Rule

> **Props pass information into a component. Callback functions allow a child to communicate back by calling a function provided by its parent.**

And:

> **`props.children` represents the content placed between a component's opening and closing tags.**

---

# 🧒 Example 1 — Like You're 7

Imagine you have a **toy box**.

You give the box different toys:

```js
function ToyBox({ children }) {
  return (
    <div>
      <h2>My Toy Box</h2>

      {children}
      {/* Whatever toys/content were placed inside */}
    </div>
  );
}

function App() {
  return (
    <ToyBox>
      <p>🚗 Car</p>
      <p>🧸 Teddy Bear</p>
      <p>🚀 Rocket</p>
    </ToyBox>
  );
}
```

`ToyBox` doesn't decide which toys are inside.

The person using `ToyBox` decides.

That's basically what **`props.children`** does.

---

# 🚀 Example 2 — Space Mission

Imagine a spaceship has a **MissionControl** component.

The spaceship's control panel can tell Mission Control when something happens.

```js
import React, { useState } from "react";

function MissionControl() {
  const [message, setMessage] = useState("Waiting for astronaut...");

  const handleAstronautMessage = (data) => {
    setMessage(data);
    // Parent receives the message from the child
  };

  return (
    <div>
      <h2>🚀 Mission Control</h2>

      <Astronaut
        sendMessage={handleAstronautMessage}
        // Parent gives the callback to Astronaut
      />

      <p>{message}</p>
    </div>
  );
}

function Astronaut({ sendMessage }) {
  return (
    <button
      onClick={() => sendMessage("🛰️ We reached the Moon!")}
      // Child calls the parent's function
    >
      Send Mission Update
    </button>
  );
}
```

### What is happening?

`MissionControl` owns the **mission message state**.

`Astronaut` receives a function called `sendMessage`.

When the astronaut clicks the button, it calls that function with:

```
"We reached the Moon!"
```

Mission Control receives it and updates the message.

🚀 **That's the child-to-parent communication pattern in action.**

---

## ⚡ Quick Revision

- `useState` → stores form values or other changing data.
- `onChange` → responds to input changes.
- `onSubmit` → responds to form submission.
- `pattern.test(value)` → checks whether a value matches a pattern.
- **Child → Parent** → child calls a callback function received as a prop.
- **Parent event handler** → parent can give an event-handling function to a child.
- `props.children` → contains content placed inside a component's tags.