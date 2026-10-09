# Ternary Conditional ,Logical Operators and forEach Loop

We have **Ternary conditional operator** , logical **AND** and **OR** operators, and the _forEach loop_

---
## Ternary Condition Operator

Ternary simply means three **3 Condition**

A Ternary Operator in JavaScript serves as a shortcut for an `if-else` statement.
 It derives its name "ternary" from involving three parts:
 1. **Condition**
 2. `true` result
 3. `false` result.

If the condition holds true, you receive the first result; otherwise, you obtain the second result.

**Syntax:**

```js
let result = (condition) ? 'value if true' : 'value if false';
//Here we separate condition and output by ?
//While separate True result and False result by :
```

**Example**:

```js
let marks1 = 50;
let result1 = (marks1 > 35) ? 'Passed' : 'Try Again';
console.log(result1);

let marks2 = 20;
let result2 = (marks2 > 35) ? 'Passed' : 'Try Again';
console.log(result2)
//As marks are above 35 first condtion that is true is fulfilled so result will be Passed while result2 will be Try Again as mark2 are below 35
```

Lets consider one of the more **Complicated Example** where we use a **NESTED** ternary operator. So we want to classify a number as "negative", "positive", or "zero":

**Example:**

```js
let num1 = 5;
let desc1 = (num > 0) ? 'Positive' : (num < 0) ? 'Negative' : 'Zero'
console.log(desc1)
//Here we have 3 results TERNARY
//Num is +,- or 0
//Syntaxually 1) firstly it checks if its + then, 2) if not then it checks if it is - 3) Finally if none then the num is zero. 
//Make sure to revise this and solve its examples more and more
```

**Remember:** 
1. Condition -> **( )**
2. If **True** then -> **' IF '**** (First)
3. If **False** then -> **' ELSE '** (Second)
4. To have nested ternary operator just make sure to  **use 2nd If as 1st IF's Else**

---
# Logical AND (`&&`), OR (`||`) and `forEach()` Loop

JavaScript has logical operators like:

- `&&` → AND
- `||` → OR

We can also use them with actual values, not just `true` and `false`.

---

## Logical AND (`&&`)

`&&` requires **both conditions** to be true.

```
let age = 25;
let hasLicense = true;

let canDrive = age >= 18 && hasLicense;

console.log(canDrive); // true
```

### With Values

`&&` returns the **first falsy value**. If everything is truthy, it returns the last value.

```
let first = { name: "John" };
let second = { name: "Jane" };

console.log(first && second); // { name: "Jane" }
```

### Short-Circuiting

A useful pattern is checking something before accessing it:

```
let text;

let message = text && text.length;

console.log(message); // undefined
```

Since `text` is `undefined`, JavaScript stops before trying `text.length`.

---

## Logical OR (`||`)

`||` returns true when **at least one condition** is true.

```
let isWeekend = false;
let isHoliday = true;

let canRelax = isWeekend || isHoliday;

console.log(canRelax); // true
```

### Default Values

`||` is commonly used to provide a fallback value.

```
let username = "";

let displayName = username || "Guest";

console.log(displayName); // Guest
```

Because `username` is falsy, `"Guest"` is used.

---

## Truthy and Falsy

Common falsy values:

```
false
0
""
null
undefined
NaN
```

Most other values are truthy.

```
[]
{}
"hello"
42
```

> ⚠️ **Remember:** Empty arrays `[]` and objects `{}` are truthy.

---

## `forEach()` Loop

`forEach()` allows us to run a function **once for every item in an array**.

### Syntax

```
array.forEach((item) => {
    // code
});
```

### Basic Example

```
let fruits = ["Apple", "Banana", "Mango"];

fruits.forEach((fruit) => {
    console.log(fruit);
});
```

Output:

```
Apple
Banana
Mango
```

JavaScript takes each item and puts it into `fruit` one at a time.

---

## `forEach()` with Index

We can also get the item's index.

```
let fruits = ["Apple", "Banana", "Mango"];

fruits.forEach((fruit, index) => {
    console.log(index, fruit);
});
```

Output:

```
0 Apple
1 Banana
2 Mango
```

Syntax:

```
array.forEach((item, index) => {
    // code
});
```

- `item` → current value
- `index` → current position

---

## `forEach()` with Objects

Very common when working with API data:

```
let users = [
    { name: "John", age: 25 },
    { name: "Sarah", age: 30 },
    { name: "Mike", age: 22 }
];

users.forEach((user) => {
    console.log(user.name);
});
```

Output:

```
John
Sarah
Mike
```

---

## `forEach()` vs `map()`

This is important for React.

|`forEach()`|`map()`|
|---|---|
|Runs code for every item|Transforms every item|
|Does not return a new array|Returns a new array|
|Useful for side effects|Useful for creating new data|
|Not normally used for rendering lists|Commonly used in React|

Example:

```
let numbers = [1, 2, 3];

let result = numbers.map((number) => {
    return number * 2;
});

console.log(result); // [2, 4, 6]
```

> **React reminder:** `map()` is commonly used when rendering lists because it returns the new array of elements.

---

## Common Mistakes

### `&&` and `||` can return values

```
console.log("Hello" && "World");
// World

console.log("" || "Guest");
// Guest
```

They don't always return `true` or `false`.

### Don't expect `forEach()` to return a new array

```
let numbers = [1, 2, 3];

let result = numbers.forEach((number) => {
    return number * 2;
});

console.log(result); // undefined
```

Use `map()` when you need a new array.

---

# Quick Revision

1. `&&` means **AND**.
2. `||` means **OR**.
3. `&&` returns the first falsy value, otherwise the last value.
4. `||` returns the first truthy value, otherwise the last value.
5. `[]` and `{}` are **truthy**.
6. `&&` can be used for short-circuit checks.
7. `||` can be used for default/fallback values.
8. `forEach()` runs code for every array item.
9. `forEach()` can provide both `item` and `index`.
10. Use `map()` when you need a **new transformed array**, especially in React.