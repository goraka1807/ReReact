# JavaScript Timers and Modules

In this lesson, we learn two useful JavaScript concepts:

1. **Timers** → `setInterval()` and `clearInterval()`
2. **Modules** → exporting and importing code between files

---

## 1) Timer
## `setInterval()`

`setInterval()` allows us to **run a function repeatedly after a fixed amount of time**.

### Syntax

```js
let intervalId = setInterval(() => {
    // code to repeat or function
}, delay)//Interval;
```

The delay is written in **milliseconds** (ms).

1 second = 1000 millisecond.

```js
let intervalId = setInterval(() => {
    console.log("Hello, World!"); //Function
}, 2000)//Interval;
```

This runs every **2 seconds**.

> `setInterval()` keeps running until we explicitly stop it.

---

## `clearInterval()`

To stop an interval, use `clearInterval()`.

```js
clearInterval(intervalId);
```

Example:

```js
let intervalId = setInterval(() => {
    console.log("Running...");
}, 1000);

clearInterval(intervalId);
```

The value returned by `setInterval()` is stored in `intervalId`, and we use that value to stop the correct timer.

### Practical Example

```js
let count = 0;

let intervalId = setInterval(() => {
    count++;

    console.log(count);

    if (count === 5) {
        clearInterval(intervalId);
    }
}, 1000);
```

This prints `1` to `5` and then stops.

---

## Handling Errors in an Interval (try and catch)

If the repeated function might throw an error, we can use `try...catch`.

```js
let intervalId = setInterval(() => {
    try {
        riskyFunction();
    } catch (error) {
        console.error(error);//function undefined
        clearInterval(intervalId);
    }
}, 2000);
```

Here:

1. `setInterval()` runs the function every 2 seconds.
2. `try` attempts to run the risky code.
3. If an error occurs, `catch` handles it.
4. `clearInterval()` stops the timer.

---

# 2) Modules

**Modules** allow us to split our JavaScript code into multiple files and share functionality between them.

There are two main types:

   A) **Named exports**
   B) **Default exports**

---

## A) Named Exports

Named exports allow us to export **multiple things** from a file, by using the name of the function as the export's name suggests.

### Export

```js
// mathFunctions.js

export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}
```

### Import

```js
import { add, multiply } from "./mathFunctions.js";

console.log(add(2, 3));       // 5
console.log(multiply(5, 2));  // 10
```

Notice the `{}`:

```js
import { add, multiply } from "./mathFunctions.js";
```

The imported names **MUST match** the named exports.

---

## B) Default Export

A module can have **just ONE default export**.

```js
// greeter.js

export default function greet() {
    return "Hello, JavaScript!";
}
```

When importing a default export, we **don't use `{}`**.

```js
import greet from "./greeter.js";

console.log(greet());
// Hello, JavaScript!
```

One useful difference is that we can choose the name when importing a default export:

```js
import sayHello from "./greeter.js";

console.log(sayHello());
```

Both `greet` and `sayHello` refer to the same default export.

**Name of the function doesn't matter when default exported.

---

# Named vs Default Export

|Named Export|Default Export|
|---|---|
|Can have multiple in one file|Only one default export per file|
|Import using `{}`|Import without `{}`|
|Import name normally matches export|Import name can be anything|
|Good for multiple related functions|Good when a module has one main purpose|

### Easy Way to Remember

```js
// Named
import { add } from "./math.js";
```

```js
// Default
import greet from "./greeter.js";
```

> **Named → `{}`**  
> **Default → no `{}`**

---

# Quick Revision

1. `setInterval()` runs code **repeatedly** after a specified delay.
2. The delay is measured in **milliseconds**.
3. `clearInterval()` stops an interval.
4. Store the interval ID so you can stop it later.
5. `try...catch` can handle errors inside repeated code.
6. **Modules** let us split JavaScript into multiple files.
7. **Named exports** use `export` and are imported with `{}`.
8. A module can have **one default export**.
9. Default exports are imported **without `{}`**.
10. A file can have multiple named exports alongside one default export.
