# Methods, Nesting Functional Components & Passing Props

## Methods in Functional Components

A functional component is a JavaScript function that returns JSX.

We can also define **functions inside a functional component** to perform specific tasks.

```js
const Spaceship = () => {

    const startThrusters = () => {
        console.log("Thrusters are ON");
    };

    return (
        <button onClick={startThrusters}>
            Start Thrusters
        </button>
    );
};
```

Here, `startThrusters` is a function defined inside the component.

When the button is clicked:

```
onClick={startThrusters}
```

React calls the function.

> **Remember:** Functions inside components can be used to handle events or perform other logic.

---

## Using Functions to Calculate Something

Functions don't only have to handle clicks. They can also contain logic and **return a value**.

```js
const Spaceship = () => {

    const fuelStatus = () => {
        let fuelLevel = 70;

        if (fuelLevel > 50) {
            return "Fuel Status: Good";
        } else {
            return "Fuel Status: Low";
        }
    };

    return (
        <h3>{fuelStatus()}</h3>
    );
};
```

Since `fuelLevel` is `70`, the function returns:

```
Fuel Status: Good
```

We use:

```js
{fuelStatus()}
```

inside JSX because `{}` allows us to use JavaScript expressions inside JSX.

---

# Nesting Functional Components

**Nesting** means using one component inside another component.

```js
const Thruster = () => {
    return (
        <h2>Thruster is ready!</h2>
    );
};

const Spaceship = () => {
    return (
        <div>
            <h1>My Spaceship</h1>
            <Thruster />
        </div>
    );
};
```

Here:

```js
<Thruster />
```

is used inside `Spaceship`.

`Spaceship` is the **parent component** and `Thruster` is the **child component**.

---

# Passing Primitive Data Types as Props

**Props** allow a parent component to pass data to a child component.

Example:

```js
const Thruster = (props) => {
    return (
        <h2>{props.status}</h2>
    );
};

const Spaceship = () => {

    let thrusterStatus = "Thrusters are functional!";

    return (
        <Thruster status={thrusterStatus} />
    );
};
```

### What's happening?

The parent has:

```
let thrusterStatus = "Thrusters are functional!";
```

It passes that value to `Thruster`:

```
<Thruster status={thrusterStatus} />
```

The child receives it through `props`:

```
const Thruster = (props) => {
```

and accesses it using:

```
props.status
```

So the child displays:

```
Thrusters are functional!
```

> **Remember:** Props are used to pass information from a parent component to a child component.

---

# Passing Functions as Props

Props don't have to contain only data.

We can also pass a **function as a prop**.

```js
const Thruster = (props) => {

    return (
        <div>
            <p>{props.status}</p>

            <button onClick={props.startThrusters}>
                Start Thrusters
            </button>
        </div>
    );
};

const Spaceship = () => {

    let thrusterStatus = "Thrusters are functional!";

    const startThrusters = () => {
        console.log("Thrusters are ON");
    };

    return (
        <Thruster
            status={thrusterStatus}
            startThrusters={startThrusters}
        />
    );
};
```

Here, `Spaceship` creates the function:

```js
const startThrusters = () => {
    console.log("Thrusters are ON");
};
```

Then passes it to `Thruster`:

```js
<Thruster startThrusters={startThrusters} />
```

The `Thruster` component receives it through props:

```js
props.startThrusters
```

and uses it on the button:

```js
<button onClick={props.startThrusters}>
```

So when the button is clicked, the function created in `Spaceship` runs.

This is useful when a **child component needs to trigger something that is defined in the parent**.

---
# Destructuring Props

Instead of writing:

```js
const Thruster = (props) => {
    console.log(props.engineStatus);
};
```

we can use **destructuring**:

```js
const Thruster = ({ engineStatus }) => {
    console.log(engineStatus);
};
```

Both do the same thing.

### Without destructuring

```js
props.engineStatus
```

### With destructuring

```js
engineStatus
```

Destructuring simply takes the property we need out of the `props` object.

For example, if props contains:

```js
{
    engineStatus: "Running"
}
```

we can directly get:

```js
const Thruster = ({ engineStatus }) => {
```

and use:

```js
console.log(engineStatus);
```

> **Remember:** Destructuring makes components cleaner when you're using several props.

---

# Quick Revision

- A functional component can contain **functions** for logic and event handling.
- Components can be **nested** inside other components.
- **Props** pass data from **parent → child**.
- Props can contain **primitive values** like strings, numbers, and booleans.
- Props can also contain **functions**.
- A child can use a function passed by its parent.
- **Destructuring props** lets us access props directly.

```js
// Normal
const Thruster = (props) => {
    return <h2>{props.status}</h2>;
};

// Destructured
const Thruster = ({ status }) => {
    return <h2>{status}</h2>;
};
```