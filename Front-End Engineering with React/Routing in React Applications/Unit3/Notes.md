# 🔗 NavLink in React Router

## 🚪 What is `NavLink`?

`NavLink` is used to **move between routes**, just like `Link`.

The main difference is that `NavLink` can tell us when its route is **currently active**.

This is useful for navigation menus where we want to highlight the page the user is currently viewing.

```
import { BrowserRouter as Router, NavLink } from "react-router-dom";

function Nav() {
  return (
    <Router>
      <NavLink to="/home">Home</NavLink>
      <NavLink to="/about">About</NavLink>
    </Router>
  );
}
```

---

## 🆚 `Link` vs `NavLink`

|Feature|`Link`|`NavLink`|
|---|---|---|
|Navigate to another route|✅|✅|
|Uses `to`|✅|✅|
|Knows if it is active|❌|✅|
|Can style active link|❌|✅|
|Best for|General navigation|Navigation menus|

### 🧠 Simple Rule

> **`Link` = Navigate**  
> **`NavLink` = Navigate + show which route is active**

---

## ✨ Active `NavLink`

A `NavLink` can change its appearance when its route is active.

For example, if we're currently on `/about`, the About link can look different.

```
<NavLink
  to="/about"
  style={({ isActive }) => ({
    color: isActive ? "orange" : "blue"
  })}
>
  About
</NavLink>
```

Here:

- `isActive` is `true` → the link is active.
- `isActive` is `false` → the link is not active.

---

## 🎨 Styling `NavLink`

We can give a `NavLink` a CSS class.

```
<NavLink
  to="/home"
  className="nav-link"
>
  Home
</NavLink>
```

Then CSS can style the link:

```
.nav-link {
  color: blue;
}

.active {
  color: orange;
}
```

---

## 🎯 Dynamic Styling with `style`

The `style` attribute can use `isActive` to change the style.

```
<NavLink
  to="/dashboard"
  style={({ isActive }) => ({
    color: isActive ? "orange" : "blue"
  })}
>
  Dashboard
</NavLink>
```

The important part is:

```
isActive ? "orange" : "blue"
```

This means:

- Active → orange
- Not active → blue

---

# 🛍️ Example 1: Online Store

`NavLink` is useful for an online store where users move between different sections.

```
function StoreNav() {
  return (
    <nav>
      <NavLink to="/shop">Shop</NavLink>
      <NavLink to="/cart">Cart</NavLink>
      <NavLink to="/orders">Orders</NavLink>
    </nav>
  );
}
```

If the user is on `/cart`, the **Cart** link can be styled differently.

This helps the user know **which section they are currently viewing**.

---

# 📚 Example 2: Learning Website

A learning website might have sections for lessons, notes, and progress.

```
function CourseNav() {
  return (
    <nav>
      <NavLink to="/lessons">Lessons</NavLink>
      <NavLink to="/notes">Notes</NavLink>
      <NavLink to="/progress">Progress</NavLink>
    </nav>
  );
}
```

We can use `isActive` to highlight the current section:

```
<NavLink
  to="/progress"
  style={({ isActive }) => ({
    fontWeight: isActive ? "bold" : "normal"
  })}
>
  Progress
</NavLink>
```

When the user is on `/progress`, **Progress** becomes bold.

---

# 🧭 Important `NavLink` Attributes

|Attribute|What it does|Example|
|---|---|---|
|**`to`**|Sets the destination route|`to="/home"`|
|**`replace`**|Replaces the current history entry|`replace`|
|**`exact`**|Matches only the exact route|`exact`|
|**`className`**|Adds a CSS class|`className="nav-link"`|
|**`style`**|Applies styles|`style={({ isActive }) => (...)}`|

### `to`

Tells `NavLink` where to go.

```
<NavLink to="/mars">Go to Mars</NavLink>
```

### `replace`

Replaces the current browser history entry instead of adding a new one.

```
<NavLink to="/home" replace>
  Home
</NavLink>
```

### `exact`

Makes the link active only when the URL matches exactly.

```
<NavLink to="/home" exact>
  Home
</NavLink>
```

---

# 🚀 Example 3: Spaceship Navigation

We can use `NavLink` to move between different parts of a spaceship:

```
function Spaceship() {
  return (
    <Router>
      <NavLink to="/cockpit">
        Cockpit
      </NavLink>

      <NavLink to="/engine-room">
        Engine Room
      </NavLink>
    </Router>
  );
}
```

The user can move between:

```
/cockpit
/engine-room
```

The active room can be styled differently so the user knows **where they currently are**.

---

# 🧠 Quick Revision

|Concept|Remember|
|---|---|
|**`Link`**|Navigate to another route|
|**`NavLink`**|Navigate + know when active|
|**`to`**|Destination|
|**`isActive`**|Tells whether the link is active|
|**`className`**|Add CSS styling|
|**`style`**|Dynamically change styling|
|**`replace`**|Replace history entry|
|**`exact`**|Match the exact route|

### ⭐ Golden Rule

> **Use `Link` for simple navigation. Use `NavLink` when you want to show which route is currently active.**