# Custom Hooks and Async API Calls in React

We use **asynchronous API calls** to get data without stopping our app.

We use **custom hooks** to reuse React logic in different components.

---

## Async API Calls

Async means:

> Start something, continue other work, and get the result later.

Example:

```
fetch('API_URL')
  .then(response => response.json())
  .then(data => console.log(data));
```

Flow:

```
fetch()
 ↓
Wait for response
 ↓
Convert to JSON
 ↓
Use data
```

---

# Custom Hooks

A **custom hook** is a reusable function that contains React logic.

Instead of writing the same `useState` + `useEffect` code again and again, we create a hook once and reuse it.

Example:

```
useFetchSpaceships()
```

Remember:

> Components reuse UI.  
> Custom hooks reuse logic.

---

## Creating a Custom Hook

```
const useFetchSpaceships = (url) => {

  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(response => response.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
};
```

---

## Understanding the Hook

### 1. Accept URL

```
const useFetchSpaceships = (url)
```

The hook receives the API URL.

---

### 2. Store Data

```
const [data, setData] = useState(null);
```

Stores the API response.

---

### 3. Loading State

```
const [loading, setLoading] = useState(true);
```

Tracks if data is still loading.

---

### 4. Fetch Data

```
useEffect(() => {
  fetch(url)
}, [url])
```

Runs the API call.

`[url]` means:

- Run when component loads
- Run again if URL changes

---

### 5. Return Values

```
return { data, loading };
```

Now other components can use this data.

---

# Using Custom Hook

```
const MarsSpaceships = () => {

const {data: spaceships, loading} =
useFetchSpaceships('API_URL');

return loading 
? <div>Loading...</div>
: <div>{spaceships.map(ship => <p>{ship.name}</p>)}</div>

}
```

---

## Important Concept: Destructuring

```
const {data: spaceships, loading} = useFetchSpaceships();
```

Means:

Take `data` and rename it to `spaceships`.

Same as:

```
const spaceships = result.data;
const loading = result.loading;
```

---

# Remember

1. **Async API call** → gets data later.
2. `fetch()` → requests API data.
3. `useState` → stores data and loading status.
4. `useEffect` → performs API calls.
5. **Custom hooks** → reusable React logic.
6. Return values from hooks and use them anywhere.

---

## Quick Revision

- API calls are asynchronous because data arrives later.
- Custom hooks help avoid repeating `useState` + `useEffect` code.
- A custom hook starts with `use`.
- Custom hooks can accept parameters like URLs.
- Custom hooks can return data to components.