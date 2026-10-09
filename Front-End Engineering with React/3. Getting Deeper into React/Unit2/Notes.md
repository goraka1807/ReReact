# 1. Component Lifecycle

A React component basically has 3 stages:

**Mount → Update → Unmount**

- **Mount** = component appears
    
- **Update** = something changes
    
- **Unmount** = component disappears

---
# 2. Cleanup

Sometimes `useEffect` **starts something** that keeps running.

Example:

```js
window.addEventListener("resize", handleResize);
```

Now the browser keeps watching for resizing.

When the component disappears, we should stop watching:

```js
return () => {
  window.removeEventListener("resize", handleResize);
};
```

So just remember:

> 🧹 **Start something → eventually clean it up.**

Examples:

- Start timer → stop timer
    
- Add event listener → remove event listener
    
- Subscribe → unsubscribe

---

# `async` / `await`

Some JavaScript tasks take time.

For example:

```js
fetch("/api/user")
```

The server doesn't answer instantly.

So JavaScript gives you a **Promise**.

### Promise = "I'll give you the answer later."

---
## `async`

When a function does this kind of waiting, we can make it an `async` function:

```js
async function getUser() {
}
```

Here:

> **`async` = this function can handle waiting.**

---

## `await`

Inside that function:

```js
const response = await fetch("/api/user");
```

Here:

> **`await` = wait for the answer.**

So:

```js
async function getUser() {
  const response = await fetch("/api/user");
}
```

Here :

> "Ask the server for the user and wait until the answer comes back."

---

# 🚨 Why can't we do this?

```js
useEffect(async () => {
  // ...
});
```

Because `useEffect` expects its function to return:

```text
nothing
```

or:

```text
cleanup function
```

But an `async` function automatically returns a **Promise**.

So instead:

```js
useEffect(() => {

  const getUser = async () => {
    const response = await fetch("/api/user");
  };

  getUser();

}, []);
```

Here:

> "`useEffect` — start this."

> "`getUser` — you handle the async/waiting stuff."

---
#  🔑 Golden Rule

 **`async` means the function can wait, and `await` means “wait for this Promise to finish.”**

---
# Remember

| **Mount**   | Appears               |
| ----------- | --------------------- |
| **Update**  | Changes               |
| **Unmount** | Disappears            |
| **Cleanup** | Stop what you started |
| **Promise** | Answer coming later   |
| **`await`** | Wait for that answer  |

> **`async` → allows `await` inside the function.**
 
 **Don’t make the `useEffect` callback itself `async`.**  
 Put the `async` function **inside** the effect and call it.