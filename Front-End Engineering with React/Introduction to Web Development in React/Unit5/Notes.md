# State **usestate** & **setstate** in React

## 1.) **State**

In React, "state" refers to the data of a component that can be monitored and altered. Picture a simple counter: the `count` value is our state data. When someone clicks the increment button, the `count` changes, which triggers a re-render.

```js
import React, { useState } from 'react';

function Counter()  {
  const [count, setCount] = useState(0); // Declare 'count' state variable with initial value 0 

  return (
    <div>
      <p>You clicked {count} times</p> {/* Display the current count */}
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button> // Increment count when button is clicked
    </div>
  );
}

export default Counter;
```

In this code snippet, our state variable `count` is declared and initialized with `0` using `useState(0)`. When the button is clicked, the `count` increases, initiating a re-render of the component.

##  2.) **useState**

`useState` is a React hook that introduces state to functional components. `useState` is used to declare a state variable. It returns a pair: the current state value and a function to update it.

```js
const [myState, setMyState] = useState(initialState);
```

Let's declare a state variable `color` with an initial state:

```js
import React, { useState } from 'react';

function ColorChanger() {
  const [color, setColor] = useState("red"); // Declare 'color' state variable with initial value 'red'

  return (
    <div>
      <h1>The current color is {color}</h1> {/* Output the color */}
    </div>
  );
}

export default ColorChanger;
```

## Using `useState` with Hardcoded Data

Suppose we have a `favoriteColor` component that uses `useState` to manage hardcoded data:

```js
import React, { useState } from 'react';

function FavoriteColor() {
  const [favoriteColor, setFavoriteColor] = useState("blue"); // Define 'favoriteColor' with initial (hardcoded) value 'blue'

  return (
    <div>
      <h1>My favorite color is {favoriteColor}</h1> {/* Output the favorite color */}
    </div>
  );
}

export default FavoriteColor;
```

`FavoriteColor` always displays "My favorite color is blue" — because `favoriteColor` is hardcoded as `"blue"`.

## **setState**

Do you want to shuffle colors? You can! `setState` is used to update state variables.

By using `setState`, we ask React to reassign our state variable and re-render our component.

## Using `setState` with Hardcoded Data

Let's illustrate the updating of state with hardcoded data:

```js
import React, { useState } from 'react';

function FavoriteColor() {
  const [favoriteColor, setFavoriteColor] = useState("blue");

  return (
    <div>
      <h1>My favorite color is {favoriteColor}</h1> {/* Output the favorite color */}
      <button onClick={() => setFavoriteColor("green")}> {/* Change color on button click */}
        Change Favorite Color
      </button> 
    </div>
  );
}

export default FavoriteColor;
```

In `FavoriteColor`, we've added a button. When it's clicked, `setFavoriteColor("green")` is called, changing the `favoriteColor` state to `"green"` and causing a re-render of the component.

---
## Quick Revision

- **State** is data that can change inside a React component.
- `useState()` lets functional components manage state.
- `useState()` gives us:
    - the **current state value**
    - a **setter function**

 ```js
	const [count, setCount] = useState(0);
 ```

```js
	count     → current value
	setCount  → updates the value
	0         → initial value
```

- The value passed to `useState()` is the **initial state**.
- Use the setter function to update state.
- Updating state causes React to **re-render** the component.

