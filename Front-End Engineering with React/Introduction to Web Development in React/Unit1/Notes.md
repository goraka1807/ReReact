# Introduction to Web Development in React

React is a **JavaScript library** used to build user interfaces.

We will learn :
- **Components**
- **JSX**
- **DOM rendering**
- **Virtual DOM**
- Basic React project structure

---

## React Components

A **component** is a reusable piece of UI.

A functional component is simply a JavaScript function that returns JSX.

```js
function WelcomeMessage() {
    return (
        <div>
            Welcome to React!
        </div>
    );
}

export default WelcomeMessage;
```

We can then use it like an HTML element:

```html
<WelcomeMessage />
```

### Remember

Think of a component as a **reusable LEGO Piece that fits in a larger lego piece too** of a React application.

---

## JSX

**JSX** lets us write HTML-like syntax inside JavaScript.

```js
const element = <h1>Hello, World!</h1>;
```

It looks like HTML, but JSX is actually part of our JavaScript code and gets transformed into JavaScript.

JSX is what allows React components to describe **what the UI should look like**.

JSX returns **Directly to HTML** body tag.

---

# Rendering React Components

**Rendering** means taking a React component and putting its output into the **DOM**.

A typical React app has a root element in the HTML:

React uses this element as the starting point for the application.

### Rendering a Component

```js
import ReactDOM from "react-dom/client";
import WelcomeMessage from "./WelcomeMessage";

const root = ReactDOM.createRoot(
    document.getElementById("root")
);

root.render(<WelcomeMessage />);
```

> Render the `WelcomeMessage` component inside this root.

---

# Virtual DOM

The **Virtual DOM** is a lightweight representation of the real DOM that React uses to efficiently update the UI.

Instead of unnecessarily changing the entire real DOM, React:

1. Creates/updates its virtual representation.
2. Figures out what actually changed.
3. Updates the necessary parts of the real DOM.

You don't normally manipulate the Virtual DOM yourself — **React handles it for you**.

> **Remember:** Virtual DOM is mainly about how React efficiently updates the UI. You don't directly work with it.

---
# Creating a New React Project

 Steps to create a new react project :
 1. Install Node.js
 2. Create a Vite framework
 3. Name and select other options to create a react project

## 1.Install Node.js

#### Before starting, you must have **Node.js** installed on your computer. 

1.) Download and install Node.js from its official website : https://nodejs.org/en

2.) After completing the install. Open the Visual Studio Code and open Terminal by **backtick** or **grave accent**   in the bottom.

3.) Then make sure the installation is complete, by typing this commands in the terminal:

```bash
node -v
npm -v
```

 4.) Create a new **Vite** Framework, by these commands in the terminal :

```bash
npm create vite
```

5.) Name your project and select other options like React ,JavaScript, etc. to complete the react project creation

---
# Basic React Project Structure

A React project commonly looks something like this, when we create it using **VITE**:

```
my-app/
├── package.json
├── node_modules/
├── public/
└── src/
    ├── App.jsx
    ├── main.jsx
    ├── App.css
    └── index.css
```

The exact structure can vary depending on the tool being used.

### Important Files

**`package.json`**

Contains project information and dependencies.

**`node_modules/`**

Contains installed packages.

**`public/`**

Contains static files that don't need to be processed as React components.

**`src/`**

Contains most of the application code.

**`App.jsx`**

Usually contains the main `App` component.

**`main.jsx`**

Usually where the React application is connected to the DOM and the root component is rendered.

---

# Comments in JSX

Normal JavaScript comments:

```js
// Single line

/*
Multi-line
comment
*/
```

Inside JSX, use curly braces:

```js
<div>
    Welcome!
    {/* This is a JSX comment */}
</div>
```

You won't use JSX comments constantly, but remember the syntax:

```js
{/* comment */}
```

---

# Quick Revision

1. **React** is a JavaScript library for building user interfaces.
2. A **component** is a reusable piece of UI.
3. A functional component is a function that returns JSX.
4. **JSX** lets us write HTML-like syntax inside JavaScript.
5. **Rendering** means putting React's UI into the DOM.
6. `createRoot()` creates the React root connected to a DOM element.
7. `root.render()` renders a component into that root.
8. The **Virtual DOM** helps React efficiently update the real DOM.
9. `src/` usually contains most of our React application code.
10. JSX comments use `{/* comment */}`.