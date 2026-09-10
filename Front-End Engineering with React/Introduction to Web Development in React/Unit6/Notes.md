# Props & Previous State

## 1.) Props

**Props = data passed from a parent component to a child component.**

**Props** are just like **SUITCASE**.

Props are **read-only**. The child can use the data, but should not directly change it.

### Example :

```js
function Child(props) {
  return <p>{props.message}</p>;
}

function Parent() {
  return <Child message="Hello!" />;
}
```

Here, `message` is passed to `Child` as a prop.

```js
props.message
```

is used to access it inside the child.

### Remember

**Props are read-only.**

---

# 2.) Previous State

The important rule for this lesson:

> **If the new state depends on the old state, use the previous-state form.**

```js
setCount(prevCount => prevCount + 1);
```

Here:

- `prevCount` = the previous value of `count`
    
- React gives us `prevCount`
    
- We return the new value
    

You can think of `prevCount` simply as:

**"Give me the value that was there before this update."**

### Golden Rule

**New state depends on old state → use `prev`.**

---

## Normal Update vs Previous State

### Normal update

```js
setCount(count + 1);
```

This uses the `count` value currently available in the function.

### Previous-state update

```js
setCount(prevCount => prevCount + 1);
```

This tells React:

> Take the latest previous value and add `1`.

For state updates that depend on the previous value, this is the safer approach.

---

# ⚠️ Why Do We Need `prev`?

React may **batch state updates** together.

For example:

```js
setCount(count + 1);
setCount(count + 1);
```

You might expect:

```text
count + 2
```

But both updates can use the same old `count` value.

So instead, use:

```js
setCount(prevCount => prevCount + 1);
setCount(prevCount => prevCount + 1);
```

Now each update receives the correct previous value.

So the count increases by **2**.

### Remember

`prevCount` is provided by React.

You don't have to create it yourself.

---

#  Conditional State Update

We can also use `prev` when the update depends on a condition.

For example, increase the count only while it is below `10`:

```js
setCount(prevCount => {
  if (prevCount < 10) {
    return prevCount + 1;
  }

  return prevCount;
});
```

### What's happening?

If:

```js
prevCount < 10
```

increase the count.

Otherwise:

```js
return prevCount;
```

Keep the value unchanged.

---

# 🚀 Example: Fuel Level

Suppose fuel should decrease by `10`, but should not decrease once it reaches `20`.

```js
setFuelLevel(prevFuelLevel => {
  if (prevFuelLevel > 20) {
    return prevFuelLevel - 10;
  }

  return prevFuelLevel;
});
```

If the fuel is:

```text
100 → 90 → 80 → ... → 30 → 20
```

Once it reaches `20`, it stays there.

The important part is that we use:

```js
prevFuelLevel
```

because the new fuel level depends on the previous fuel level.

---

# 🧮 Calculating a New Value From Previous State

Sometimes we need to calculate the new value first and then use it for another decision.

```js
setFuelLevel(prevFuelLevel => {
  const newFuelLevel =
    prevFuelLevel > 20
      ? prevFuelLevel - 10
      : prevFuelLevel;

  return newFuelLevel;
});
```

Here:

```js
const newFuelLevel = ...
```

stores the value after the update.

This is useful when **another condition depends on the new value**.

---

# 🚀 Example: Spaceship Dashboard

Suppose we have two pieces of state:

```js
const [fuelLevel, setFuelLevel] = useState(100);
const [hullIntegrity, setHullIntegrity] = useState("Stable");
```

Rules:

- Fuel decreases by `10`.
    
- Fuel should not go below the limit.
    
- If the **new fuel level** becomes `20` or less, hull integrity becomes `"Critical"`.
    

For example:

```text
Old fuel = 30
```

After decreasing:

```text
New fuel = 20
```

Now we can check:

```js
newFuelLevel <= 20
```

and change the hull status.

### Important

If you need to make another decision based on the **new state value**, calculate that new value first.

---

# 🚛 Example: Jettison Cargo

Suppose cargo starts at `100`.

Every click removes `30`.

```text
100 → 70 → 40 → 10 → 0
```

If the previous weight is already below `30`, set it directly to `0`.

```js
setCargoWeight(prevWeight => {
  const newWeight =
    prevWeight < 30
      ? 0
      : prevWeight - 30;

  return newWeight;
});
```

The important idea is:

```js
prevWeight < 30
```

If there isn't enough cargo left to remove another `30`, return `0`.

---

# ⛽ Example: Fuel Tank

Suppose fuel starts at `5`.

It should decrease until it reaches `0`.

```text
5 → 4 → 3 → 2 → 1 → 0
```

But it should never become:

```text
-1 ❌
```

So the update needs to check the previous fuel value before decreasing it.

```js
setFuelLevel(prevFuelLevel => {
  if (prevFuelLevel > 0) {
    return prevFuelLevel - 1;
  }

  return prevFuelLevel;
});
```

Once the value reaches `0`, it stays `0`.

---

# 🎮 Props + Functions

Props don't have to contain only strings or numbers.

A parent can also pass a **function as a prop**.

```js
function ControlPanel(props) {
  return (
    <button onClick={props.launch}>
      Launch
    </button>
  );
}

function Spaceship() {
  const launch = () => {
    alert("Launching spaceship!");
  };

  return <ControlPanel launch={launch} />;
}
```

The important part is:

```js
<ControlPanel launch={launch} />
```

This passes the `launch` function to `ControlPanel`.

Then:

```js
onClick={props.launch}
```

tells React to run that function when the button is clicked.

### Remember

A prop can contain a **function**, not just data like strings and numbers.

---

# 🧠 Quick Revision

|Concept|Meaning|
|---|---|
|`props`|Data passed from parent to child|
|`useState()`|Creates state|
|State setter|Updates state|
|`prev`|Previous state value|
|`setCount(prev => ...)`|Update based on previous state|
|`return prev`|Keep the old value|
|`return prev + 1`|Increase by 1|
|`return prev - 1`|Decrease by 1|

## Golden Rule

> **If the new state depends on the old state → use `prev`.**

---

# 🎯 After This Lesson, I Should Be Able To

- Explain what **props** are.
    
- Understand that props are **read-only**.
    
- Explain what **previous state** means.
    
- Use `prev` when a new state depends on the old state.
    
- Write conditional state updates using `prev`.
    
- Understand why multiple state updates can cause unexpected results.
    
- Calculate a new state value before using it in another condition.
    
- Pass a **function as a prop**.