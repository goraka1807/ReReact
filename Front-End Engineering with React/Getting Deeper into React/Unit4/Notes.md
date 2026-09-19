# ⚛️ `useEffect` for Prop Changes in Nested Components

## 1.  `useEffect`

We already know that **props are values passed from a parent component to a child component**.

Here, we want `useEffect` to **watch a prop** and run some code whenever that prop changes.

The important part is the **dependency array**:

```js
useEffect(() => {
  // runs when fuelLevel changes
}, [fuelLevel]);
```

Because `fuelLevel` is inside `[ ]`, React watches it.

If `fuelLevel` changes, the effect runs again.

---

# 🚀 2. A Prop Changing in a Component

Let's start with just one component.

```js
import React, { useEffect } from 'react';

function Spaceship({ fuelLevel }) {
  useEffect(() => {
    console.log(`Fuel level changed to ${fuelLevel}`);
  }, [fuelLevel]);

  return <h2>Fuel: {fuelLevel}</h2>;
}
```

Here:

```js
function Spaceship({ fuelLevel })
```

means `Spaceship` **receives `fuelLevel` as a prop**.

The effect watches that prop:

```js
[fuelLevel]
```

So whenever the value of `fuelLevel` changes, the effect runs.

### Example 1

If:

```js
fuelLevel = 100
```

the effect logs:

```text
Fuel level changed to 100
```

If it later becomes:

```js
fuelLevel = 90
```

the effect runs again:

```text
Fuel level changed to 90
```

### Example 2

If it changes from:

```text
90 → 80
```

the effect runs again.

### Example 3

If it changes from:

```text
80 → 70
```

the effect runs again.

The important point is that **`fuelLevel` is the prop being watched**.

---

# 🧩 3. Nested Components — Where Do Props Come From?

Now we introduce two components:

- `Spaceship` — parent
    
- `ControlPanel` — child
    

The `Spaceship` passes `fuelLevel` to `ControlPanel`.

```js
function ControlPanel({ fuelLevel }) {
  useEffect(() => {
    console.log(`Fuel level changed to ${fuelLevel}`);
  }, [fuelLevel]);

  return <h3>Fuel: {fuelLevel}</h3>;
}

function Spaceship({ fuelLevel }) {
  return <ControlPanel fuelLevel={fuelLevel} />;
}
```

### Where does `fuelLevel` come from?

Look at this:

```js
function Spaceship({ fuelLevel }) {
```

`Spaceship` receives `fuelLevel` from **its own parent**.

Then `Spaceship` passes it to `ControlPanel`:

```js
<ControlPanel fuelLevel={fuelLevel} />
```

And `ControlPanel` receives it here:

```js
function ControlPanel({ fuelLevel }) {
```

So the same value is being passed through the component structure.

### Example 1

If `Spaceship` receives:

```js
fuelLevel = 100
```

it passes:

```js
<ControlPanel fuelLevel={100} />
```

`ControlPanel` receives `100`.

---

### Example 2

If the value becomes `90`, `Spaceship` renders again:

```js
<ControlPanel fuelLevel={90} />
```

Now `ControlPanel` receives `90`.

Its effect sees that `fuelLevel` changed and runs.

---

### Example 3

If it becomes `80`:

```js
<ControlPanel fuelLevel={80} />
```

Again, the `ControlPanel` receives the new prop and its effect runs.

### 💡 Remember

> **A component doesn't magically get a prop. The parent has to pass it.**

---

# 🛸 4. Practical Example — Spaceship Controls Its Own State

Now let's make the example realistic.

The `Spaceship` owns the `fuelLevel` state:

```js
function Spaceship() {
  const [fuelLevel, setFuelLevel] = useState(100);

  const decreaseFuel = () => {
    setFuelLevel(fuelLevel - 10);
  };

  return (
    <div>
      <ControlPanel fuelLevel={fuelLevel} />

      <button onClick={decreaseFuel}>
        Decrease Fuel
      </button>
    </div>
  );
}
```

Notice something important:

### `Spaceship` owns the state

```js
const [fuelLevel, setFuelLevel] = useState(100);
```

So `fuelLevel` starts at `100`.

Then `Spaceship` passes that state to `ControlPanel`:

```js
<ControlPanel fuelLevel={fuelLevel} />
```

The child receives it as a prop:

```js
function ControlPanel({ fuelLevel }) {
```

---

# 🔄 5. What Happens When the Button Is Clicked?

The button calls:

```js
decreaseFuel
```

which does:

```js
setFuelLevel(fuelLevel - 10);
```

Let's follow three changes.

### Example 1 — First click

Initial state:

```text
fuelLevel = 100
```

Click button:

```js
setFuelLevel(90);
```

`Spaceship` renders again and passes:

```js
<ControlPanel fuelLevel={90} />
```

`ControlPanel` receives `90`.

Its effect:

```js
useEffect(() => {
  console.log(`Fuel level changed to ${fuelLevel}`);
}, [fuelLevel]);
```

runs and logs:

```text
Fuel level changed to 90
```

---

### Example 2 — Second click

Current state:

```text
fuelLevel = 90
```

Click:

```js
setFuelLevel(80);
```

Now:

```js
<ControlPanel fuelLevel={80} />
```

The `ControlPanel` receives the new prop.

The dependency changed:

```text
90 → 80
```

So the effect runs.

---

### Example 3 — Third click

```text
80 → 70
```

`Spaceship` passes:

```js
<ControlPanel fuelLevel={70} />
```

The `ControlPanel` receives `70`, and its effect runs again.

### 🧠 Golden Rule

> **If a prop is in `useEffect`'s dependency array, the effect runs again when that prop changes.**

---

# 🎛️ 6. Multiple Child Components Using the Same Prop

We can pass the same `fuelLevel` to multiple `ControlPanel` components:

```js
function Spaceship() {
  const [fuelLevel, setFuelLevel] = useState(100);

  const decreaseFuel = () => {
    setFuelLevel(fuelLevel - 10);
  };

  return (
    <div>
      <ControlPanel fuelLevel={fuelLevel} />
      <ControlPanel fuelLevel={fuelLevel} />

      <button onClick={decreaseFuel}>
        Decrease Fuel
      </button>
    </div>
  );
}
```

Both children receive the **same value**.

### Example 1

At the beginning:

```text
Spaceship fuelLevel = 100
```

Both receive:

```js
fuelLevel={100}
```

---

### Example 2

After clicking:

```text
100 → 90
```

Both receive:

```js
fuelLevel={90}
```

Both `ControlPanel` effects detect the change.

---

### Example 3

After another click:

```text
90 → 80
```

Both receive:

```js
fuelLevel={80}
```

Again, both effects run.

So one state value in the parent can be passed to **multiple children as props**.

---

# ⛽ 7. Three Levels of Nested Components

Now we go one level deeper:

```text
Spaceship
   ↓
ControlPanel
   ↓
FuelGauge
```

The important thing is **where the prop is passed at each level**.

```js
function FuelGauge({ fuelLevel }) {
  useEffect(() => {
    console.log(`Fuel Gauge: ${fuelLevel}`);
  }, [fuelLevel]);

  return <p>Fuel: {fuelLevel}</p>;
}

function ControlPanel({ fuelLevel }) {
  useEffect(() => {
    console.log(`Control Panel: ${fuelLevel}`);
  }, [fuelLevel]);

  return <FuelGauge fuelLevel={fuelLevel} />;
}

function Spaceship() {
  const [fuelLevel, setFuelLevel] = useState(100);

  const decreaseFuel = () => {
    setFuelLevel(fuelLevel - 10);
  };

  return (
    <div>
      <ControlPanel fuelLevel={fuelLevel} />

      <button onClick={decreaseFuel}>
        Decrease Fuel
      </button>
    </div>
  );
}
```

Let's break down the important parts.

### `Spaceship` owns the state

```js
const [fuelLevel, setFuelLevel] = useState(100);
```

### `Spaceship` passes the prop to `ControlPanel`

```js
<ControlPanel fuelLevel={fuelLevel} />
```

### `ControlPanel` receives the prop

```js
function ControlPanel({ fuelLevel }) {
```

Then `ControlPanel` passes it further to `FuelGauge`:

```js
<FuelGauge fuelLevel={fuelLevel} />
```

### `FuelGauge` receives it

```js
function FuelGauge({ fuelLevel }) {
```

So the prop is explicitly passed at **each level**.

---

# 🔄 8. What Happens When `fuelLevel` Changes?

Suppose the initial value is:

```text
100
```

The values are:

```text
Spaceship state = 100
ControlPanel prop = 100
FuelGauge prop = 100
```

Now click **Decrease Fuel**.

### Example 1 — `100 → 90`

`Spaceship` updates:

```js
setFuelLevel(90);
```

Then:

```js
<ControlPanel fuelLevel={90} />
```

`ControlPanel` receives `90` and passes it:

```js
<FuelGauge fuelLevel={90} />
```

`FuelGauge` receives `90`.

Both effects see their `fuelLevel` dependency change and run.

---

### Example 2 — `90 → 80`

The same thing happens:

```text
Spaceship state = 80
ControlPanel prop = 80
FuelGauge prop = 80
```

Both effects run because their `fuelLevel` prop changed.

---

### Example 3 — `80 → 70`

Again:

```text
Spaceship state = 70
ControlPanel prop = 70
FuelGauge prop = 70
```

Both components receive the updated prop and their effects run.

---

#  The Important Picture

For this lesson, remember the relationship:

```js
function Spaceship() {
  const [fuelLevel, setFuelLevel] = useState(100);

  return (
    <ControlPanel fuelLevel={fuelLevel} />
  );
}
```

`Spaceship` **owns the state**.

```js
function ControlPanel({ fuelLevel }) {
  return <FuelGauge fuelLevel={fuelLevel} />;
}
```

`ControlPanel` **receives the prop and passes it onward**.

```js
function FuelGauge({ fuelLevel }) {
  useEffect(() => {
    // React watches fuelLevel
  }, [fuelLevel]);
}
```

`FuelGauge` **receives the prop and watches it with `useEffect`**.

### 💡 Remember

> **State can live in the parent, travel through props to nested children, and each child can use `useEffect([prop])` to react when that prop changes.**

# 🏆 Golden Rule

> **`useEffect(..., [fuelLevel])` means: “Run this effect when `fuelLevel` changes.”**

And don't forget **where `fuelLevel` comes from**:

```text
State is created somewhere
        ↓
Parent passes it as a prop
        ↓
Child receives the prop
        ↓
Child can pass it further
        ↓
useEffect can watch that prop
```

This is the core idea of the entire lesson.