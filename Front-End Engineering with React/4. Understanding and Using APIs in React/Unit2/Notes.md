# 🚨 Handling API Errors & Retries in React

When working with APIs, requests don't always succeed. The internet may disconnect, the server may be down, or the server may return an error response.

React can handle these situations by keeping track of the **error state** and showing a **fallback UI**.

---

## 🚨 Handling API Errors

Create an `error` state alongside your API data.

```js
function App() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  // data stores the API result
  // error stores any error that occurs

  useEffect(() => {
    fetch("https://api-regional.codesignalcontent.com/posting-application-2/posts/")
      .then((response) => {
        // Check whether the HTTP response was successful
        if (!response.ok) {
          throw Error(response.statusText);
        }

        return response.json();
        // Convert successful response into JSON
      })
      .then((data) => setData(data))
      // Store the fetched data

      .catch((error) => setError(error));
      // Store the error if the request fails
  }, []);

  return (
    <div>
      {error
        ? <p>{error.message}</p>
        : data
          ? <p>Data fetched successfully!</p>
          : "Loading..."}
    </div>
  );
}
```

### 🧠 What happens?

- **Error occurs** → `.catch()` receives it.
- `setError(error)` stores it in state.
- React re-renders and shows the error message.
- If there is no error and data hasn't arrived yet → `"Loading..."`.
- If data arrives → success message.

### HTTP Error Categories

- **400–499** → client-side errors
- **500–599** → server-side errors

---

## 🔄 Retry Mechanism

Sometimes an API fails temporarily. Instead of giving up immediately, we can **try the request again**.

We can keep track of how many attempts have happened with a `retries` state.

```js
function App() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [retries, setRetries] = useState(0);
  // retries keeps track of failed attempts

  useEffect(() => {
    if (retries < 3) {
      // Try the request only while retries are below 3

      fetch("https://api-regional.codesignalcontent.com/posting-application-2/posts/")
        .then((response) => {
          if (!response.ok) {
            throw Error(response.statusText);
          }

          return response.json();
        })
        .then((data) => setData(data))

        .catch((error) => {
          setError(error);
          setRetries(retries + 1);
          // Increase retry count when the request fails
        });
    }
  }, [retries]);
  // Effect runs again whenever retries changes
}
```

### 🔁 How the retry works

If the request fails:

`error → retries increases → useEffect runs again → fetch is tried again`

The condition `retries < 3` prevents unlimited retries.

---

## 🧠 Golden Rule

> **API requests can fail, so always handle the error and give the user a useful fallback instead of leaving them with a broken or empty UI.**

For temporary failures, a **limited retry mechanism** can give the request another chance.

---

## ⚡ Quick Revision

- `error` state → stores API errors.
- `response.ok` → checks whether the response was successful.
- `.catch()` → handles the failed request.
- **Fallback UI** → shows loading, success, or error states.
- `retries` → tracks retry attempts.
- `[retries]` → makes the effect run again when the retry count changes.