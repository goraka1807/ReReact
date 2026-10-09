# Managing Complex & Split State in React

## 1. What is Complex State?

**Complex state** simply means state that contains more than one piece of information.

It can be:

* An **object**
* An **array**
* An **array of objects**

### Example 1 — Object

```js
const [user, setUser] = useState({
  name: "John",
  age: 25,
  city: "Pune"
});
```

One state variable contains multiple values.
### Example 2 — Array

```js
const [items, setItems] = useState([
  "Apple",
  "Orange",
  "Banana"
]);
```

The state is an array containing multiple values.
### Example 3 — Array of Objects

```js
const [shoppingCart, setShoppingCart] = useState([
  { name: "Apple", quantity: 3, price: 0.5 },
  { name: "Orange", quantity: 2, price: 0.75 }
]);
```

This is more complex because we have:

**array → objects → multiple properties**

---

#  2. Managing Complex State

When changing complex state, we usually create a **new copy** instead of directly changing the existing state.

This is where the **spread operator `...`** becomes useful.

### Example 1 — Add an item to an array

```js
const [items, setItems] = useState(["Apple", "Orange"]);

setItems([...items, "Banana"]);
```

The old array:

```text
["Apple", "Orange"]
```

becomes:

```text
["Apple", "Orange", "Banana"]
```
### Example 2 — Add an object

```js
const [cart, setCart] = useState([
  { name: "Apple", quantity: 3 }
]);

setCart([
  ...cart,
  { name: "Banana", quantity: 1 }
]);
```

`...cart` keeps the existing items and the new object is added.
### Example 3 — Updating an object

```js
const [user, setUser] = useState({
  name: "John",
  age: 25
});

setUser({
  ...user,
  age: 26
});
```

`...user` keeps:

```js
name: "John"
```

and we replace:

```js
age: 25
```

with:

```js
age: 26
```

### Golden Rule

> **Don't directly modify React state. Create a new version of it and give that new version to the setter.**

---

# ✂️ 3. What Does "Splitting State" Mean?

Suppose you have:

```js
const [user, setUser] = useState({
  name: "John",
  age: 25,
  city: "Pune"
});
```

Here, **name, age and city are all inside one state object**.

Sometimes it makes more sense to keep them separate:

```js
const [name, setName] = useState("John");
const [age, setAge] = useState(25);
const [city, setCity] = useState("Pune");
```

Now each value has its **own state**.

That's **splitting state**

### Example 1 — User information

Instead of:

```js
const [user, setUser] = useState({
  name: "John",
  age: 25
});
```

You can have:

```js
const [name, setName] = useState("John");
const [age, setAge] = useState(25);
```

### Example 2 — Form fields

```js
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [email, setEmail] = useState("");
```

Each input gets its own state.

### Example 3 — Product information

```js
const [productName, setProductName] = useState("Laptop");
const [quantity, setQuantity] = useState(1);
const [price, setPrice] = useState(1000);
```

Each value can now change independently.

### 💡 Remember

> **Split state when different pieces of information need to change independently.**

You will have more `useState()` calls, but each piece becomes easier to manage.

---

# 🛒 4. Splitting State in a Shopping Cart Form

Imagine a form where the user enters:

* Item name
* Quantity
* Price

Instead of putting all three into one state object:

```js
const [item, setItem] = useState({
  name: "",
  quantity: 1,
  price: 1
});
```

we can split them:

```js
const [itemName, setItemName] = useState("");
const [itemQuantity, setItemQuantity] = useState(1);
const [itemPrice, setItemPrice] = useState(1);
```

Now each field has its own state.

### Example 1 — Item name

```js
function handleNameChange(event) {
  setItemName(event.target.value);
}
```

Whatever the user types becomes `itemName`.

### Example 2 — Quantity

```js
function handleQuantityChange(event) {
  setItemQuantity(event.target.value);
}
```

The quantity state gets updated when the user changes the input.

### Example 3 — Price

```js
function handlePriceChange(event) {
  setItemPrice(event.target.value);
}
```

Same idea for price.

---

# 📝 5. Capturing Form Data

This is an important React pattern.

A form input can be connected directly to state.

The basic pattern is:

```js
const [value, setValue] = useState("");

function handleChange(event) {
  setValue(event.target.value);
}
```

Then:

```js
<input
  value={value}
  onChange={handleChange}
/>
```

There are **three important pieces** here:

### `value`

```js
value={value}
```

The input displays whatever is currently stored in state.

### `onChange`

```js
onChange={handleChange}
```

Runs whenever the user types.

### `event.target.value`

```js
event.target.value
```

Gets what the user just typed.

---

# 👤 6. Form Example — First & Last Name

### Example 1 — State

```js
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
```

### Example 2 — Change handlers

```js
function handleFirstNameChange(event) {
  setFirstName(event.target.value);
}

function handleLastNameChange(event) {
  setLastName(event.target.value);
}
```

### Example 3 — Inputs

```js
<form>
  <label>
    First Name:
    <input
      type="text"
      value={firstName}
      onChange={handleFirstNameChange}
    />
  </label>

  <label>
    Last Name:
    <input
      type="text"
      value={lastName}
      onChange={handleLastNameChange}
    />
  </label>
</form>
```

Now the flow is simple:

**User types → `onChange` runs → state updates → input shows new state**

---

# 🔄 7. Adding the Form Data to the Cart

Once we have:

```js
const [itemName, setItemName] = useState("");
const [itemQuantity, setItemQuantity] = useState(1);
const [itemPrice, setItemPrice] = useState(1);
```

we can create an item when the form is submitted.

```js
function addItem(event) {
  event.preventDefault();

  setShoppingCart([
    ...shoppingCart,
    {
      name: itemName,
      quantity: itemQuantity,
      price: itemPrice
    }
  ]);
}
```

### What's happening?

`...shoppingCart`

Keeps the existing cart items.

Then:

```js
{
  name: itemName,
  quantity: itemQuantity,
  price: itemPrice
}
```

creates the new item.

---

# 🧹 8. Resetting the Form

After adding the item, we don't want the old values to remain in the form.

So we reset them:

```js
setItemName("");
setItemQuantity(1);
setItemPrice(1);
```

### Complete example

```js
function addItem(event) {
  event.preventDefault();

  setShoppingCart([
    ...shoppingCart,
    {
      name: itemName,
      quantity: itemQuantity,
      price: itemPrice
    }
  ]);

  setItemName("");
  setItemQuantity(1);
  setItemPrice(1);
}
```

### Example 1 — Reset text

```js
setItemName("");
```

### Example 2 — Reset quantity

```js
setItemQuantity(1);
```

### Example 3 — Reset price

```js
setItemPrice(1);
```

So after submission, the form returns to its starting values.

---

# Golden Rule — Complex State

> **When updating an object or array in React, don't mutate the old state. Create a new copy and update that copy.**

For arrays:

```js
setItems([...items, newItem]);
```

For objects:

```js
setUser({
  ...user,
  age: 26
});
```

---

# 💡 Remember — Splitting State

> **One `useState` = one independently managed piece of state.**

If values are naturally connected and usually change together, keeping them together can make sense.

If they change independently, **splitting them can make the code easier to manage.**

---

# ⚛️ Quick Revision

| Concept                  | Remember                                           |
| ------------------------ | -------------------------------------------------- |
| Complex state            | State containing multiple pieces of data           |
| Object state             | Use `{ ...state }` when creating an updated object |
| Array state              | Use `[...state]` when creating an updated array    |
| Splitting state          | Give independent values their own `useState()`     |
| `value`                  | Shows the current state in an input                |
| `onChange`               | Detects user input                                 |
| `event.target.value`     | Gets the typed value                               |
| `event.preventDefault()` | Prevents the form's normal page refresh            |
| Resetting                | Set state back to its initial values               |

# Remember 

* Understand what **complex state** means.
* Update arrays and objects without directly modifying them.
* Understand why the **spread operator `...`** is useful with state.
* Know when state can be **split into multiple `useState()` calls**.
* Connect form inputs to React state.
* Use `value`, `onChange`, and `event.target.value`.
* Add form data to an existing array state.
* Reset form fields after submission.