# API and Fetch API in React

An **API** lets our app **request data from another system**.

Think: **App → API → Data**

---

## API

API is like a **waiter** between our app and a server.

```text
App → API → Server
App ← API ← Data
```

---

## `fetch()`

`fetch()` is used to request data from an API.

**Syntax:**

```js
fetch('API_URL')
```

Example:

```js
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data));
```

### Remember:

- `fetch()` → asks for data
    
- `.json()` → converts response into usable JSON
    
- `data` → the actual received data
    

---

## Fetch in React

Usually:

- **`useEffect`** → fetch the data
    
- **`useState`** → store the data
    

```js
const [data, setData] = useState(null);

useEffect(() => {
  fetch('API_URL')
    .then(response => response.json())
    .then(data => setData(data));
}, []);
```

Then display it:

```jsx
<h1>{data.name}</h1>
```

---

## Loading

Data takes time to arrive, so we can show:

```jsx
if (loading) {
  return <p>Loading...</p>;
}
```

Flow:

```text
fetch()
  ↓
Loading...
  ↓
Data arrives
  ↓
Store with useState
  ↓
Display data
```

---

## Remember

**API** → gets data  
**`fetch()`** → requests data  
**`.json()`** → converts response  
**`useEffect`** → fetches data  
**`useState`** → stores data  
**Loading** → shown while waiting