# Axios in React

# 📬 What Is Axios?

**Axios** is a JavaScript library used to make **HTTP requests**.

For example, your React app might ask:

> "Hey server, give me the latest posts."

Axios sends that request and brings the response back.

### 🛵 Axios as a Delivery Person

Imagine a restaurant:

- **React app** → Customer
- **Axios** → Delivery person
- **Server/API** → Restaurant
- **Request** → Your order
- **Response** → Your food

React doesn't need to handle all the communication details itself. Axios helps send the request and bring the response back.

Axios commonly works with **Promises** and **`async/await`**.

---
## Where  is Axios used ?

**Axios** is useful whenever your React application needs to communicate with a **server/API**.

Common examples:

- 📰 Fetch posts, news, or articles
- 👤 Get user/profile data
- 🛒 Load products from an online store
- 📝 Send form data to a server
- 🔐 Send login/signup requests
- ✏️ Update or delete existing data

Think of Axios as a **messenger between your React app and the server**.

---

#  Installing Axios

In a normal React project, install Axios with npm (Node Package Manager):

```bash
npm install axios
```

Then import it:

```js
import axios from "axios";
// Gives the component access to Axios
```

---

# ⚛️ Using Axios in a React Component

Axios can be used inside `useEffect` when we want to fetch data when the component loads.

```js
function Posts() {
  const [posts, setPosts] = useState([]);
  // Stores the posts received from the API

  useEffect(() => {
    async function fetchData() {
      const response = await axios.get(
        "https://api-regional.codesignalcontent.com/posting-application-2/posts/"
      );
      // Axios sends a GET request to the API

      setPosts(response.data);
      // response.data contains the API's data
    }

    fetchData();
    // Run the async function
  }, []);

  return (
    <div>
      {posts.map((post) => (
        <div key={post.id}>
          <h3>{post.text}</h3>
          <p>Likes: {post.likesCount}</p>
        </div>
      ))}
    </div>
  );
}
```

### Important Axios difference

With **Fetch**, we normally need:

```js
const response = await fetch(url);
const data = await response.json();
```

With **Axios**:

```js
const response = await axios.get(url);
const data = response.data;
```

Axios automatically handles the JSON transformation for us.

---

# Error Handling with Axios

When using `async/await`, use `try/catch` to handle errors.

```js
useEffect(() => {
  async function fetchData() {
    try {
      const response = await axios.get("/posts/");
      setPosts(response.data);
    } catch (error) {
      console.log(error);
      // Handle the failed request here
    }
  }

  fetchData();
}, []);
```

### Why `try/catch`?

If the request fails, execution moves to the `catch` block instead of breaking the component's request logic.

---

# 🆚 Axios vs Fetch API

Both can communicate with APIs, but they handle some things differently.

| Feature                   | Fetch API                | Axios                |
| ------------------------- | ------------------------ | -------------------- |
| Built into browser        | ✅ Yes                    | ❌ No, install it     |
| GET requests              | ✅                        | ✅                    |
| POST requests             | ✅                        | ✅                    |
| JSON handling             | Manual `response.json()` | Automatic            |
| Error handling            | More manual              | More straightforward |
| `async/await`             | ✅                        | ✅                    |
| Request cancellation      | Supported                | Supported            |
| CSRF protection           | ❌ Not built-in           | ✅ Built-in           |
| Streams / service workers | ✅ Useful                 | Less suited          |

### 🧠 Easy way to remember

**Fetch** = built into JavaScript/browser and gives you more direct control.

**Axios** = extra library that makes common HTTP communication more convenient.

Neither automatically makes every API task better; the choice depends on what your application needs.

---

# ♻️ Reusable Axios Instances

Imagine your application talks to the **same server** many times.

Instead of repeatedly writing the complete base URL:

```js
axios.get(
  "https://api-regional.codesignalcontent.com/posting-application-2/posts/"
);
```

we can create an Axios instance with a **base URL**.

```js
const instance = axios.create({
  baseURL:
    "https://api-regional.codesignalcontent.com/posting-application-2",
});
```

Now requests can simply use:

```js
instance.get("/posts/");
```

### In a React component

```js
function Posts() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    async function fetchData() {
      const response = await instance.get("/posts/");
      // Uses the reusable Axios instance

      setPosts(response.data);
      // Store the returned data
    }

    fetchData();
  }, []);

  return (
    <div>
      {posts.map((post) => (
        <div key={post.id}>
          <h3>{post.text}</h3>
          <p>Likes: {post.likesCount}</p>
        </div>
      ))}
    </div>
  );
}
```

### 💡 Why use an instance?

If many components communicate with the same API, a reusable instance keeps the **base configuration in one place**.

This makes larger applications cleaner and easier to maintain.

---

# 🧠 Golden Rule

> **Axios is an HTTP client that helps your React app communicate with APIs.**

Remember the basic pattern:

```js 
const response = await axios.get(url);
const data = response.data;
```

And for repeated API configuration:

```js
const instance = axios.create({
  baseURL: "https://example.com",
});
```

---

# ⚡ Quick Revision

- **Axios** → JavaScript HTTP client.
- `npm install axios` → installs Axios.
- `axios.get()` → makes a GET request.
- `response.data` → contains the returned data.
- `try/catch` → handles errors with `async/await`.
- **Axios instance** → reusable API configuration.
- **Fetch** → built into the browser.
- **Axios** → extra library with convenient HTTP features.